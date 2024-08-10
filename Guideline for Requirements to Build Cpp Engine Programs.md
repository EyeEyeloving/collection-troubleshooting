# Guideline for Requirements to Build Cpp Engine Programs

The purpose of this document is to provide a guideline for the property configuration of the external files involving header files and libraries in a solution based on ***Microsoft Visual Studio***.

> To use an integrated development environment (IDE) such as **Microsoft** **Visual Studio** or **Xcode** to write your source code, set up your environment for building C++ engine applications using the following libraries and include files. Engine applications require the engine library `libMatlabEngine`, the MATLAB Data Array library `libMatlabDataArray`, and supporting include files. ------- < [Requirements to Build C++ Engine Programs - MATLAB &amp; Simulink (mathworks.com)](https://www.mathworks.com/help/matlab/matlab_external/build-c-engine-programs.html) >

You will find static libraries that need to be imported manually:

> - Engine library : `matlabroot` \extern\lib\win64\ `complier` \libMatlabEngine.lib
> - MATLAB Data Array library : `matlabroot` \extern\lib\win64\ `complier` \libMatlabDataArray.lib

where `matlabroot` is the installation path of MATLAB, and `complier` is one of **microsoft** and **mingw64**.

- In the **configuration properties** -> **C/C++** menu bar -> **General** tab, you need to add the necessary header file retrieval address in the **Additional Include Directories** : `matlabroot` \extern\include\ .
- In the **configuration properties** -> **Linker** menu bar -> **General** tab, you need to add the necessary header file retrieval address in the **Additional Include Directories** : `matlabroot` \extern\\lib\win64\ `complier`  .
- In the **configuration properties** -> **Linker** menu bar -> **Input** tab, you need to add `libMatlabEngine.lib` and `libMatlabDataArray.lib` in `Additional Dependencies`.

Lastly, you may also need to set environment variables to provide the index address of the dynamic library (see Troubleshooting for details).

## Testing

[Requirements to Build C++ Engine Programs - MATLAB &amp;&amp; Simulink (mathworks.com)](https://www.mathworks.com/help/matlab/matlab_external/build-c-engine-programs.html)

```cpp
#include "MatlabDataArray.hpp"
#include "MatlabEngine.hpp"
#include <iostream>
void callSQRT() {

    using namespace matlab::engine;

    // Start MATLAB engine synchronously
    std::unique_ptr<MATLABEngine> matlabPtr = startMATLAB();

    //Create MATLAB data array factory
    matlab::data::ArrayFactory factory;

    // Define a four-element typed array
    matlab::data::TypedArray<double> const argArray =
        factory.createArray({ 1,4 }, { -2.0, 2.0, 6.0, 8.0 });

    // Call MATLAB sqrt function on the data array
    matlab::data::Array const results = matlabPtr->feval(u"sqrt", argArray);

    // Display results
    for (int i = 0; i < results.getNumberOfElements(); i++) {
        double a = argArray[i];
        std::complex<double> v = results[i];
        double realPart = v.real();
        double imgPart = v.imag();
        std::cout << "Square root of " << a << " is " <<
            realPart << " + " << imgPart << "i" << std::endl;
    }
}

int main() {
    callSQRT();
    return 0;
}
```

## Troubleshooting

The compilation result indicates that dynamic libraries such as `libMatlabEngine.dll` cannot be found. The introduction of dynamic libraries requires placing files with the suffix `dll` in the directory where the `exe` is located - however, this is not realistic. Therefore, you also need to add the address of `matlabroot\extern\bin\win64\` to the **System Environment Variables** (you may need to restart the computer to activate this setting).
