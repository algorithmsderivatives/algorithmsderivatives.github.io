Trade_* NewEquityForward(const String_& trade_name,
						const Handle_<Index::Equity_>& index,
						const Ccy_& ccy,
						const DateTime_& expiry,
						double strike,
						int signed_num_contracts,
						const CollateralType_& collateral)
{
	return new EquityForward_(trade_name, index, ccy, expiry, strike, signed_num_contracts, collateral);
}
