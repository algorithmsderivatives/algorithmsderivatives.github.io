
bool operator==(const ValuationMethod_& lhs, const ValuationMethod_& rhs) {return lhs.val_ == rhs.val_;}
namespace
{
    Vector_<ValuationMethod_>& TheValuationMethodList()
    {
        RETURN_STATIC(Vector_<ValuationMethod_>);
    }
}    // leave local


Vector_<ValuationMethod_> ValuationMethodListAll()
{
   if (TheValuationMethodList().empty())
   {
     TheValuationMethodList().emplace_back(ValuationMethod_::Value_::CLOSED_FORM);
     TheValuationMethodList().emplace_back(ValuationMethod_::Value_::MONTE_CARLO);
     TheValuationMethodList().emplace_back(ValuationMethod_::Value_::PDE);
   }
   return TheValuationMethodList();
}


const char* ValuationMethod_::String() const
{
    switch (val_)
    {
    default:
	
        return 0;
		case Value_::CLOSED_FORM:
		return "CLOSED_FORM";
	case Value_::MONTE_CARLO:
		return "MONTE_CARLO";
	case Value_::PDE:
		return "PDE";
		
    }}
struct ReadStringValuationMethod_
{
	ReadStringValuationMethod_() {}

    bool operator()(const String_& src, ValuationMethod_::Value_* val) const    // returns true iff recognized input
    {
        bool retval = true;
		if (0);	// otiose code to allow regular else-if structure
		else if (src.empty())
			{retval = true;*val = ValuationMethod_::Value_::CLOSED_FORM;}
		
	else if (String::Equivalent(src, "CLOSEDFORM"))
		*val = ValuationMethod_::Value_::CLOSED_FORM;
	else if (String::Equivalent(src, "CF"))
		*val = ValuationMethod_::Value_::CLOSED_FORM;

	else if (String::Equivalent(src, "MONTECARLO"))
		*val = ValuationMethod_::Value_::MONTE_CARLO;
	else if (String::Equivalent(src, "MC"))
		*val = ValuationMethod_::Value_::MONTE_CARLO;

	else if (String::Equivalent(src, "PDE"))
		*val = ValuationMethod_::Value_::PDE;
        else
            retval = false;
        return retval;
    }
};

ValuationMethod_::ValuationMethod_(const String_& src)
{
    static const ReadStringValuationMethod_ READ_FIXED;    // allows precomputation for speed, in constructor
    if (READ_FIXED(src, &val_))
        return;
   throw Exception_("'" + src + "' is not a recognizable ValuationMethod"); 
}
