# CMake c++项目配置

项目目录

```
E:.
.test_cpp
│  CMakeLists.txt
│
├─MainApp
│      CMakeLists.txt
│      Main.cpp
│      Main.h
```

test_cpp/ CMakeLists.txt

```cmake
#指定cmake最低版本
cmake_minimum_required(VERSION 2.6)
#设置项目名称
project(test_cpp)
#打印信息
message(${CMAKE_SYSTEM_NAME})
message(${CMAKE_SOURCE_DIR})

#判断设置操作系统版本 
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(PROJ_SYS_LINUX TRUE)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    set(PROJ_SYS_WINDOWS TRUE)
else()
    message(FATAL_ERROR, "Unknown target system \"${CMAKE_SYSTEM_NAME}\".")
endif()
#指定cmake最低版本
cmake_minimum_required(VERSION 2.6)
#设置项目名称
project(test_cpp)
#打印信息
message(${CMAKE_SYSTEM_NAME})
message(${CMAKE_SOURCE_DIR})

#判断设置操作系统版本 
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(PROJ_SYS_LINUX TRUE)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    set(PROJ_SYS_WINDOWS TRUE)
else()
    message(FATAL_ERROR, "Unknown target system \"${CMAKE_SYSTEM_NAME}\".")
endif()

#设置全局inc路径
include_directories(${CMAKE_SOURCE_DIR})
#设置全局link路径
link_directories(${CMAKE_SOURCE_DIR})

#设置工程目录
add_subdirectory(MainApp)
#设置全局inc路径
include_directories(${CMAKE_SOURCE_DIR})
#设置全局link路径
link_directories(${CMAKE_SOURCE_DIR})

#


```

test_cpp/MainApp/ CMakeLists.txt CMakeLists.txt

```cmake
#当前目录
message(STATUS ${CMAKE_CURRENT_SOURCE_DIR})

set(SRC_HeadFiles
    Main.h
)

source_group("Head Files" FILES ${SRC_HeadFiles})

set(SRC_MainApp_Files
    Main.cpp
)

#源码分组，对应vs的文件过滤器
source_group("MainApp Files" FILES ${SRC_MainApp_Files})

#添加操作系统自己的文件
if(WIN32)
    message(STATUS "windows system ...")
else(WIN32)
    message(STATUS "other system ...")
endif(WIN32)

set(Source
    ${SRC_HeadFiles}
    ${SRC_MainApp_Files}
)

set(ProjectName MainApp)
#生成可执行文件
add_executable(
    ${ProjectName}    #文件名
    ${Source}         #源码集合
)

#添加动态链接库 可以有多个
# target_link_libraries(
#     ${ProjectName}
#     ${curl}
# )
```

<u>**持续更新中**</u>