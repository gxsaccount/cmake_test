# 检查编译标志  
CMake支持尝试使用传递给函数CMAKE_CXX_COMPILER_FLAG的任何标志编译程序。然后将结果存储在你传入的变量中。  

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)  

此示例将尝试使用标志-std=c++11编译程序，并将结果存储在变量COMPILER_SUPPORTS_CXX11中。  

include(CheckCXXCompilerFlag)这一行告诉CMake包含此函数以使其可用。   

# 添加标志  
一旦确定编译是否支持标志，就可以使用标准的cmake方法将该标志添加到目标。在本例中，我们使用CMAKE_CXX_FLAGS将该标志传递到所有目标。  


        if(COMPILER_SUPPORTS_CXX11)#
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
        elseif(COMPILER_SUPPORTS_CXX0X)#
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
        else()
            message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
        endif()

上面的示例只检查编译标志的GCC版本，并支持从C++11回退到标准化前的C++0x标志。在实际使用中，你可能希望检查C14，或者添加对不同编译设置方法的支持，例如-std=gnu11。  

# 构建示例  

        $ mkdir build
        $ cd build

        $ cmake ..
        -- The C compiler identification is GNU 4.8.4
        -- The CXX compiler identification is GNU 4.8.4
        -- Check for working C compiler: /usr/bin/cc
        -- Check for working C compiler: /usr/bin/cc -- works
        -- Detecting C compiler ABI info
        -- Detecting C compiler ABI info - done
        -- Check for working CXX compiler: /usr/bin/c++
        -- Check for working CXX compiler: /usr/bin/c++ -- works
        -- Detecting CXX compiler ABI info
        -- Detecting CXX compiler ABI info - done
        -- Performing Test COMPILER_SUPPORTS_CXX11
        -- Performing Test COMPILER_SUPPORTS_CXX11 - Success
        -- Performing Test COMPILER_SUPPORTS_CXX0X
        -- Performing Test COMPILER_SUPPORTS_CXX0X - Success
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /data/code/01-basic/L-cpp-standard/i-common-method/build

        $ make VERBOSE=1
        /usr/bin/cmake -H/data/code/01-basic/L-cpp-standard/i-common-method -B/data/code/01-basic/L-cpp-standard/i-common-method/build --check-build-system CMakeFiles/Makefile.cmake 0
        /usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/progress.marks
        make -f CMakeFiles/Makefile2 all
        make[1]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/depend
        make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        cd /data/code/01-basic/L-cpp-standard/i-common-method/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake --color=
        Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/DependInfo.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
        Dependee "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles/hello_cpp11.dir/depend.internal".
        Scanning dependencies of target hello_cpp11
        make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        make -f CMakeFiles/hello_cpp11.dir/build.make CMakeFiles/hello_cpp11.dir/build
        make[2]: Entering directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        /usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 1
        [100%] Building CXX object CMakeFiles/hello_cpp11.dir/main.cpp.o
        /usr/bin/c++    -std=c++11   -o CMakeFiles/hello_cpp11.dir/main.cpp.o -c /data/code/01-basic/L-cpp-standard/i-common-method/main.cpp
        Linking CXX executable hello_cpp11
        /usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cpp11.dir/link.txt --verbose=1
        /usr/bin/c++    -std=c++11    CMakeFiles/hello_cpp11.dir/main.cpp.o  -o hello_cpp11 -rdynamic
        make[2]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        /usr/bin/cmake -E cmake_progress_report /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles  1
        [100%] Built target hello_cpp11
        make[1]: Leaving directory `/data/code/01-basic/L-cpp-standard/i-common-method/build'
        /usr/bin/cmake -E cmake_progress_start /data/code/01-basic/L-cpp-standard/i-common-method/build/CMakeFiles 0



# 使用CMAKE_CXX_STANDARD变量  
设置CMAKE_CXX_STANDARD变量会导致所有目标上的CXX_STANDARD属性改变。这会影响CMake在编译时设置适当的标志。  

        set(CMAKE_CXX_STANDARD 11)

# 使用target_compile_features  
在目标上调用target_compile_features函数将检查传入的功能，并由CMake确定正确的用于目标的编译器标志。


        target_compile_features(hello_cpp11 PUBLIC cxx_auto_type)


与其他target_*函数一样，你可以指定所选目标的功能范围。这将填充目标的INTERFACE_COMPILE_FEATURES属性。可用功能列表可从CMAKE_CXX_COMPILE_FEATURES变量中找到。你可以使用以下代码获取可用功能的列表：  

        message("List of compile features: ${CMAKE_CXX_COMPILE_FEATURES}")





