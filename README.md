
#include "Matrix.h"
//#include <Base/String.h>
//#include <Base/Cell.h>
//#include <Base/Exception.h>



struct Cell_;

namespace Matrix
{
	Matrix_<Cell_> Format
		(const Vector_<const Matrix_<Cell_>*>& args,
		const String_& format);

	template<class T_> Matrix_<T_> MakeTranspose(const Matrix_<T_>& src)
	{
		Matrix_<T_> retval(src.Cols(), src.Rows());
		for (int ir = 0; ir < src.Rows(); ++ir)
			Copy(src.Row(ir), &retval.Col(ir));
		return retval;
	}

	template<class E_> void Append(Matrix_<E_>* above, const Matrix_<E_>& below)
	{
		assert(above != nullptr);
		const int offset = above->Rows();
		above->Resize(offset + below.Rows(), Max(above->Cols(), below.Cols()));    // efficient unless below is too wide
		for (int ir = 0; ir < below.Rows(); ++ir)
			Copy(below.Row(ir), &above->Row(ir + offset));
	}

	// make a 1x1 matrix
	template<class E_> Matrix_<E_> M1x1(const E_& src)
	{
		Matrix_<E_> retval(1, 1);
		retval(0, 0) = src;
		return retval;
	}

	// copy/transform all matrix elements; these functions all take advantage of the known layout of matrix!
	template<class E_> typename Matrix_<E_>::Row_::iterator EndElements(Matrix_<E_>& m)
	{
		return m.Row(m.Empty() ? 0 : m.Rows() - 1).end();
	}
	template<class E_> typename Matrix_<E_>::ConstRow_::const_iterator EndElements(const Matrix_<E_>& m)
	{
		return m.Row(m.Empty() ? 0 : m.Rows() - 1).end();
	}

	template<class E_, class Op_> void Transform(Matrix_<E_>* container, Op_ op)
	{
		Matrix_<E_>::Row_::iterator b = container->Row(0).begin();
		transform(b, EndElements(*container), b, op);
	}
	template<class E_, class EE_, class Op_> void Transform(Matrix_<E_>* to_modify, const Matrix_<EE_>& other, Op_ op)
	{
		assert(other.Rows() == to_modify->Rows() && other.Cols() == to_modify->Cols());
		Matrix_<E_>::Row_::iterator b = container->Row(0).begin();
		transform(b, EndElements(*to_modify), other.Row(0).begin(), b, op);
	}
	template<class EI_, class EO_, class Op_> void Transform(const Matrix_<EI_>& in, Op_ op, Matrix_<EO_>* out)
	{
		assert(in.Rows() == out->Rows() && in.Cols() == out->Cols());
		transform(in.Row(0).begin(), EndElements(in), out->Row(0).begin(), op);
	}
	template<class E1_, class E2_, class EO_, class Op_> void Transform(const Matrix_<E1_>& in1, const Matrix_<E2_>& in2, Op_ op, Matrix_<EO_>* out)
	{
		assert(in1.Rows() == out->Rows() && in1.Cols() == out->Cols());
		assert(in2.Rows() == out->Rows() && in2.Cols() == out->Cols());
		transform(in1.Row(0).begin(), EndElements(in1), in2.Row(0).begin(), out->Row(0).begin(), op);
	}

	template<class ES_, class ED_> void Copy(const Matrix_<ES_>& src, Matrix_<ED_>* dst)
	{
		assert(dst && src.Rows() == dst->Rows() && src.Cols() == dst->Cols());
		copy(src.Row(0).begin(), EndElements(src), dst->Row(0).begin());
	}
	template<class E_, class Op_> auto Apply(Op_ op, const Matrix_<E_>& src)->Matrix_<decltype(op(src(0, 0)))>
	{
		Matrix_<decltype(op(src(0, 0)))> retval(src.Rows(), src.Cols());
		Transform(src, op, &retval);
		return retval;
	}

	template<class E_> Matrix_<E_> FromVectors(const Vector_<Vector_<E_> >& vv, bool by_cols = false, bool quiet = true, bool ragged = false)
	{
		if (vv.empty())
			return Matrix_<E_>();

		const int size2 = ragged ? *MaxElement(Apply([](const Vector_<E_>& v){ return v.size(); }, vv)) : vv[0].size();
		Matrix_<E_> retval(by_cols ? size2 : vv.size(), by_cols ? vv.size() : size2);
		for (int ii = 0; ii < vv.size(); ++ii)
		{
			REQUIRE(quiet || ragged || vv[ii].size() == size2, "Invalid array sizing");
			if (by_cols)
				copy(vv[ii].begin(), vv[ii].end(), retval.Col(ii).begin());
			else
				copy(vv[ii].begin(), vv[ii].end(), retval.Row(ii).begin());
		}
		return retval;
	}
	template<class E_> Vector_<Vector_<E_> > ToVectors(const Matrix_<E_>& m, bool by_cols = false)
	{
		Vector_<Vector_<E_> > retval(by_cols ? m.Cols() : m.Rows());
		for (int ii = 0; ii < retval.size(); ++ii)
		{
			retval[ii] = by_cols
				? Copy(m.Col(ii))
				: Copy(m.Row(ii));
		}
		return retval;
	}

	//Y
	template<class E_>
	inline Matrix_<E_> FromVectorByRows(size_t rows, size_t columns, const Vector_<E_>& v)
	{
		Matrix_<E_> ret(rows, columns);
		size_t k = 0;
		for (size_t i = 0; i<rows; ++i)
		{
			for (size_t j = 0; j<columns; ++j)
			{
				ret(i, j) = v[k++];
				if (k >= v.size()) k = 0; // allow to duplicate vector by rows
			}
		}
		return ret;
	}

	//Y
	template<class E_>
	inline Matrix_<E_> Constant(size_t rows, size_t columns, const E_& val = E_())
	{
		Matrix_<E_> m(rows, columns);
		m.Fill(val);
		return m;
	}

	bool IsClose(const Matrix_<>& m1, const Matrix_<>& m2, double eps = EPSILON); //Y

}

