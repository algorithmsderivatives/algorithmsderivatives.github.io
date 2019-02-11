
#include String.h
#include Vector.h
#include Dictionary.h
#include AnalyticResult.h
#include "Base/Optionals.h"
#include "Currency.h"
#include "ValuationMethod.h"


namespace Valuation
{
	namespace Names //Y
	{
		BAREWORD(CURRENCY);
		BAREWORD(METHOD);

		BAREWORD(MC_NPATHS);
		BAREWORD(MC_STATISTICS_NAME);

		BAREWORD(PDE_NTIMESTEPS);
		BAREWORD(PDE_NSPACESTEPS);
		BAREWORD(PDE_NSIGMAS);
		BAREWORD(PDE_THETA);
	}

	namespace MonteCarlo
	{
		struct Parameters_
		{
			unsigned int nPaths_;
			String_ statistics_name_;
			Vector_<String_> to_report_;

			Parameters_()
				: nPaths_(0), statistics_name_("MEANVAR") {}

			Parameters_(const Dictionary_& src);
		};
	}

	namespace PDE
	{
		struct BoundaryParameters_
		{
			String_ type_;
			boost::optional<double> x_;
			boost::optional<double> val_;

			BoundaryParameters_()
				: type_("NEUMANN") {}

			double getVal(double defaultDirichlet) const
			{
				bool tmp = val_.is_initialized(); //DEBUG
				if (type_ == "DIRICHLET")
					return val_.is_initialized() ? val_.value() : defaultDirichlet;
				else
					return val_.is_initialized() ? val_.value() : 0;
			}
		};

		struct Parameters_
		{
			unsigned int nTimeSteps_;
			unsigned int nSpaceSteps_; //TODO : vector
			unsigned int nSigmas_;
			double theta_;
			bool fullGrid_;
			BoundaryParameters_ low_;  // TODO : vector
			BoundaryParameters_ high_; // TODO : vector
			
			Parameters_()
				: nTimeSteps_(0), nSpaceSteps_(0), nSigmas_(0), theta_(0.5), fullGrid_(false) {}

			Parameters_(const Dictionary_& src);
		};
	} 

	struct Parameters_ //Y
	{
		ValuationMethod_ method_;
		Ccy_ valueCcy_;
		MonteCarlo::Parameters_ mc_;
		PDE::Parameters_ pde_;
		AnalyticResult_ analyticResult_;

		Parameters_(const Ccy_& valueCcy, const String_ analyticResult = String_())
			: valueCcy_(valueCcy), analyticResult_(analyticResult) {}

		Parameters_(const Dictionary_& src);
	};
}
