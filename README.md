	struct EquityOptionPayout_ : PayoutSimple_ //Y
	{
		Valuation::address_t fixing_;
		double strike_;
		OptionType_ type_;
		int size_; // size is signed : long short x nominal
		dst_t dst_; //Handle_<Payment::Tag_>

		EquityOptionPayout_(
			const String_& name,
			const DateTime_& expiry,
			Valuation::address_t fixing,
			double strike,
			const OptionType_& type,
			int signed_num_contracts,
			const Handle_<Payment::Tag_>& dst)
			: PayoutSimple_(name, expiry), fixing_(fixing),
			strike_(strike), size_(signed_num_contracts), type_(type), dst_(dst) {}

		void DoNode(const UpdateToken_& values, State_* state, NodeValues_& pay) const
		{
			assert(values.eventTime_ == eventTime_);
			const double spot = values[fixing_];
			const double payout = type_.Payout(spot, strike_);
			pay[dst_] += size_*payout;
		}
	};

	Payment_ MakeOptionPayment
		(const DateTime_& expiry,
		const Ccy_& ccy,
		const String_& stream)
	{
		Payment::Info_ info("Equity option delivery", expiry);
		return Payment_(expiry, ccy, expiry.Date(), stream, info);
	}
	
	struct EquityOption_ : Trade_
	{
		Handle_<Index::Equity_> index_;
		DateTime_ expiry_;
		double strike_;
		const OptionType_ type_;
		double size_;	// signed number of contracts

		EquityOption_
			(const String_& trade_name,
			const Handle_<Index::Equity_>& index,
			const Ccy_& ccy,
			const DateTime_& expiry,
			double strike,
			const OptionType_& type,
			int signed_num_contracts,
			const CollateralType_& collateral)
			:
			Trade_(Vector::V1(trade_name), EquityUnderlying(ccy, index, expiry), ccy, collateral),
			index_(index),
			expiry_(expiry),
			strike_(strike),
			type_(type),
			size_(signed_num_contracts)
		{   }

		Payout_* MakePayout
			(const ValuationParameters_&,
			ValueRequest_& mkt)
			const override
		{
			const String_& name = valueNames_[0];
			Handle_<Payment::Tag_> payDst(mkt.PayDst(MakeOptionPayment(expiry_, valueCcy_, name)));
			return new EquityOptionPayout_
				(name, expiry_, mkt.Fixing(expiry_, *index_), strike_, type_, size_, payDst);
		}
	};
