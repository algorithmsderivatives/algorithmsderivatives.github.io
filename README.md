
#include "Random.h"

void Random_::FillUniform(Vector_<> *deviates)
{
	for (Vector_<>::iterator pd = deviates->begin(); pd != deviates->end(); ++pd)
		*pd = NextUniform();
}
