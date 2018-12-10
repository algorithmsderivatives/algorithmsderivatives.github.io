
#include "Date.h"


class String_;
class Date_;

namespace Date
{
	bool IsDateString(const String_& src);	// predicts whether FromString will work -- examines format only
	Date_ FromString(const String_& src);	// tries our best to recognize the string -- rejects both mm/dd/yyyy and dd/mm/yyyy due to ambiguity
	int MonthFromFutureCode(char code);

	double DDate(const Date_& t, const Date_& from); //Y
	Vector_<> DDate(const Vector_<Date_>& dates, const Date_& from); //Y
} // Date
