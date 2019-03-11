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
		const CollateralType_& collateral);

	EquityOptionData_* NewData(const Trade_& trade); //Y
}
