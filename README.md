namespace
{
	class SimpleBlackModel_ : public Model_, public HasAnalyticEquity_, public HasAnalyticEquityBarrier_
	{
		Handle_<YieldCurve_> yc_;
		const double spot_;
		Handle_<Dividends_> div_;
		Handle_<BSImp::Vol_> vol_;
	public:
		SimpleBlackModel_(
			const String_& modelName,
			const Handle_<YieldCurve_>& yc,
			double spot,
			const Handle_<Dividends_> & div,
			const Handle_<BSImp::Vol_>& vol)
			: Model_(modelName), yc_(yc), spot_(spot), div_(div), vol_(vol) {}

		double Price(const EquityOptionData_& opt, const AnalyticResult_& result) const override;

		double Price(const EquityBarrierData_& opt) const override;

		Handle_<SDE_> ForTrade(_ENV, const Underlying_& trade) const override;

		Model_* Mutant_Model
			(const String_* new_name = nullptr,
			const Slide_* slide = nullptr)
			const;

		DateTime_ VolStart() const override
		{
			return DateTime::Minimum(); //TODO
		}

		Handle_<YieldCurve_> YieldCurve(const Ccy_& ccy) const override { return yc_; }

		void Write(Archive::Store_& dst) const override;
	};

	double SimpleBlackModel_::Price(const EquityOptionData_& opt, const AnalyticResult_& result) const
	{
		double forward = Black::ForwardValue(*(YieldCurve(opt.valueCcy_).get()),
			spot_,
			*(div_.get()),
			opt.expiry_);
		return Black::OptionValue(
			*(yc_.get()),
			forward, 
			*(vol_.get()), 
			opt.expiry_,
			opt.delivery_, 
			opt.strike_, 
			opt.type_,
			result);
	}

	double SimpleBlackModel_::Price(const EquityBarrierData_& opt) const
	{	
		return Black::BarrierOptionValue(
			*(yc_.get()),
			spot_, 
			*(div_.get()),
			*(vol_.get()), 
			opt.underlying_.expiry_,
			opt.underlying_.delivery_, 
			opt.underlying_.strike_, 
			opt.underlying_.type_,
			opt.barrier_,
			opt.barrierType_);
	}

	Model_* SimpleBlackModel_::Mutant_Model(const String_* new_name, const Slide_* slide) const //Y
	{
		if (new_name)
		{
			return new SimpleBlackModel_(*new_name, yc_, spot_, div_, vol_);
		}
		else if (slide)
		{
		}
		return new SimpleBlackModel_(*new_name, yc_, spot_, div_, vol_);
	}

	Handle_<SDE_> SimpleBlackModel_::ForTrade(_ENV, const Underlying_& underlying) const
	{
		return Handle_<SDE_>(NewBlackSDE(spot_, yc_, vol_, div_));
	}

	void SimpleBlackModel_::Write(Archive::Store_& dst) const
	{
	}
} //namespace


Model_* NewBlackModel2(
	const String_& modelName,
	const String_& equityName,
	Handle_<YieldCurve_> yc,
	const double spot,
	Handle_<Dividends_> div,
	Handle_<BSImp::Vol_> vol)
{
	return new SimpleBlackModel_(modelName, yc, spot, div, vol);
}
