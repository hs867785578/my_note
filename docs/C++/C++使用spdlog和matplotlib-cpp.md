其中spdlog是一个头文件库，不需要链接。
matplotlib-cpp需要调用python，所以需要cmake中配置python
##  新建一个文件夹third_part

把spdlog和matplotlib-cpp clone下来
![](images/C++使用spdlog和matplotlib-cpp_image_1.png)


##  配置cmake

```cmake
cmake_minimum_required(VERSION 3.0.0)
project(test VERSION 0.1.0 LANGUAGES C CXX)
set(EXPORT_COMPILE_COMMANDS ON)

find_package(Python3 COMPONENTS Interpreter Development REQUIRED)
find_package(Python3 COMPONENTS NumPy)
find_package(PythonLibs REQUIRED)

add_executable(test main.cpp)

target_include_directories(test
PRIVATE
${CMAKE_CURRENT_SOURCE_DIR}/third_party/spdlog/include
${CMAKE_CURRENT_SOURCE_DIR}/third_party/matplotlib-cpp
${PYTHON_INCLUDE_DIRS})

target_link_libraries(test
PRIVATE
${PYTHON_LIBRARIES})
```
