# Mex utilities help developing mex function in Matlab faster.
It is required to install Opencv (any version). The three files `mexopencv.hpp`, `MxArray.cpp`, `MxArray.hpp` have been copied over in [mexopencv](https://github.com/kyamagu/mexopencv).

## How to use
To use mex-utils, we includes header file `mexopencv.hpp`.
```
#include "mexopencv.hpp"
```

## Developing a new MEX function
An example to use MxArray class to develop new mex function.

``` cpp
#include "mexopencv.hpp"
void mexFunction(int nlhs, mxArray *plhs[], int nrhs, const mxArray *prhs[])
{
    // Check arguments
    nargchk (nlhs<=1 && nrhs==1);

    // Convert MxArray to cv::Mat
    cv::Mat mat = MxArray(prhs[0]).toMat();

    // Do whatever you want

    // Convert cv::Mat back to mxArray*
    plhs[0] = MxArray(mat);
}
```

This example simply copies an input to `cv::Mat` object and then copies again
to the output. Notice how the `MxArray` class provided by mexopencv converts
`mxArray` to `cv::Mat` object. Of course you would want to do something more
with the object.

The `mexopencv.hpp` header includes a class `MxArray` to manipulate
`mxArray` objects. Mostly this class is used to convert between OpenCV data
types and `mxArray`.

``` cpp
int i            = MxArray(prhs[0]).toInt();
double d         = MxArray(prhs[0]).toDouble();
bool b           = MxArray(prhs[0]).toBool();
std::string s    = MxArray(prhs[0]).toString();
cv::Mat mat      = MxArray(prhs[0]).toMat();   // For pixels
cv::Mat ndmat    = MxArray(prhs[0]).toMatND(); // For N-D array
cv::Point pt     = MxArray(prhs[0]).toPoint();
cv::Size siz     = MxArray(prhs[0]).toSize();
cv::Rect rct     = MxArray(prhs[0]).toRect();
cv::Scalar sc    = MxArray(prhs[0]).toScalar();
cv::SparseMat sp = MxArray(prhs[0]).toSparseMat(); // Only double to float

plhs[0] = MxArray(i);
plhs[0] = MxArray(d);
plhs[0] = MxArray(b);
plhs[0] = MxArray(s);
plhs[0] = MxArray(mat);
plhs[0] = MxArray(ndmat);
plhs[0] = MxArray(pt);
plhs[0] = MxArray(siz);
plhs[0] = MxArray(rct);
plhs[0] = MxArray(sc);
plhs[0] = MxArray(sp); // Only 2D float to double
```

Check `MxArray.hpp` for the complete list of the conversion API.

