
inline std::ostream& operator<<(std::ostream& dst, const String_& s) { return dst << reinterpret_cast<const std::string&>(s); }
