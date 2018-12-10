//#include "Platform.h"
#include "DateTimeUtils.h"
//#include "Strict.h"

#include "Algorithms.h"
#include "DateTime.h"
#include "DateUtils.h"
#include "Exception.h"



bool DateTime::IsDateTimeString(const String_& src)
{
	// I fear this is going to be a performance bottleneck
	auto space = src.find(' ');
	if (space == String_::npos)
		return false;
	if (!Date::IsDateString(src.substr(0, space)))
		return false;
	// accept anything with a date, space, something, colon, something
	auto colon = src.find(':', space);
	return colon != String_::npos
			&& colon > space + 1
			&& colon + 1 < src.size();
}

DateTime_ DateTime::FromString(const String_& src)
{
	NOTICE(src);
	auto space = src.find(' ');
	const Date_ date = Date::FromString(src.substr(0, space));
	if (space == String_::npos)
		return DateTime_(date, 0);
	// split remainder on ':'
	Vector_<String_> tParts = String::Split(src.substr(space + 1), ':', true);
	REQUIRE(tParts.size() >= 2 && tParts.size() <= 3, "Expected hh:mm or hh:mm:ss time");
	Vector_<int> hms = Apply(String::ToInt, tParts);
	REQUIRE(*MinElement(hms) >= 0 && hms[0] < 24 && *MaxElement(hms) < 60, "Hour/minute/second out of bounds");
	return DateTime_(date, hms[0], hms[1], hms.size() > 2 ? hms[2] : 0);
}


double DateTime::DDate(const DateTime_& t, const DateTime_& from) //Y
{
	return (NumericValueOf(t) - NumericValueOf(from)) / 365.0;
}

namespace
{
	struct DDate_ : std::unary_function<DateTime_, double> //Y
	{
		const DateTime_ from_;
		DDate_(const DateTime_& from) : from_(from) {}
		double operator()(const DateTime_& t)
		{
			return DateTime::DDate(t, from_);
		}
	};
}
Vector_<> DateTime::DDate(const Vector_<DateTime_>& dates, const DateTime_& from) //Y
{
	return Apply(DDate_(from), dates);
}

DateTime_ DateTime::FromDuration(const DateTime_& start, const double duration) //Y
{
	return DateTime_(start.Date().AddDays(static_cast<int>(365 * duration + 0.5)));
}

Vector_<DateTime_> DateTime::FromDurations(const DateTime_& start, const Vector_<>& durations) //Y
{
	//return Apply(boost::bind(&Time::FromDuration, start, _1), durations);
	return Apply([&](const double duration){ return FromDuration(start, duration); }, durations);
}

DateTime_ DateTime::AddMSec(const DateTime_& t, long long msec)
{
	long long msecOld = DateTime::MSec(t);
	msec += msecOld;
	return DateTime_(msec);
}
