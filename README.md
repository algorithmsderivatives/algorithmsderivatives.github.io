
#include "Base\Dictionary.h"
class UIRow_;
class Storable_;

struct  ValuationParameters_ : StoreAsDictionary_<ValuationParameters_>
{	
	ValuationMethod_ method_;
	int nPaths_;
	Ccy_ valueCcy_;
	int nXSizePDE_;
	int nTSizePDE_;
	double nSigmasPDE_;
	double thetaPDE_;
	BoundaryTypePDE_ boundLowTypePDE_;
	double boundLowXPDE_;
	double boundLowValPDE_;
	BoundaryTypePDE_ boundHighTypePDE_;
	double boundHighXPDE_;
	double boundHighValPDE_;
	ValuationParameters_(const Dictionary_& src);
	ValuationParameters_(const Dictionary_& src, const std::map<String_, Handle_<Storable_>>&) : ValuationParameters_(src) {}
	Dictionary_ Data() const;
};