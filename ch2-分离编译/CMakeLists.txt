cmake_minimum_required(VERSION 3.5)
project(ch2-hello_heads)
message(STATUS "根源目录 = ${CMAKE_SOURCE_DIR}")
message(STATUS "如果使用子项目和目录，则为当前源目录= ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS "当前 cmake 项目的源目录=${PROJECT_SOURCE_DIR}")
message(STATUS "根二进制文件生成目录。这是运行cmake命令的目录。=${CMAKE_BINARY_DIR}")
message(STATUS "你当前所处的生成目录。=${CMAKE_CURRENT_BINARY_DIR}") 
message(STATUS "当前项目的生成目录=${PROJECT_BINARY_DIR}") 

set(SOURCES 
    src/hello.cpp 
    src/main.cpp
) 

add_executable(hello_heads ${SOURCES})

target_include_directories(
    hello_heads
    PRIVATE
    ${PROJECT_SOURCE_DIR}/include
)


