	void PrintResult(const String_& title, double result, double target)
	{
		cout << title << (AP::IsClose(result, target, AP::BIG_EPSILON) ? "ok" : "not ok") << endl;
	}

	void PrintMCResults(
		const Vector_<pair<String_, double> >& vals,
		const boost::optional<double>& ref_price, double spot)
	{
		map<long, double> outputs;
		for (Vector_<pair<String_, double> >::const_iterator it = vals.begin(); it != vals.end(); ++it)
		{
			Vector_<String_> labels = String::Split(it->first, '-', false); // cf statistics
			long nSim = labels.size() >=2 ? String::ToInt(labels[1]) : 0; //TODO
			outputs[nSim] = it->second;		
		}

		for (map<long, double>::const_iterator it = outputs.begin(); it != outputs.end(); ++it)
		{
			//if (ref_price.is_initialized())
			//	cout << it->first << " : " << (it->second - ref_price.value())*10000.0/spot << endl;
			//else
				cout << it->first << " : " << it->second << endl;
		}
	}
