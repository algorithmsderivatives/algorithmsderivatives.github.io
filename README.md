//#include "Trade.h"
//#include "Model.h"
//#include "Valuation.h"
//#include "ValuationParameters.h"


namespace MonteCarlo
{
	ReEvaluator_* NewSolver(_ENV, 
		const Trade_& trade, 
		const Model_& model, 
		const ValuationParameters_& params);

}
