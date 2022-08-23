# 添加子目录  

        add_subdirectory(sublibrary1)
        add_subdirectory(sublibrary2)
        add_subdirectory(subbinary) 

# 引用子项目目录 

当使用project()命令创建项目时，CMake将自动创建许多变量，这些变量可用于引用有关项目的详细信息。然后，其他子项目或主项目可以使用这些变量。例如，引用你可以使用的不同项目的源目录。  

        ${sublibrary1_SOURCE_DIR}
        ${sublibrary2_SOURCE_DIR} 

CMake创建的变量包括：  

Variable |	Info  
-|-
PROJECT_NAME |	由当前project()设置的项目名称
CMAKE_PROJECT_NAME |	由project()命令设置的第一个项目的名称，即顶级项目
PROJECT_SOURCE_DIR |	当前项目的源目录
PROJECT_BINARY_DIR |	当前项目的生成目录
name_SOURCE_DIR | 	名为“name”的项目的源目录。在本例中，创建的源目录将是sublibrary1_SOURCE_DIR、sublibrary2_SOURCE_DIR和subbinary_SOURCE_DIR
name_BINARY_DIR |	名为“name”的项目的二进制目录。在本例中，创建的二进制目录为sublibrary1_BINARY_DIR 、sublibrary2_BINARY_DIR和subbinary_BINARY_DIR。

# 头文件库  

如果你有一个被创建为只包含头文件的库，cmake支持接口目标，以允许在没有任何构建输出的情况下创建目标。有关更多详细信息https://cmake.org/cmake/help/v3.4/command/add_library.html#interface-libraries  

在创建目标时，你还可以使用INTERFACE作用域包含该目标的目录。INTERFACE作用域用于制定目标要求，这些要求在链接此目标的任何库中使用，但不用于目标本身的编译。如下示例，链接至此目标的任何目标都将包含一个include目录，但此目标本身并不进行编译（即不产生任何实体内容）：  

        target_include_directories(${PROJECT_NAME}
            INTERFACE
                ${PROJECT_SOURCE_DIR}/include
        )

# 从子项目中引用库  

如果某个子项目创建库，则其他项目可以通过在target_link_library()命令中调用该项目的名称来引用该库。这意味着你不必引用新库的完整路径，它将作为依赖项被添加。  

        target_link_libraries(subbinary
            PUBLIC
                sublibrary1
        )

或者，你可以创建一个别名目标，使你可以在只读上下文中引用该目标。
要创建别名目标运行，请执行以下操作：  

        add_library(sublibrary2)
        add_library(sub::lib2 ALIAS sublibrary2)
要引用别名，只需如下所示：

        target_link_libraries(subbinary
            sub::lib2
        )

# 包含来自子项目的头文件目录  

当从子项目添加库时，从cmake v3开始，不需要使用它们在二进制文件的include目录中添加项目include目录。  

这由创建库时target_include_directory()命令中的作用域控制。  
在本例中，因为子二进制可执行文件链接了subibrary1和subibrary2库，所以它将自动包括${subibrary1_source_DIR}/include和${subibrary2_source_DIR}/include文件夹，因为它们是随库的PUBLIC和INTERFACE范围导出的。  




