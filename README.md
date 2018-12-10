
class DateTime_
{
	Date_ date_;
	uint32_t fracInSec_;
public:
	DateTime_() : fracInSec_(0) {}	// midnight on invalid date
	explicit DateTime_(const Date_& date, double frac = 0.0);
	explicit DateTime_(long long msec);	// convert from unix-era time
	DateTime_(const Date_& date, int hour, int minute = 0, int second = 0);
	inline Date_ Date() const { return date_; }
	double Frac() const;
	inline bool operator==(const DateTime_& rhs) const { return date_ == rhs.date_ && fracInSec_ == rhs.fracInSec_; }

	friend std::size_t std::hash<DateTime_>::operator()(const DateTime_& dt) const;
};

double operator-(const DateTime_& lhs, const DateTime_& rhs);
bool operator<(const DateTime_& lhs, const DateTime_& rhs);
inline bool operator>(const DateTime_& lhs, const DateTime_& rhs) { return rhs < lhs; }
inline bool operator<=(const DateTime_& lhs, const DateTime_& rhs) { return !(rhs < lhs); }
inline bool operator>=(const DateTime_& lhs, const DateTime_& rhs) { return !(lhs < rhs); }
inline bool operator!=(const DateTime_& lhs, const DateTime_& rhs) { return !(lhs == rhs); }


namespace DateTime
{
	int Hour(const DateTime_& dt);
	int Minute(const DateTime_& dt);
	String_ TimeString(const DateTime_& dt);
	inline String_ ToString(const DateTime_& dt) { return Date::ToString(dt.Date()) + " " + TimeString(dt); }
	DateTime_ Minimum();
	long long MSec(const DateTime_& dt);
}

// support use in interpolators
inline double NumericValueOf(const DateTime_& src) { return NumericValueOf(src.Date()) + src.Frac(); }
