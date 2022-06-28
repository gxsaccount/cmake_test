# 生成器  
CMake生成器负责为底层构建系统编写输入文件(例如Makefile)。运行cmake--help将显示可用的生成器。  

        Generators

        The following generators are available on this platform (* marks default):
        Green Hills MULTI            = Generates Green Hills MULTI files
                                        (experimental, work-in-progress).
        * Unix Makefiles               = Generates standard UNIX makefiles.
        Ninja                        = Generates build.ninja files.
        Ninja Multi-Config           = Generates build-<Config>.ninja files.
        Watcom WMake                 = Generates Watcom WMake makefiles.
        CodeBlocks - Ninja           = Generates CodeBlocks project files.
        CodeBlocks - Unix Makefiles  = Generates CodeBlocks project files.
        CodeLite - Ninja             = Generates CodeLite project files.
        CodeLite - Unix Makefiles    = Generates CodeLite project files.
        Eclipse CDT4 - Ninja         = Generates Eclipse CDT 4.0 project files.
        Eclipse CDT4 - Unix Makefiles= Generates Eclipse CDT 4.0 project files.
        Kate - Ninja                 = Generates Kate project files.
        Kate - Unix Makefiles        = Generates Kate project files.
        Sublime Text 2 - Ninja       = Generates Sublime Text 2 project files.
        Sublime Text 2 - Unix Makefiles
                                    = Generates Sublime Text 2 project files.

正如本文所述，CMake包括不同类型的生成器，如命令行生成器、IDE生成器和其他生成器。  

# 命令行生成工具生成器  

这些生成器用于命令行构建工具，如Make和Ninja。在使用CMake生成构建系统之前，必须配置所选的工具链。  
支持的生成器包括：

* Borland Makefiles
* MSYS Makefiles
* MinGW Makefiles
* NMake Makefiles
* NMake Makefiles JOM
* Ninja
* Unix Makefiles
* Watcom WMake

# IDE构建工具生成器  

这些生成器用于集成开发环境，其中包括它们自己的编译器。例如Visual Studio和Xcode，它们本身就包含一个编译器。  

* Visual Studio 6
* Visual Studio 7
* Visual Studio 7 .NET 2003
* Visual Studio 8 2005
* Visual Studio 9 2008
* Visual Studio 10 2010
* Visual Studio 11 2012
* Visual Studio 12 2013
* Xcode

# 其他生成器  

这些生成器创建配置并与其他IDE工具共同工作，并且必须包含在IDE或命令行生成器中。  

支持的生成器包括  

* CodeBlocks
* CodeLite
* Eclipse CDT4
* KDevelop3
* Kate
* Sublime Text 2
在本例中，ninja是通过命令sudo apt-get install ninja-build安装的。  

# 调用生成器  

        cmake .. -G Ninja  
完成上述操作后，CMake将生成所需的Ninja构建文件，这些文件可以通过使用Ninja命令运行。  
cmake .. -G Ninja  

        $ ls
        build.ninja  CMakeCache.txt  CMakeFiles  cmake_install.cmake  rules.ninja  
# 构建示例 
        $ mkdir build.ninja

        $ cd build.ninja/

        $ cmake .. -G Ninja
        -- The C compiler identification is GNU 4.8.4
        -- The CXX compiler identification is GNU 4.8.4
        -- Check for working C compiler using: Ninja
        -- Check for working C compiler using: Ninja -- works
        -- Detecting C compiler ABI info
        -- Detecting C compiler ABI info - done
        -- Check for working CXX compiler using: Ninja
        -- Check for working CXX compiler using: Ninja -- works
        -- Detecting CXX compiler ABI info
        -- Detecting CXX compiler ABI info - done
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/J-building-with-ninja/build.ninja

        $ ninja -v
        [1/2] /usr/bin/c++     -MMD -MT CMakeFiles/hello_cmake.dir/main.cpp.o -MF "CMakeFiles/hello_cmake.dir/main.cpp.o.d" -o CMakeFiles/hello_cmake.dir/main.cpp.o -c ../main.cpp
        [2/2] : && /usr/bin/c++      CMakeFiles/hello_cmake.dir/main.cpp.o  -o hello_cmake  -rdynamic && :

        $ ls
        build.ninja  CMakeCache.txt  CMakeFiles  cmake_install.cmake  hello_cmake  rules.ninja

        $ ./hello_cmake
        Hello CMake!