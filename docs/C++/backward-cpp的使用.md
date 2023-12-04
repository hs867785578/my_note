
## 0 安装依赖

```bash
sudo apt-get install libdw-dev
```
## 1 官方使用方法（add subdirecttory）

需要把整个包都clone下来

```bash
git clone https://github.com/bombela/backward-cpp.git
```

CMakeLists.txt
记住，要打开debug模式
```
cmake_minimum_required(VERSION 3.15)
project(test)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_subdirectory(backward-cpp)
set(CMAKE_BUILD_TYPE Debug)
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PUBLIC Backward::Interface)
```
测试代码

```cpp
#include<stdio.h>
#include<stdlib.h>
#define BACKWARD_HAS_DW 1
#include "backward.hpp"
namespace backward{
    backward::SignalHandling sh;
}

int main(){
    char *c = "hello world";
    c[1] = 'H';
}
```

## 2 简洁方法

只需要clone hpp文件
```bash
wget https://raw.githubusercontent.com/bombela/backward-cpp/master/backward.hpp
```

CMakeLists.txt
记住，要打开debug模式
```
cmake_minimum_required(VERSION 3.15)
project(test)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# debug
set(CMAKE_BUILD_TYPE Debug)
add_executable(${PROJECT_NAME} main.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE
    backward-cpp
)
target_link_libraries(${PROJECT_NAME} PRIVATE
    dw
)
```
测试代码

```cpp
#include<stdio.h>
#include<stdlib.h>
#define BACKWARD_HAS_DW 1
#include "backward.hpp"
namespace backward{
    backward::SignalHandling sh;
}

int main(){
    char *c = "hello world";
    c[1] = 'H';
}
```
