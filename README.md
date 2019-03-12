
template<class C1_, class C2_> auto Zip(const C1_& v1, const C2_& v2) -> Vector_<decltype(make_pair(v1.front(), v2.front()))>
{
	auto join = []
	(const typename C1_::value_type& e1, const typename C2_::value_type& e2) 
	{return make_pair(e1, e2); };
	return Apply(join, v1, v2);
}

