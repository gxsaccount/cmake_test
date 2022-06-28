cmake_minimum_required(VERSION 3.5)

project(cmake_examples_install)  

add_library(cmake_examples_inst SHARED
    src/hello.cpp
)

target_include_directories(cmake_examples_inst
    PUBLIC
    ${PROJECT_SOURCE_DIR}/include
)

add_executable(cmake_examples_inst_bin
    src/main.cpp
)

target_link_libraries(cmake_examples_inst_bin 
    PRIVATE
    cmake_examples_inst
)


if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
    message(STATUS "Setting default CMAKE_INSTALL_PREFIX path to ${CMAKE_BINARY_DIR}/install")
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
endif()

# Binaries
install(TARGETS cmake_examples_inst_bin 
DESTINATION bin) 
# Library
install (TARGETS cmake_examples_inst
    LIBRARY DESTINATION lib)
# Header files
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ 
    DESTINATION include)
# Config
install (FILES cmake-examples.conf
    DESTINATION etc)