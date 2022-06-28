# 编译器选项
CMake公开了用于控制编译和链接代码的程序的选项。这些程序包括:  
* CMAKE_C_COMPILER - 用于编译c代码的程序.
* CMAKE_CXX_COMPILER - 用于编译c++代码的程序.
* CMAKE_LINKER - 用于链接二进制文件的程序.
# 设置标志  
如第6节示例中所述，你可以使用cmake gui或通过命令行来设置CMake选项。  

下面是通过命令行向编译器传递参数的示例。  

cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6  

在设置这些选项之后，当你运行make时，clang将用于编译你的二进制文件。这可以从make输出中的以下几行看出。  

    /usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
    Linking CXX executable hello_cmake
    /usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1
    /usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic

# 构建示例
下面是示例输出：  

        $ mkdir build.clang

        $ cd build.clang/

        $ cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
        -- The C compiler identification is Clang 3.6.0
        -- The CXX compiler identification is Clang 3.6.0
        -- Check for working C compiler: /usr/bin/clang-3.6
        -- Check for working C compiler: /usr/bin/clang-3.6 -- works
        -- Detecting C compiler ABI info
        -- Detecting C compiler ABI info - done
        -- Check for working CXX compiler: /usr/bin/clang++-3.6
        -- Check for working CXX compiler: /usr/bin/clang++-3.6 -- works
        -- Detecting CXX compiler ABI info
        -- Detecting CXX compiler ABI info - done
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang

        $ make VERBOSE=1
        /usr/bin/cmake -H/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang -B/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang --check-build-system CMakeFiles/Makefile.cmake 0
        /usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/progress.marks
        make -f CMakeFiles/Makefile2 all
        make[1]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/depend
        make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        cd /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake --color=
        Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/DependInfo.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
        Dependee "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/CMakeDirectoryInformation.cmake" is newer than depender "/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles/hello_cmake.dir/depend.internal".
        Scanning dependencies of target hello_cmake
        make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        make -f CMakeFiles/hello_cmake.dir/build.make CMakeFiles/hello_cmake.dir/build
        make[2]: Entering directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        /usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 1
        [100%] Building CXX object CMakeFiles/hello_cmake.dir/main.cpp.o
        /usr/bin/clang++-3.6     -o CMakeFiles/hello_cmake.dir/main.cpp.o -c /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/main.cpp
        Linking CXX executable hello_cmake
        /usr/bin/cmake -E cmake_link_script CMakeFiles/hello_cmake.dir/link.txt --verbose=1
        /usr/bin/clang++-3.6       CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake -rdynamic
        make[2]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        /usr/bin/cmake -E cmake_progress_report /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles  1
        [100%] Built target hello_cmake
        make[1]: Leaving directory `/home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang'
        /usr/bin/cmake -E cmake_progress_start /home/matrim/workspace/cmake-examples/01-basic/I-compiling-with-clang/build.clang/CMakeFiles 0

        $ ./hello_cmake
        Hello CMake!
