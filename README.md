// higher-level DateTime functionality
// e.g., functions that might throw

#pragma once

#include "Platform.h"



class String_;
class DateTime_;

namespace DateTime
{
	bool IsDateTimeString(const String_& src);
	DateTime_ FromString(const String_& src);	// date, space, colon-separated numbers

	double DDate(const DateTime_& t, const DateTime_& from); //Y
	Vector_<> DDate(const Vector_<DateTime_>& dates, const DateTime_& from); //Y

	DateTime_ FromDuration(const DateTime_& start, const double duration); //Y
	Vector_<DateTime_> FromDurations(const DateTime_& start, const Vector_<>& durations); //Y

	DateTime_ AddMSec(const DateTime_& t, long long msec);
}
