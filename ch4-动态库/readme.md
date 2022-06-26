# 添加静态库  

add_library()功能用于从某些源文件创建库。调用方式如下：  

        add_library(hello_library STATIC
            src/Hello.cpp
        )


此命令将使用add_library()调用中的源代码创建一个名为libhello_library.a的静态库  
如上一节所述，我们按照现代 CMake 的建议，将源文件直接传递给add_library调用。  

# 添加头文件目录  

在本例中，我们使用target_include_directory()函数将include目录包含在库中，并将范围设置为PUBLIC。  

        target_include_directories(hello_library
            PUBLIC
                ${PROJECT_SOURCE_DIR}/include
        )

这将导致包含的目录在以下位置使用：  
* 在编译该库时
* 在编译链接至该库的任何其他目标时。
作用域的含义是：  

* PRIVATE - 将目录添加到此目标的include目录中
* INTERFACE - 将该目录添加到任何链接到此库的目标的include目录中（不包括自己）。
* PUBLIC - 它包含在此库中，也包含在链接此库的任何目标中 

对于公共头文件，让你的include文件夹使用子目录“命名空间”通常是个好主意。  
传递给target_include_directories的目录将是你的Include目录树的根目录，并且你的C++文件应该包含从那里到你要使用的头文件的路径。  
在本例中，你可以看到我们按如下方式执行操作。使用此方法意味着，在项目中使用多个库时，头文件名冲突的可能性较小。比如：    

  #include "static/Hello.h"  

# 链接库  

在创建使用库的可执行文件时，你必须告诉编译器有关库的信息。这可以使用target_link_library()函数来完成。  

        add_executable(hello_binary
            src/main.cpp
        )

        target_link_libraries( hello_binary
            PRIVATE
                hello_library
        )

这告诉CMake在链接时将hello_library与hello_binary可执行文件链接起来。  
它还将从hello_library传递任何具有PUBLIC或INTERFACE作用范围的include目录到hello_binary。  

编译器调用它的一个示例是:  

/usr/bin/c++ CMakeFiles/hello_binary.dir/src/main.cpp.o -o hello_binary -rdynamic libhello_library.a  

