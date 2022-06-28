
# 导入目标
导入目标是由FindXXX模块导出的只读目标（例如Boost::filesystem）。  

要包括Boost文件系统，你可以执行以下操作：  

        target_link_libraries( imported_targets
            PRIVATE
                Boost::filesystem
        )
这将自动链接Boost::FileSystem和Boost::System库，同时还包括Boost include目录（即不必手动添加include目录）。    

# 构建示例
        $ mkdir build

        $ cd build/

        $ cmake ..
        -- The C compiler identification is GNU 5.4.0
        -- The CXX compiler identification is GNU 5.4.0
        -- Check for working C compiler: /usr/bin/cc
        -- Check for working C compiler: /usr/bin/cc -- works
        -- Detecting C compiler ABI info
        -- Detecting C compiler ABI info - done
        -- Detecting C compile features
        -- Detecting C compile features - done
        -- Check for working CXX compiler: /usr/bin/c++
        -- Check for working CXX compiler: /usr/bin/c++ -- works
        -- Detecting CXX compiler ABI info
        -- Detecting CXX compiler ABI info - done
        -- Detecting CXX compile features
        -- Detecting CXX compile features - done
        -- Boost version: 1.58.0
        -- Found the following Boost libraries:
        --   filesystem
        --   system
        boost found
        -- Configuring done
        -- Generating done
        -- Build files have been written to: /data/code/01-basic/K-imported-targets/build

        $ make
        Scanning dependencies of target imported_targets
        [ 50%] Building CXX object CMakeFiles/imported_targets.dir/main.cpp.o
        [100%] Linking CXX executable imported_targets
        [100%] Built target imported_targets


        $ ./imported_targets
        Hello Third Party Include!
        Path is not relative