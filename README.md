
#include "Statistics.h"
#include Vector.h
#include String.h


namespace Statistics
{
	Statistics_* NewStdStatistics(const Vector_<String_>& toReport = Vector_<String_>());
}




class StdStatistics_ : public Statistics_
{
public:
    StdStatistics_(const Vector_<String_>& toReport);
    virtual void Add(double result);
    virtual Vector_<pair<String_, double> > Results() const;

	double Mean() const;
	double Variance() const;
	double StdDev() const;
	double StdErr() const;
	double Skewness() const;
	double Kurtosis() const;

private:
    double mean_, M2_, M3_, M4_;
    unsigned long nPaths_;
	const Vector_<String_> toReport_;
};



StdStatistics_::StdStatistics_(const Vector_<String_>& toReport)
    : mean_(0.0), M2_(0.0), M3_(0.0), M4_(0.0),
	nPaths_(0UL), toReport_(toReport) {}


void StdStatistics_::Add(double x)
{
    nPaths_++;
	double delta = x - mean_;
	double delta_n = delta/nPaths_;
	double delta_n2 = delta_n*delta_n;
	double term1 = delta*delta_n*(nPaths_ - 1);
	double inv_n = 1.0/(double)nPaths_;

	double tmp = std::pow(delta, 4)*(1.0 - inv_n)*(1.0 - 3.0*inv_n + 3.0*inv_n*inv_n);
	//M4_ += term1*delta_n2*(nPaths_*nPaths_ - 3.0*nPaths_ + 3.0) + 6.0*delta_n2*M2_ - 4.0*delta_n*M3_; // overflow
	M4_ += tmp + 6.0*delta_n2*M2_ - 4.0*delta_n*M3_; 
	M3_ += term1*delta_n*(nPaths_ - 2.0) - 3.0*delta_n*M2_;
	M2_ += term1;
	mean_ = mean_ + delta_n;
}

double StdStatistics_::Mean() const
{
	REQUIRE(nPaths_ > 0.0, "nPaths_ = 0, unsufficient");
	return mean_;
}

double StdStatistics_::Variance() const
{
	REQUIRE(nPaths_ > 1.0, "nPaths_ <=1, unsufficient");
	double var = M2_/(nPaths_ - 1.0);
	return var;
}

double StdStatistics_::StdDev() const 
{
	return sqrt(Variance());
}

double StdStatistics_::StdErr() const
{
	double var = Variance();
	return sqrt(var/nPaths_);
}

double StdStatistics_::Skewness() const 
{
	// http://en.wikipedia.org/wiki/Skewness
    REQUIRE(nPaths_ > 2, "sample number <=2, unsufficient");
    if (M2_==0.0) return 0.0; //TODO
	double skew = sqrt(nPaths_/M2_)*(M3_/M2_); // g1
    return skew;
}


double StdStatistics_::Kurtosis() const 
{
	// http://en.wikipedia.org/wiki/Kurtosis
    REQUIRE(nPaths_ > 3, "sample number <=3, unsufficient");
    double c2 = 3.0*(nPaths_ - 1.0)/(nPaths_ - 2.0)*(nPaths_ - 1.0)/(nPaths_ - 3.0);
    if (M2_==0) return c2;
	double kurtosis = (nPaths_*M4_) / (M2_*M2_); // g2
	return kurtosis;
}

Vector_<pair<String_, double> > StdStatistics_::Results() const
{
    Vector_<pair<String_, double> > ret;
	if (toReport_.empty())
	{
		ret.push_back(make_pair("Mean", Mean()));
	}
	else
	{
		for (Vector_<String_>::const_iterator it = toReport_.begin(); it != toReport_.end(); ++it)
		{
			if (*it == "MEAN")
			{
				ret.push_back(make_pair("Mean  ", Mean()));
			}
			else if (*it == "STDDEV")
			{
				ret.push_back(make_pair("StdDev", StdDev()));
			}			
			else if (*it == "STDERR")
			{
				ret.push_back(make_pair("StdErr", StdErr()));
			}
			else if (*it == "VARIANCE")
			{
				ret.push_back(make_pair("Var   ", Variance()));
			}
			else if (*it == "SKEWNESS")
			{
				ret.push_back(make_pair("Skew  ", Skewness()));
			}
			else if (*it == "KURTOSIS")
			{
				ret.push_back(make_pair("Kurtos", Kurtosis()));
			}
		}
	}
    return ret;
}



namespace Statistics
{
	Statistics_* NewStdStatistics(const Vector_<String_>& toReport)
	{
		return new StdStatistics_(toReport);
	}
}
