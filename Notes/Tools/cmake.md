# gcc/g++


# cmake

## cmake 语法

### cmake_minimum_required
指定cmake最小版本要求
 `cmake_minimum_required(VERSION versionNumber [FATAL_ERROT])`

```cmake
# cmake最小版本要求为2.8.3
cmake_minimum_required(VERSION 2.8.3)
```

### project
用来指定工程的名字和支持的语言
 `project(projectname [CXX][C][Java])`

```cmake
# 指定工程名为HELLO
project(HELLO)
```

### set
显式定义变量
 `set(VAR [VALUE][CACHE TYPE DOCSTRING[FORCE]])`

```cmake
# 定义SRC变量，其值为sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```

### include_directories
向工程添加多个指定的库文件搜索路径
 `include_directories([AFTER|BEFORE][SYSTEM] dir1 dir2)`

```cmake
# 将/usr/include/myincludefolder 和 ./include 添加到头文件搜索路径
include_directories(/usr/include/myincludefoler ./include)
```

### link_directories
向工程添加多个特定的库文件搜索路径
 `link_directories(dir1 dir2)`

```cmake
# 将/usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径
link_directories(/usr/lib/mylibfolder ./lib
```

### add_library
生成库文件
 `add_library(libname [SHARED|STATIC|MODULE][EXCLUDE_FROM_ALL] source1 source2 ... sourceN)`

```cmake
# 通过变量SRC生成libhello.so共享库
add_library(hello SHARED ${SRC})
```

### add_compile_options
添加编译参数
 `add_compile_options(<option> ...)`

```cmake
# 添加编译参数 -Wall -std=c++11
add_compile_options(-Wall -std=c++11 -o2)
```

### add_executable
生成可执行文件
 `add_library(exename source1 source2 ... sourceN)`

```cmake
# 编译main.cpp生成可执行文件main
add_executable(main main.cpp)
```

### target_link_libraries
为target添加需要链接的共享库
 `target_link_libraries(target library1<debug | optimized> library2 ...)`

```cmake
# 将hello动态库文件链接到可执行文件main
target_link_libraries(main hello)
```

### add_subdirectory
向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置
 `add_subdirectory(source_dir [binary_dir][EXCLUDE_FROM_ALL])`

```cmake
# 添加src子目录，src中需有一个CMakeLists.txt
add_subdirectory(src)
```

### aux_source_directory
发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表
 `aux_source_directory(dir VARIABLE)`

```cmake
# 定义SRC变量，其值为当前目录下所有的源代码文件
aux_source_directory(. SRC)
# 编译SRC变量所代表的源代码文件，生成main可执行文件
add_executable(main ${SRC})
```

## cmake常见变量

- `CMAKE_C_FLAGs` gcc编译选项

- `CMAKE_CXX_FLAGS` g++编译选项
    ```cmake
    # 在CMAKE_CXX_FLAGS编译选项后追加-std=c++11
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    ```

- `CMAKE_BUILD_TYPE` 编译类型(Debug, Release)
    ```cmake
    # 设定编译类型为debug，调试时需选择debug
    set(CMAKE_BUILD_TYPE Debug)
    # 设定编译类型为Release，调试时需选择Release
    set(CMAKE_BUILD_TYPE Release)
    ```

- `CMAKE_BINARY_DIR`
  `PROJECT_BINARY_DIR`
  `<projectname>_BINARY_DIR`
    1. 这三个变量指代的内容是一致的
    2. 如果是in source build，指的就是工程顶层目录
    3. 如果是out-of-source 编译，指的是工程编译发生的目录
    4. PROJECT_BINARY_DIR 跟其他指令稍有区别

- `CMAKE_SOURCE_DIR`
  `PROJECT_SOURCE_DIR`
  `<projectname>_SOURCE_DIR`
    1. 这三个变量指代的内容是一致的，指的是工程顶层目录
    2. 也就是在in source build时，他和CMAKE_BINARY_DIR等变量一致
    3. PROJECT_SOURCE_DIR 跟其他指令稍有区别

- `CMAKE_C_COMPILER` 指定C编译器

- `CMAKE_CXX_COMPILER` 指定C++编译器

- `EXECUTABLE_OUTPUT_PATH` 可执行文件输出的存放路径

- `LIBRARY_OUTPUT_PATH` 库文件输出的存放路径

## cmake编译工程

cmake目录结构: 项目的主目录存在一个CMakeLists.txt文件

### 两种方式设置编译规则
1. 包含源文件的子文件夹包含CMakeLists.txt文件，主目录的CMakeLists.txt通过add_subdirectory添加子目录即可
2. 包含源文件的子文件夹未包含CMakeLists,txt文件，子目录编译规则体现在主目录的CMakeLists.txt中

### 编译流程
1. 手动编写CMakeLists.txt
2. 执行命令cmake PATH生成MakeFile(PATH是顶层CMakeLists.txt所在的目录)
3. 执行命令make进行编译

### 两种构建方式
建议使用第二种构建方式
- 内部构建(in-source build)
    内部构建会在同级目录下生成一大堆中间文件，这些文件并不是我们最终所需要的，和工程源文件放在一起会显得杂乱无章。
    ```cmd
    ## 内部构建

    # 在当前目录下，编译本目录的CMakeLists.txt，生成Makefile和其他文件
    cmake .
    # 执行make命令，生成target
    make
    ```

- 外部构建(out-of-source build)
    将编译输出文件与源文件放到不同目录中
    ```cmd
    ## 外部构建

    # 1. 在当前目录下，创建build文件夹
    mkdir build
    # 2. 进入build文件夹
    cd build
    # 3. 编译上级目录的CMakeLists.txt，生成Makefile和其他文件
    cmake .. (cmake . -G "Unix Makefiles")
    # 4. 执行make命令，生成target
    make
    ```