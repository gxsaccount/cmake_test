# 安装  
CMake提供了添加make install目标的功能，以允许用户安装二进制文件、库和其他文件。  
基本安装位置由变量CMAKE_INSTALL_PREFIX控制，该变量可以使用ccmake或通过使用cmake .. -DCMAKE_INSTALL_PREFIX=/install/location调用cmake来设置。  

安装文件由install()函数控制。  

        install (TARGETS cmake_examples_inst_bin
            DESTINATION bin)

将目标cmake_examples_inst_bin生成的二进制文件安装到目标目录${CMAKE_INSTALL_PREFIX}/bin中。  

        install (TARGETS cmake_examples_inst
            LIBRARY DESTINATION lib)

将目标cmake_examples_inst生成的共享库安装到目标目录${CMAKE_INSTALL_PREFIX}/lib中。  

        install (TARGETS cmake_examples_inst
            LIBRARY DESTINATION lib
            RUNTIME DESTINATION bin) 

这在Windows上可能不起作用。在具有DLL目标的平台上，可能需要添加以下内容。     

        install (TARGETS cmake_examples_inst
            LIBRARY DESTINATION lib
            RUNTIME DESTINATION bin) 

        install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
            DESTINATION include)  

将针对cmake_examples_inst库进行开发的头文件安装到${CMAKE_INSTALL_PREFIX}/include目录中。  

        install (FILES cmake-examples.conf
            DESTINATION etc)
将配置文件安装到目标${CMAKE_INSTALL_PREFIX}/etc。  

在运行make install之后，CMake会生成一个install_mark.txt文件，其中包含所有已安装文件的详细信息。  

# 构建实例  

        $ mkdir build

        $ cd build/

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
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /home/matrim/workspace/cmake-examples/01-basic/E-installing/build

        $ make
        Scanning dependencies of target cmake_examples_inst
        [ 50%] Building CXX object CMakeFiles/cmake_examples_inst.dir/src/Hello.cpp.o
        Linking CXX shared library libcmake_examples_inst.so
        [ 50%] Built target cmake_examples_inst
        Scanning dependencies of target cmake_examples_inst_bin
        [100%] Building CXX object CMakeFiles/cmake_examples_inst_bin.dir/src/main.cpp.o
        Linking CXX executable cmake_examples_inst_bin
        [100%] Built target cmake_examples_inst_bin

        $ sudo make install
        [sudo] password for matrim:
        [ 50%] Built target cmake_examples_inst
        [100%] Built target cmake_examples_inst_bin
        Install the project...
        -- Install configuration: ""
        -- Installing: /usr/local/bin/cmake_examples_inst_bin
        -- Removed runtime path from "/usr/local/bin/cmake_examples_inst_bin"
        -- Installing: /usr/local/lib/libcmake_examples_inst.so
        -- Installing: /usr/local/etc/cmake-examples.conf

        $ cat install_manifest.txt
        /usr/local/bin/cmake_examples_inst_bin
        /usr/local/lib/libcmake_examples_inst.so
        /usr/local/etc/cmake-examples.conf

        $ ls /usr/local/bin/
        cmake_examples_inst_bin

        $ ls /usr/local/lib
        libcmake_examples_inst.so

        $ ls /usr/local/etc/
        cmake-examples.conf

        $ LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib cmake_examples_inst_bin
        Hello Install! 

# 注意事项  

* 如果/usr/local/lib不在库路径中，你可能需要在运行二进制文件之前将其添加到路径中。  
* 覆盖默认安装位置 
如前所述，默认安装位置是从CMAKE_INSTALL_PERFIX设置的，默认为/usr/local/  
如果你想为所有用户更改这个默认位置，可以在添加任何二进制文件或库之前将以下代码添加到你的顶端CMakeLists.txt中。  

        if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
            message(STATUS "Setting default CMAKE_INSTALL_PREFIX path to ${CMAKE_BINARY_DIR}/install")
            set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
        endif()

此示例将默认安装位置设置为你的构建目录下。  

# 目标文件夹  

如果你希望通过进行安装来确认是否包含所有文件，则make install目标支持DESTDIR参数。

        make install DESTDIR=/tmp/stage
这将为你的所有安装文件创建安装路径${DESTDIR}/${CMAKE_INSTALL_PREFIX}。在此示例中，它将在路径/tmp/stage/usr/local下安装所有文件  

        $ tree /tmp/stage
        /tmp/stage
        └── usr
            └── local
                ├── bin
                │   └── cmake_examples_inst_bin
                ├── etc
                │   └── cmake-examples.conf
                └── lib
                    └── libcmake_examples_inst.so

# 卸载  

默认情况下，CMake不会添加make uninstall目标。有关如何生成卸载目标的详细信息，请参阅https://gitlab.kitware.com/cmake/community/-/wikis/FAQ  
要从本例中删除文件的简单方法，可以使用：  

        sudo xargs rm < install_manifest.txt