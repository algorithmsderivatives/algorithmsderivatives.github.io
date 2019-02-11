
#pragma once
#include "Random.h"


namespace Random
{
	Random_* NewBoostMersenneTwister(unsigned long seed);
}




#include "BoostMT.h"
#include <boost/random.hpp>



namespace
{
	class BoostMT_ : public Random_ 
	{
		boost::mt19937 generator_;
		boost::random::uniform_real_distribution<> uni_dist_;
		boost::variate_generator<boost::mt19937&, boost::random::uniform_real_distribution<> > uni_;

	public:
		BoostMT_(unsigned long seed)
		: generator_(seed),
		  uni_dist_(0, 1),
		  uni_(generator_, uni_dist_) {}
			
		double NextUniform();

		Random_* Branch(int i_child = 0) const;
	};

	double BoostMT_::NextUniform()
	{
		return uni_();
	}

	Random_* BoostMT_::Branch(int i_child) const
	{
		return 0; //TODO
	}
} //namespace

Random_* Random::NewBoostMersenneTwister(unsigned long seed)
{
	return new BoostMT_(seed);
}

