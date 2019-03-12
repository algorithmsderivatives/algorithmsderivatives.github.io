
	Equity_::Equity_( 
		const String_& eq_name,
		const Date_* delivery_date,
		const String_* delay_increment)
		: eqName_(eq_name)
	{
		if (delivery_date)
		{
			delivery_ = *delivery_date;
		}
		else if (delay_increment)
		{
			delivery_ = *delay_increment;
		}
		else
		{
			delivery_ = Cell_(); // empty
		}
	}
