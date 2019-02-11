
class  ValuationMethod_
{
public:
   enum class Value_ : char
   {
      		CLOSED_FORM,
		MONTE_CARLO,
		PDE,
      _N_VALUES
   } val_;
      
    ValuationMethod_(Value_ val) : val_(val) {assert(val < Value_::_N_VALUES);}
private:
    friend  bool operator==(const ValuationMethod_& lhs, const ValuationMethod_& rhs);
	friend struct ReadStringValuationMethod_;
    friend Vector_<ValuationMethod_> ValuationMethodListAll();
	friend bool operator<(const ValuationMethod_& lhs, const ValuationMethod_& rhs){return lhs.val_ < rhs.val_;}
public:
    explicit ValuationMethod_(const String_& src);
    const char* String() const;
   Value_ Switch() const {return val_;}
	ValuationMethod_() : val_(Value_::CLOSED_FORM) {};
};
Vector_<ValuationMethod_> ValuationMethodListAll();

 bool operator==(const ValuationMethod_& lhs, const ValuationMethod_& rhs); 
inline bool operator!=(const ValuationMethod_& lhs, const ValuationMethod_& rhs) {return !(lhs == rhs);}
inline bool operator==(const ValuationMethod_& lhs, ValuationMethod_::Value_ rhs) {return lhs.Switch() == rhs;}
inline bool operator!=(const ValuationMethod_& lhs, ValuationMethod_::Value_ rhs) {return lhs.Switch() != rhs;}
