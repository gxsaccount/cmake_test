cmake_minimum_required(VERSION 3.5)
project (third_party_include)
# find a boost install with the libraries filesystem and system
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)
if(Boost_FOUND)
    message ("boost found")
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()

# Add an executable
add_executable(third_party_include main.cpp)

# link against the boost libraries
target_link_libraries( third_party_include
    PRIVATE
        Boost::filesystem
)