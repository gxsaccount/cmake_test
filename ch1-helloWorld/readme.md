https://www.cnblogs.com/juzaizai/category/1993745.html  

# CMakeLists.txt  

CMakeLists.txt是存储所有CMake命令的文件。当cmake在文件夹中运行时，它将查找此文件，如果不存在，cmake 将因错误退出。  

# 最小 CMake 版本 
使用 CMake 创建项目时，你可以指定支持的最低版本的 CMake。  

        cmake_minimum_required(VERSION 3.5)

# 项目  
一个CMake构建文件可以包括一个项目名称，以便在使用多个项目时更容易引用某些变量。project()函数将创建一个值为hello_cmake的变量${PROJECT_NAME}    

        project (hello_cmake)  


# 创建可执行文件  

add_executable()命令规定，应从指定的源文件构建可执行文件，在此示例中是main.cpp。add_executable()函数的第一个参数是要构建的可执行文件的名称，第二个参数是要编译的源文件列表。  

        add_executable(hello_cmake main.cpp)  


# 二进制目录  

运行cmake命令的根文件夹或顶级文件夹称为CMAKE_BINARY_DIR，是所有二进制文件的根文件夹。CMake既支持就地构建和生成二进制文件，也支持在源代码外构建和生成二进制文件。 

## 就地构建  
就地构建将会在与源代码相同的目录结构中生成所有临时文件。这意味着所有的Makefile和目标文件都散布在你的普通代码中。要创建就地构建目标，请在根目录中运行cmake命令。例如：  

        A-hello-cmake$ cmake .
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
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake

        A-hello-cmake$ tree
        .
        ├── CMakeCache.txt
        ├── CMakeFiles
        │   ├── 2.8.12.2
        │   │   ├── CMakeCCompiler.cmake
        │   │   ├── CMakeCXXCompiler.cmake
        │   │   ├── CMakeDetermineCompilerABI_C.bin
        │   │   ├── CMakeDetermineCompilerABI_CXX.bin
        │   │   ├── CMakeSystem.cmake
        │   │   ├── CompilerIdC
        │   │   │   ├── a.out
        │   │   │   └── CMakeCCompilerId.c
        │   │   └── CompilerIdCXX
        │   │       ├── a.out
        │   │       └── CMakeCXXCompilerId.cpp
        │   ├── cmake.check_cache
        │   ├── CMakeDirectoryInformation.cmake
        │   ├── CMakeOutput.log
        │   ├── CMakeTmp
        │   ├── hello_cmake.dir
        │   │   ├── build.make
        │   │   ├── cmake_clean.cmake
        │   │   ├── DependInfo.cmake
        │   │   ├── depend.make
        │   │   ├── flags.make
        │   │   ├── link.txt
        │   │   └── progress.make
        │   ├── Makefile2
        │   ├── Makefile.cmake
        │   ├── progress.marks
        │   └── TargetDirectories.txt
        ├── cmake_install.cmake
        ├── CMakeLists.txt
        ├── main.cpp
        ├── Makefile


## 源外构建

使用源外构建，你可以创建单个生成文件夹，该文件夹可以位于文件系统的任何位置。所有临时构建的目标文件都位于此目录中，以保持源码目录结构的整洁。  
要进行源外构建，请运行build文件夹中的cmake命令，并将其指向根CMakeLists.txt文件所在的目录。使用源外构建时，如果你想从头开始重新创建cmake环境，只需删除构建目录，然后重新运行cmake。  

        A-hello-cmake$ mkdir build

        A-hello-cmake$ cd build/

        matrim@freyr:~/workspace/cmake-examples/01-basic/A-hello-cmake/build$ cmake ..
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
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/A-hello-cmake/build
        
        A-hello-cmake/build$ make ..
        make: Nothing to be done for `..'.

        A-hello-cmake/build$ cd ..

        A-hello-cmake$ tree
        .
        ├── build
        │   ├── CMakeCache.txt
        │   ├── CMakeFiles
        │   │   ├── 2.8.12.2
        │   │   │   ├── CMakeCCompiler.cmake
        │   │   │   ├── CMakeCXXCompiler.cmake
        │   │   │   ├── CMakeDetermineCompilerABI_C.bin
        │   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
        │   │   │   ├── CMakeSystem.cmake
        │   │   │   ├── CompilerIdC
        │   │   │   │   ├── a.out
        │   │   │   │   └── CMakeCCompilerId.c
        │   │   │   └── CompilerIdCXX
        │   │   │       ├── a.out
        │   │   │       └── CMakeCXXCompilerId.cpp
        │   │   ├── cmake.check_cache
        │   │   ├── CMakeDirectoryInformation.cmake
        │   │   ├── CMakeOutput.log
        │   │   ├── CMakeTmp
        │   │   ├── hello_cmake.dir
        │   │   │   ├── build.make
        │   │   │   ├── cmake_clean.cmake
        │   │   │   ├── DependInfo.cmake
        │   │   │   ├── depend.make
        │   │   │   ├── flags.make
        │   │   │   ├── link.txt
        │   │   │   └── progress.make
        │   │   ├── Makefile2
        │   │   ├── Makefile.cmake
        │   │   ├── progress.marks
        │   │   └── TargetDirectories.txt
        │   ├── cmake_install.cmake
        │   └── Makefile
        ├── CMakeLists.txt
        ├── main.cpp



