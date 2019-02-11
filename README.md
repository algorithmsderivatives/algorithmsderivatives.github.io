
#include "ValuationParameters.h"
#include Exception.h
#include StringUtils.h



namespace Valuation
{
	Parameters_::Parameters_(const Dictionary_& src)
		: mc_(src), pde_(src), analyticResult_("NPV")
	{
		REQUIRE(src.Has(Names::METHOD), "valuation method missing");
		method_ = ValuationMethod_(Cell::OwnString(src.At(Names::METHOD)));
		REQUIRE(src.Has(Names::CURRENCY), "valuation currency missing");
		valueCcy_ = Ccy_(Cell::OwnString(src.At(Names::CURRENCY)));
	}

	MonteCarlo::Parameters_::Parameters_(const Dictionary_& src)
	{
		nPaths_ = String::ToInt(Cell::OwnString(src.At(Names::MC_NPATHS)));
		statistics_name_ = Cell::OwnString(src.At(Names::MC_STATISTICS_NAME));
	}

	PDE::Parameters_::Parameters_(const Dictionary_& src)
		: fullGrid_(false)
	{
		nTimeSteps_ = String::ToInt(Cell::OwnString(src.At(Names::PDE_NTIMESTEPS)));
		nSpaceSteps_ = String::ToInt(Cell::OwnString(src.At(Names::PDE_NSPACESTEPS)));
		nSigmas_ = String::ToInt(Cell::OwnString(src.At(Names::PDE_NSIGMAS)));
		theta_ = String::ToDouble(Cell::OwnString(src.At(Names::PDE_THETA)));
	}
}
