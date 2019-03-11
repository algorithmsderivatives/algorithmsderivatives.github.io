	EquityOptionData_* EquityOption::NewData(const Trade_& trade)
{
	DYN_PTR(myTrade, const EquityOption_, &trade);
	if (!myTrade) return 0;
	return new EquityOptionData_(
		myTrade->valueCcy_, myTrade->index_->eqName_,
		myTrade->expiry_, myTrade->expiry_.Date(),
		myTrade->strike_, myTrade->type_);
}

namespace
{
	struct EquityTradeData_ : public TradeData_ //Y
	{
		String_ eq_name_;
		Ccy_ ccy_;
		DateTime_ expiry_;

		EquityTradeData_(
			const String_& trade_name,
			const String_& eq_name,
			const Ccy_& ccy,
			const DateTime_& expiry)
			: TradeData_(trade_name),
			eq_name_(eq_name), ccy_(ccy), expiry_(expiry) {}

		void Write(Archive::Store_& dst) const override
		{
			//TODO
		}
	};
} //namespace

namespace
{
	struct EquityOptionTradeData_ : public EquityTradeData_ //Y
	{
		double strike_;
		int size_;
		OptionType_ type_;
		CollateralType_ collateral_;

		EquityOptionTradeData_(
			const String_& trade_name,
			const String_& eq_name,
			const Ccy_& ccy,
			const DateTime_& expiry,
			double strike,
			int size,
			const OptionType_& type,
			const CollateralType_& collateral)
			: EquityTradeData_(trade_name, eq_name, ccy, expiry),
			strike_(strike), size_(size), type_(type), collateral_(collateral_) {}

		Trade_ * XParse() const
		{
			//TOCHECK : name_ de Storable_
			Handle_<Index::Equity_> index(new Index::Equity_(eq_name_));
			return new EquityOption_(name_, index, ccy_, expiry_, strike_, type_, size_ , collateral_);
		}

		void Write(Archive::Store_& dst) const override
		{
			//TODO
		}
	};
} //namespace

namespace EquityOption //Y
{
	TradeData_* NewTradeData(
		const String_& trade_name,
		const String_& eq_name,
		const Ccy_& ccy,
		const DateTime_& expiry,
		double strike,
		int sign,
		const OptionType_& type,
		const CollateralType_& collateral)
	{
		return new EquityOptionTradeData_(trade_name, eq_name, ccy, expiry, strike, sign, type, collateral);
	}
}
