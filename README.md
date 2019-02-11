namespace MonteCarlo
{

	///////////////////////
	//
	//		Average Values
	//
	///////////////////////

	struct XAverage_
	{
		map<String_, Vector_<pair<String_, double> > > values_;

		XAverage_(
			map<String_, Vector_<double> >& values, // const Stream x MC values
			const String_& statName,
			const Vector_<String_>& toReport)
		{
			typedef map<String_, Vector_<double> > vals_t;
			for (vals_t::const_iterator iv = values.begin(); iv != values.end(); ++iv)
			{
				//values_[iv->first] = Average(iv->second);
				unique_ptr<Statistics_> statistics(Statistics::MakeStatistics(statName, toReport));
				statistics->Add(iv->second);
				Vector_<pair<String_, double> > results = statistics->Results();
				values_[iv->first] = results;
			}
		}
	};

	struct XToTradeValues_
	{
		typedef map<String_, Vector_<pair<String_, double> > > weights_t; // Trade x Stream x Weight
		typedef map<String_, Vector_<pair<String_, double> > > streams_t; // Stream x Statistic x Value

		Vector_<pair<String_, double> > values_;

		XToTradeValues_(const weights_t& tradeWeights, streams_t&  streamValues, Ccy_ valueCcy)
		{
			map<String_, double> tradeVals;
			for (weights_t::const_iterator it = tradeWeights.begin(); it != tradeWeights.end(); ++it)
			{
				const String_& trade = it->first;
				const Vector_<pair<String_, double> >& streamWeights = it->second;
				for (Vector_<pair<String_, double> >::const_iterator is = streamWeights.begin(); is != streamWeights.end(); ++is)
				{
					const String_& stream = is->first;
					const double weight = is->second;

					String_ ccyStream = String_(valueCcy.String()) + "~" + stream; // TODO

					const Vector_<pair<String_, double> >& values = streamValues[ccyStream];
					for (Vector_<pair<String_, double> >::const_iterator iv = values.begin(); iv != values.end(); ++iv)
					{
						//TODO : StdErr ... n'est pas additif !!!
						tradeVals[trade + ":" + iv->first] += weight * iv->second;
					}
				}
			}
			values_ = Zip(Keys(tradeVals), MapValues(tradeVals));
		}
	};


	struct StepInfo_
	{
		DateTime_ from_;
		DateTime_ to_;
		Handle_<ModelStepper_> stepper_;
		std::list<DiscountInfo_> discountInfos_;

		StepInfo_(const DateTime_& from, const DateTime_& to, 
			Handle_<ModelStepper_> stepper, const std::list<DiscountInfo_>& discountInfos = std::list<DiscountInfo_>())
			: from_(from), to_(to), stepper_(stepper), discountInfos_(discountInfos) {}		
	};

	void MakeStepInfos(
		const SDE_& sde,
		const DateTime_& vol_start,
		const std::map<DateTime_, Vector_<DiscountInfo_> >& paymentInfos,
		StepAccumulator_* cumulative,
		Vector_<Handle_<ModelStepper_> >* exemplars,
		// out
		std::map<DateTime_, shared_ptr<StepInfo_> >* stepInfos
		)
	{
		typedef std::map<DateTime_, Vector_<DiscountInfo_> > paymentmap_t;
		typedef std::map<DateTime_, Vector_<shared_ptr<StepInfo_> > > stepmap_t;

		Vector_<DateTime_> eventTimes = Keys(paymentInfos);
		// create step infos
		DateTime_ from = vol_start;
		for (int j = 0; j<eventTimes.size(); j++)
		{
			DateTime_ to = eventTimes[j];
			if (from < to)
			{
				const ModelStepper_* exemplar = exemplars ? (*exemplars)[j].get() : 0;
				Handle_<ModelStepper_> stepper(sde.NewStepper(from, to, cumulative, exemplar));
				shared_ptr<StepInfo_> info(new StepInfo_(from, to, stepper));
				(*stepInfos)[from] = info;
			}
			from = to;
		}
		// dispatch payments
		for (auto& kv : paymentInfos)
		{
			for (auto& info : kv.second)
			{
				auto it = stepInfos->lower_bound(DateTime_(info.date_));
				if (it == stepInfos->begin())
				{
					double test = 0;
				}
				else
				{
					--it;
					it->second->discountInfos_.push_back(info);
				}
			}
		}
	}



	///////////////////////
	//
	//		Value Monte Carlo
	//
	///////////////////////


	size_t DimensionMonteCarlo(const std::map<DateTime_, shared_ptr<StepInfo_> >& stepInfos)
	{
		size_t ret = 0;
		for (auto& kv : stepInfos)
		{
			const StepInfo_& stepInfo = *kv.second;
			Handle_<ModelStepper_> stepper = stepInfo.stepper_;
			ret += stepper->NumGaussians();
		}
		return ret;
	}

	Vector_<pair<String_, double> > ValueMonteCarlo(_ENV, 
		const SDE_& model,
		ValueRequest_& request, 
		const Payout_& payout, 
		const ValuationParameters_& params,
		Vector_<Handle_<ModelStepper_> >* steps,
		StepAccumulator_* base_cumulant, 
		Vector_<Handle_<ModelStepper_> >* base_steps,
		const MonteCarlo::PathsRecord_* base_paths,
		shared_ptr<ValuesStore_> store)
	{
		const Date_ volStart = Global::Dates_().AccountingDate();

		shared_ptr<MonteCarloValues_> nodes(NewMonteCarloValues(params.nPaths_));

		auto myRequest = dynamic_cast<const MonteCarloRequest_*>(&request);
		auto& paymentInfos = myRequest->infos_;
			
		// 6 
		unique_ptr<Asset_> asset(model.NewAsset(request)); //TODO
		REQUIRE(asset.get(), "Invalid asset model");

		std::map<DateTime_, shared_ptr<StepInfo_> > stepInfos;
		MakeStepInfos(model, DateTime_(volStart), paymentInfos, base_cumulant, base_steps, &stepInfos);

		//paths_(cumulant_->NewPathsRecord(params.nPaths_, );

		// MakeCorrelated

		MonteCarlo::Workspace_* workspace = 0;

		size_t dimension = DimensionMonteCarlo(stepInfos);

		//unique_ptr<Random_> rand(Random::NewGenerator("PARK_MILLER", -842150451));
		unique_ptr<Random_> rand(Random::NewGenerator("BOOST_MT", 42));
		//unique_ptr<Random_> rand(Random::NewGenerator("BOOST_MT", -842150451));

		Random_* more_randoms = 0;
		Vector_<Handle_<DefaultEvent_> >* defaults = 0;

		Payout_::State_* state = payout.NewState();

		// p212 (th2) Envelope is also called at the vol start time to find the initial state variables
		double nSigma = 5.0;
		Vector_<pair<double, double> > envelope = base_cumulant->Envelope(DateTime_(volStart), nSigma);
		Vector_<> modelState(envelope.size(), 0.0); // assume model states start from 0

		Vector_<> deviates(dimension);

		// Boucle sur les chemins
		for (unsigned long iPath=0; iPath<params.nPaths_; ++iPath)
		{
			nodes->SetPath(iPath);
			// p 202
			// The operator= member of Payout_::State_ lets us initialize the state appropriately.
			// state = ???;
			modelState.Fill(0); //TODO

			payout.StartPath(state); //TOCHECK

			double rolling_df = 1;

			rand->FillNormal(&deviates);

			Vector_<>::const_iterator devIt = deviates.begin();
			for (auto& kv : stepInfos)
			{
				const StepInfo_& stepInfo = *kv.second;
				const DateTime_& eventTime = stepInfo.to_;
				Handle_<ModelStepper_> stepper = stepInfo.stepper_;
				//cout << "event_time : " << DateTime::ToString(event_time) << endl;
				//int dim = stepper->NumGaussians();
				//deviates.Resize(dim);
				//rand->FillNormal(&deviates);

				//MonteCarlo::Workspace_* workspace = stepper->NewWorkspace(); //TOCHECK

				// 1 - The solver invokes the stepper to change the model state.
				stepper->Step(devIt, &modelState, workspace, more_randoms, &rolling_df, defaults);

				// 2 - The solver sends the state variables to the asset.
				// 3 - The asset updates index values and index paths, and returns an update token.
				UpdateToken_ token = asset->Update(eventTime, modelState);

				// df
				for (auto& discountInfo : stepInfo.discountInfos_)
				{
					const double PtT = token[discountInfo.loc_];
					const double df = rolling_df * PtT;
					nodes->SetDiscountFactor(*discountInfo.tag_, df);
				}
					
				// 4 - The solver invokes DoDefault as necessary, then DoNode, using this token.
				// NodeValuesDefault_
				// payout_->DoDefault : TODO
				// 5 - The payout uses index values and paths to compute payment amounts which it adds to the node values.
				//
				// p175 the model must compute index values from the state variables at a node
				payout.DoNode(token, state, *nodes);

				devIt += stepper->NumGaussians();
			} // end loop event times
				
		}  // end loop on paths
			
		// 10 - Backward induction actions are applied to the whole path set. 
		// These change values by creating new nodes within their streams.
		Vector_<BackwardInduction::Action_> actions = payout.BackwardSteps();

		// 11 - The node values are discounted, averaged across paths, and summed to produce stream values.
		map<String_, Vector_<double> > discountedNodes = store->DiscountNodes();

		Vector_<String_> to_report_;
		to_report_.push_back("MEAN");
		XAverage_ average(discountedNodes, "MEANVAR", to_report_);

		// 12 -  The payout provides the weights to translate to trade values
		map<String_, Vector_<pair<String_, double> > > weights = payout.StreamWeights();
		XToTradeValues_ toTrade(weights, average.values_, params.valueCcy_);
		return toTrade.values_;
	}

	///////////////////////
	//
	//		Task_
	//
	///////////////////////

	// p208
	class Task_ : public ReEvaluator_
	{
		unique_ptr<ValueRequest_> request_;
		unique_ptr<const Payout_> payout_;

		unique_ptr<StepAccumulator_> cumulant_;
		Vector_<Handle_<ModelStepper_> > steps_;
		unique_ptr<MonteCarlo::PathsRecord_> paths_;

	public:
		// The request and payout are orphan pointers, whose memory belongs to the MC_ once the constructor is called.
		// We pass the request and payout separately, rather than have the Task_ call Trade_::Payout, in order to avoid
		// a compilation dependence of Monte Carlo tasks on Trade_s; the two should be separate and equal.
		Task_(_ENV, 
			const SDE_& model, 
			ValueRequest_* request,
			const Payout_* payout, 
			const ValuationParameters_& params, //Y
			shared_ptr<ValuesStore_> store) //Y
			: request_(request), payout_(payout),
			cumulant_(model.NewAccumulator()) //Y
		{
			const Date_ volStart = Global::Dates_().AccountingDate();
			REQUIRE(cumulant_, "accumulator is empty")
			paths_.reset(cumulant_->NewPathsRecord(params.nPaths_, shared_ptr<MonteCarlo::PathsRecord_>()));

			// steps_ : TODO

			baseVals_ = ValueMonteCarlo(_env, 
							model,
							*request, 
							*payout_, 
							params,
							&steps_,
							cumulant_.get(),
							0,
							paths_.get(),
							store);
		}

		Vector_<pair<String_, double> > Values(_ENV, const Model_* bumped_model = 0) const
		{
//p201
//7) The MC solver stores the model's steppers between event times; for the
//bumped run, it supplies the stored stepper from the base run.
			return baseVals_; //TOCHECK
		}
	};

	ReEvaluator_* NewSolver(_ENV, 
		const Trade_& trade, 
		const Model_& model, 
		const ValuationParameters_& params) //TODO 
	{
		// 2 - The model produces an SDE describing the dynamics of the necessary assets.
		Handle_<SDE_> sde = model.ForTrade(_env, trade.underlying_);
		
		// 3 - The SDE produces a Value Request for use by the trade.
		shared_ptr<ValueRequest_> request(sde->NewRequest());

		// 4- The MC solver wraps that value request in another, which will handle requests 
		// from past data and will provide stream locations to which values can be written.
		shared_ptr<ValuesStore_> store(NewValuesStore(params.nPaths_));
		unique_ptr<ValueRequest_> mc_request(NewMonteCarloRequest(request, store));

		//DateTime_ volStart(Environment::AccountingDate()); //TOCHECK
		//unique_ptr<PastAwareRequest_> past_request(NewPastAwareRequest(_env, *mc_request, volStart, volStart.Date()));

		// 5 - The trade produces a payout, filling out the value request in the process.
		unique_ptr<Payout_> payout(trade.MakePayout(params, *mc_request));

		// The MC solver wraps that value request in another, which will handle requests 
		// from past data and will provide stream locations to which values can be written			
		return new Task_(_env, *sde, mc_request.release(), payout.release(), params, store); 
	}

} // namespace
