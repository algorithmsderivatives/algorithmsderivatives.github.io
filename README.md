	shared_ptr<ValueRequestImp_> myRequest = std::dynamic_pointer_cast<ValueRequestImp_>(request);
	REQUIRE(myRequest, "wrong store type")
	shared_ptr<ValuesStoreImp_> myStore = std::dynamic_pointer_cast<ValuesStoreImp_>(store);
	REQUIRE(myStore, "wrong store type")
