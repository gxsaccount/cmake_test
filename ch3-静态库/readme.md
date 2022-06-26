# 添加动态库  

add_library()功能用于从某些源文件创建库。调用方式如下：  

        add_library(hello_library SHARED
            src/Hello.cpp
        )


这将使用传递给add_library()函数的源码文件创建一个名为libhello_library.so的共享库。  

# 别名目标  

顾名思义，别名目标是目标的替代名称，可以在只读上下文中替代真实的目标名称。  

add_library(hello::library ALIAS hello_library)  

ALIAS类似于“同义词”。ALIAS目标只是原始目标的另一个名称。因此ALIAS目标的要求是不可修改的——您无法调整其属性、安装它等  

# 链接共享库  

链接共享库与链接静态库相同。创建可执行文件时，请使用target_link_library()函数指向库。  

        add_executable(hello_binary
            src/main.cpp
        )

        target_link_libraries(hello_binary
            PRIVATE
                hello::library
        )

这告诉CMake使用别名目标名称将hello_library链接到hello_binary可执行文件。  

        /usr/bin/c++ CMakeFiles/hello_binary.dir/src/main.cpp.o -o hello_binary -rdynamic libhello_library.so \
            -Wl,-rpath,/home/matrim/workspace/cmake-examples/01-basic/D-shared-library/build
