
ValuationParameters_::ValuationParameters_(const Dictionary_& src)
	:
method_(Dictionary::Extract(src, "METHOD", std::ptr_fun(Cell::ToEnum<ValuationMethod_>), ValuationMethod_())), 
nPaths_(Dictionary::Extract(src, "NPATHS", std::ptr_fun(Cell::ToInt), 5000)), 
valueCcy_(Dictionary::Extract(src, "VALUECCY", std::ptr_fun(Cell::ToEnum<Ccy_>), Ccy_())), 
nXSizePDE_(Dictionary::Extract(src, "NXSIZEPDE", std::ptr_fun(Cell::ToInt), 51)), 
nTSizePDE_(Dictionary::Extract(src, "NTSIZEPDE", std::ptr_fun(Cell::ToInt), 10)), 
nSigmasPDE_(Dictionary::Extract(src, "NSIGMASPDE", std::ptr_fun(Cell::ToDouble), 5)), 
thetaPDE_(Dictionary::Extract(src, "THETAPDE", std::ptr_fun(Cell::ToDouble), 0.5)), 
boundLowTypePDE_(Dictionary::Extract(src, "BOUNDLOWTYPEPDE", std::ptr_fun(Cell::ToEnum<BoundaryTypePDE_>), BoundaryTypePDE_())), 
boundLowXPDE_(Dictionary::Extract(src, "BOUNDLOWXPDE", std::ptr_fun(Cell::ToDouble), 0)), 
boundLowValPDE_(Dictionary::Extract(src, "BOUNDLOWVALPDE", std::ptr_fun(Cell::ToDouble), 0)), 
boundHighTypePDE_(Dictionary::Extract(src, "BOUNDHIGHTYPEPDE", std::ptr_fun(Cell::ToEnum<BoundaryTypePDE_>), BoundaryTypePDE_())), 
boundHighXPDE_(Dictionary::Extract(src, "BOUNDHIGHXPDE", std::ptr_fun(Cell::ToDouble), 0)), 
boundHighValPDE_(Dictionary::Extract(src, "BOUNDHIGHVALPDE", std::ptr_fun(Cell::ToDouble), 0))
{	
}
Dictionary_ ValuationParameters_::Data() const
{
	Dictionary_ retval;	
	if (method_ != ValuationMethod_())
		retval.Insert("METHOD", Cell::FromEnum<ValuationMethod_>(method_));
	retval.Insert("NPATHS", Cell::FromInt(nPaths_));
	retval.Insert("VALUECCY", Cell::FromEnum<Ccy_>(valueCcy_));
	retval.Insert("NXSIZEPDE", Cell::FromInt(nXSizePDE_));
	retval.Insert("NTSIZEPDE", Cell::FromInt(nTSizePDE_));
	retval.Insert("NSIGMASPDE", Cell::FromDouble(nSigmasPDE_));
	retval.Insert("THETAPDE", Cell::FromDouble(thetaPDE_));
	retval.Insert("BOUNDLOWTYPEPDE", Cell::FromEnum<BoundaryTypePDE_>(boundLowTypePDE_));
	retval.Insert("BOUNDLOWXPDE", Cell::FromDouble(boundLowXPDE_));
	retval.Insert("BOUNDLOWVALPDE", Cell::FromDouble(boundLowValPDE_));
	retval.Insert("BOUNDHIGHTYPEPDE", Cell::FromEnum<BoundaryTypePDE_>(boundHighTypePDE_));
	retval.Insert("BOUNDHIGHXPDE", Cell::FromDouble(boundHighXPDE_));
	retval.Insert("BOUNDHIGHVALPDE", Cell::FromDouble(boundHighValPDE_));
	return retval;
}
