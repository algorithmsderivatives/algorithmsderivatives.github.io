
#include String.h
#include Vector.h

class Statistics_
{
public:
    Statistics_() {}
	virtual ~Statistics_() {}
    virtual void Add(double result) = 0;
    virtual void Add(const Vector_<>& results);
	virtual Vector_<std::pair<String_, double> > Results() const = 0;
	virtual std::string ToString(const std::string& indent = "", const std::string& nextIndent = "") const;
};




#include "Statistics.h"
#include StringUtils.h
#include Environment.h
#include <sstream>
using namespace std;


void Statistics_::Add(const Vector_<>& results)
{
	for (Vector_<>::const_iterator it = results.begin(); it != results.end(); ++it)
	{
		Add(*it);
	}
}


string Statistics_::ToString(const string& indent, const string& nextIndent) const
{
	stringstream sstr;
	Vector_<pair<String_, double> > results = Results();
	for (unsigned long i=0; i < results.size(); i++)
	{
		sstr << results[i].first << " : " << String::FromDouble(results[i].second) << endl;
	}
	return sstr.str();
}

