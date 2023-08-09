![1.png](../_resources/1.png)

[![20201124032511.png](../_resources/20201124032511.png)](https://www.csdn.net/)

- [博客](https://blog.csdn.net/)
- [下载](https://download.csdn.net/)
- [学习](https://edu.csdn.net/)
- [社区](https://bbs.csdn.net/)
- [GitCode](https://gitcode.net/?utm_source=csdn_toolbar)
- [云服务](https://dev-portal.csdn.net/welcome?utm_source=toolbar)
- [猿如意](https://devbit.csdn.net/?source=csdn_toolbar)

 [![2_qq_44640266](../_resources/2_qq_44640266)](https://blog.csdn.net/qq_44640266)

 [会员中心 ![20210918025138.gif](../_resources/20210918025138.gif)](https://mall.csdn.net/vip)

 [足迹](https://i.csdn.net/#/user-center/collection-list?type=1)

 [动态](https://blink.csdn.net/)

 [消息](https://i.csdn.net/#/msg/index)

 [创作中心![20220627041202.png](../_resources/20220627041202.png)](https://mp.csdn.net/)

 [**发布**](https://mp.csdn.net/edit)

# ubuntu下protobuf使用案例[命令行-＞cmake-＞VS code-＞bazel]

 ![original.png](../_resources/original.png)

 [windSeS](https://windses.blog.csdn.net/)  ![newUpTime2.png](../_resources/newUpTime2.png)  已于 2022-05-25 11:00:45 修改  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  1081  [![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏  9]()

 分类专栏：  [coding](https://blog.csdn.net/u013468614/category_6754321.html)  文章标签：  [c++](https://so.csdn.net/so/search/s.do?q=c%2B%2B&t=blog&o=vip&s=&l=&f=&viparticle=)  [linux](https://so.csdn.net/so/search/s.do?q=linux&t=blog&o=vip&s=&l=&f=&viparticle=)  [ubuntu](https://so.csdn.net/so/search/s.do?q=ubuntu&t=blog&o=vip&s=&l=&f=&viparticle=)  [cmake](https://so.csdn.net/so/search/s.do?q=cmake&t=blog&o=vip&s=&l=&f=&viparticle=)  [protobuf](https://so.csdn.net/so/search/s.do?q=protobuf&t=blog&o=vip&s=&l=&f=&viparticle=)

 [版权<div style="display: none;"></div>

]()

 [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)      coding  专栏收录该内容](https://blog.csdn.net/u013468614/category_6754321.html)

 26 篇文章  7 订阅

 [订阅专栏]()

 <div style="display: none;"></div>

系统： [ubuntu](https://so.csdn.net/so/search?q=ubuntu&spm=1001.2101.3001.7020) 18.04

点击下载本博客使用的示例程序[protobuf.tar.xz](https://download.csdn.net/download/u013468614/12707438)

**protobuf.tar.xz**文件解压后的目录树如下所示，根据需要下载：

 ![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70#pic_center](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70#pic_center)

## 一、protobuf安装

```
sudo apt-get install autoconf automake libtool curl make g++ unzip git
git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
protoc --version
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10

若最后输出类似如下信息，说明protobuf安装成功。
 ![20200811150414417.png](../_resources/20200811150414417.png)

## 二、命令行使用protobuf

### 2.1 构建proto文件

```
mkdir -p CSDN_ws/protobuf/teminal/
cd CSDN_ws/protobuf/teminal/
gedit AddressBook.proto
```

- 1
- 2
- 3

将以下文本复制到**AddressBook.proto**，并保存。

```
package tutorial;

message Person {
    required string name=1;
    required int32 id=2;
    optional string email=3;

    enum PhoneType {
        MOBILE=0;
        HOME=1;
        WORK=2;
    }

    message PhoneNumber {
        required string number=1;
        optional PhoneType type=2 [default=HOME];
    }

    repeated PhoneNumber phone=4;
}

message AddressBook {
    repeated Person person=1;
}
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24

### 2.2 生成c++头文件

```
protoc --cpp_out=./ AddressBook.proto
ls
```

- 1
- 2

此时发现，目录下多出两个文件：`AddressBook.pb.cc`与`AddressBook.pb.h`。
 ![20200811151828131.png](../_resources/20200811151828131.png)

### 2.3 使用protobuf头文件

`gedit main.cpp`

- 1

将以下文本复制到**main.cpp**，并保存。

```
#include <iostream>
#include <fstream>
#include <vector>
#include "AddressBook.pb.h"

using namespace std;

*// This function fills in a Person message based on user input.*
void PromptForAddress(tutorial::Person* person)
{
    cout << "Enter person ID number: ";
    int id;
    cin >> id;
    person->set_id(id);
    cin.ignore(256, '\n');

    cout << "Enter name: ";
    getline(cin, *person->mutable_name());

    cout << "Enter email address (blank for none): ";
    string email;
    getline(cin, email);
    if (!email.empty())
    {
        person->set_email(email);
    }

    while (true)
    {
        cout << "Enter a phone number (or leave blank to finish): ";
        string number;
        getline(cin, number);
        if (number.empty())
        {
            break;
        }

        tutorial::Person::PhoneNumber* phone_number = person->add_phone();
        phone_number->set_number(number);

        cout << "Is this a mobile, home, or work phone? ";
        string type;
        getline(cin, type);
        if (type == "mobile")
        {
            phone_number->set_type(tutorial::Person::MOBILE);
        }
        else if (type == "home")
        {
            phone_number->set_type(tutorial::Person::HOME);
        }
         else if (type == "work")
         {
            phone_number->set_type(tutorial::Person::WORK);
        }
        else
        {
            cout << "Unknown phone type.  Using default." << endl;
        }
    }
}

int main(int argc, char *argv[])
{
    GOOGLE_PROTOBUF_VERIFY_VERSION;

    if (argc != 2)
    {
        cerr << "Usage:  " << argv[0] << " ADDRESS_BOOK_FILE" << endl;
        return -1;
    }

    tutorial::AddressBook address_book;

    {
    *// Read the existing address book.*
    fstream input(argv[1], ios::in | ios::binary);
    if (!input) {
      cout << argv[1] << ": File not found.  Creating a new file." << endl;
    } else if (!address_book.ParseFromIstream(&input)) {
      cerr << "Failed to parse address book." << endl;
      return -1;
    }
    }

    *// Add an address.*
    PromptForAddress(address_book.add_person());

    {
    *// Write the new address book back to disk.*
    fstream output(argv[1], ios::out | ios::trunc | ios::binary);
    if (!address_book.SerializeToOstream(&output)) {
      cerr << "Failed to write address book." << endl;
      return -1;
    }
    }

    *// Optional:  Delete all global objects allocated by libprotobuf.*
    google::protobuf::ShutdownProtobufLibrary();

    return 0;
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29
- 30
- 31
- 32
- 33
- 34
- 35
- 36
- 37
- 38
- 39
- 40
- 41
- 42
- 43
- 44
- 45
- 46
- 47
- 48
- 49
- 50
- 51
- 52
- 53
- 54
- 55
- 56
- 57
- 58
- 59
- 60
- 61
- 62
- 63
- 64
- 65
- 66
- 67
- 68
- 69
- 70
- 71
- 72
- 73
- 74
- 75
- 76
- 77
- 78
- 79
- 80
- 81
- 82
- 83
- 84
- 85
- 86
- 87
- 88
- 89
- 90
- 91
- 92
- 93
- 94
- 95
- 96
- 97
- 98
- 99
- 100
- 101
- 102
- 103

接着在命令行执行以下命令编译：

```
g++ AddressBook.pb.cc main.cpp -o main `pkg-config --cflags --libs protobuf`
ls
```

- 1
- 2

编译成功的话会多出一个名为**main**的文件。

 ![20200811153215996.png](../_resources/20200811153215996.png)执行**main**，会让你录入信息：

`./main name.txt`

- 1

![2020081115343034.png](../_resources/2020081115343034.png)以上就是teminal下使用protobuf的所有过程，结束后你的工作空间应该如下：

 ![2020081115392916.png](../_resources/2020081115392916.png)

## 三、cmake下使用protobuf

### 3.1 安装cmake

`sudo apt-get install cmake cmake-qt-qui`

- 1

### 3.2 编写CMakeLists文件

```
mkdir -p CSDN_ws/protobuf/cmake/

cp CSDN_ws/protobuf/teminal/AddressBook.proto CSDN_ws/protobuf/cmake/AddressBook.proto

cp CSDN_ws/protobuf/teminal/main.cpp CSDN_ws/protobuf/cmake/main.cpp
cd CSDN_ws/protobuf/cmake/
gedit CMakeLists.txt
```

- 1
- 2
- 3
- 4
- 5

将以下内容复制到**CMakeLists.txt**文件中：

```
cmake_minimum_required(VERSION 3.10)
PROJECT (cppTest)
SET(SRC_LIST main.cpp)

# Find required protobuf package

find_package(Protobuf REQUIRED)
if(PROTOBUF_FOUND)
    message(STATUS "protobuf library found")
else()
    message(FATAL_ERROR "protobuf library is needed but cant be found")
endif()

include_directories(${PROTOBUF_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})
PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS AddressBook.proto)

ADD_EXECUTABLE(cppTest ${SRC_LIST} ${PROTO_SRCS} ${PROTO_HDRS})

target_link_libraries(cppTest ${PROTOBUF_LIBRARIES})
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20

### 3.3 cmake编译与运行

```
cd CSDN_ws/protobuf/cmake/
cmake .
make
./cppTest  name.txt
```

- 1
- 2
- 3
- 4

运行结果和终端运行一样，最终文件目录如下所示：

 ![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70-3)

## 四、VS code下使用protobuf

### 4.1 安装VS code

ubuntu 18.04进入ubuntu software输入visual studio code，安装：

![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70-2)

去VS Code自带的商店下载的插件，快捷键：Ctrl+Shift+x，下载各种依赖包，包括：c/c++，c/c++ clang command adapter，c++ intellisense，CMake和CMake Tools如下图所示：

 ![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70)

### 4.2 编译执行

构建vscode文件夹，并打开vs code:

```
mkdir CSDN_ws/protobuf/vscode/

cp CSDN_ws/protobuf/cmake/AddressBook.proto CSDN_ws/protobuf/vscode/AddressBook.proto

cp CSDN_ws/protobuf/cmake/main.cpp CSDN_ws/protobuf/vscode/main.cpp
cp CSDN_ws/protobuf/cmake/CMakeLists.txt CSDN_ws/protobuf/vscode/CMakeLists.txt
cd CSDN_ws/protobuf/vscode/
code .
```

- 1
- 2
- 3
- 4
- 5
- 6

点击左侧的Debug按钮，选择添加配置（Add configuration）,然后选择C++（GDB/LLDB)，将自动生成launch.json文件如下：

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.

    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387

    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++ - Build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/cppTest",
            "args": ["name.txt"],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: g++ build active file",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29

在vs code界面点击F5,会提示要求配置task.json文件，将以下文本复制到默认task.json文件中：

```
{

	"version": "2.0.0",
	"tasks": [
		{
			"type": "shell",
			"label": "protobuf build",
			"command": "cmake . && make",
			"group": "build"
		}
	]

}
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11

反回到main.cpp界面，点击F5,会显示编译消息，最终是运行结果：
 ![20200811200318264.png](../_resources/20200811200318264.png)

最终文件目录如下所示：

 ![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70-1)

如果你留恋这种顺畅，估计会发现以下这个不和谐的波浪线：

![20200811201433619.png](../_resources/20200811201433619.png)

点击图中那个黄色的小灯泡，会出现几个小提示（**这个小灯泡不影响编译，只是头文件不被VS code识别，无法自动补全**），点击`Add to "includePath":${workspaceFolder}`，会发现生多了一个`c_cpp_properties.json`:

```
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "${workspaceFolder}"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++14",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17

最后，VS code文件目录如下：
 ![20200811202101577.png](../_resources/20200811202101577.png)

## 五、bazel

### 5.1 安装bazel

```
sudo apt install curl gnupg
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -

echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list

sudo apt update && sudo apt install bazel
```

- 1
- 2
- 3
- 4

可以利用[Bazel入门：编译C++项目](https://blog.csdn.net/elaine_bao/article/details/78668657)先熟悉一下。

### 5.2 构建项目

本部分参考[Bazel使用：编译protobuf](https://blog.csdn.net/qq_41347613/article/details/107858226)。

```
mkdir -p CSDN_ws/protobuf/bazel/

cp CSDN_ws/protobuf/teminal/AddressBook.proto CSDN_ws/protobuf/bazel/AddressBook.proto

cp CSDN_ws/protobuf/teminal/main.cpp CSDN_ws/protobuf/bazel/main.cpp
cd CSDN_ws/protobuf/bazel/
gedit WORKSPACE
```

- 1
- 2
- 3
- 4
- 5

在**WORKSPACE**文件里输入以下内容：

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

*# rules_cc defines rules for generating C++ code from Protocol Buffers.*
http_archive(
    name = "rules_cc",

    sha256 = "35f2fb4ea0b3e61ad64a369de284e4fbbdcdba71836a5555abb5e194cf119509",

    strip_prefix = "rules_cc-624b5d59dfb45672d4239422fa1e3de1822ee110",
    urls = [

        "https://mirror.bazel.build/github.com/bazelbuild/rules_cc/archive/624b5d59dfb45672d4239422fa1e3de1822ee110.tar.gz",

        "https://github.com/bazelbuild/rules_cc/archive/624b5d59dfb45672d4239422fa1e3de1822ee110.tar.gz",

    ],
)

*# rules_proto defines abstract rules for building Protocol Buffers.*
http_archive(
    name = "rules_proto",

    sha256 = "2490dca4f249b8a9a3ab07bd1ba6eca085aaf8e45a734af92aad0c42d9dc7aaf",

    strip_prefix = "rules_proto-218ffa7dfa5408492dc86c01ee637614f8695c45",
    urls = [

        "https://mirror.bazel.build/github.com/bazelbuild/rules_proto/archive/218ffa7dfa5408492dc86c01ee637614f8695c45.tar.gz",

        "https://github.com/bazelbuild/rules_proto/archive/218ffa7dfa5408492dc86c01ee637614f8695c45.tar.gz",

    ],
)

load("@rules_cc//cc:repositories.bzl", "rules_cc_dependencies")
rules_cc_dependencies()

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()
rules_proto_toolchains()
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29
- 30

### 5.3 编写BUILD

`gedit BUILD`

- 1

```
load("@rules_cc//cc:defs.bzl", "cc_proto_library")
load("@rules_proto//proto:defs.bzl", "proto_library")

cc_proto_library(
    name = "AddressBook_proto",
    deps = [":AddressBook_proto_lib"],
)

proto_library(
    name = "AddressBook_proto_lib",
    srcs = ["AddressBook.proto"],
)

cc_binary(
    name = "main",
    srcs = ["main.cpp"],
    deps = ["AddressBook_proto"],
)
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18

### 5.4 编译并执行项目

执行以下命令编译项目

`sudo bazel build :main`

- 1

得到如下输出信息：

 ![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70#pic_center](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0Njg2MTQ=,size_16,color_FFFFFF,t_70#pic_center-1)

执行以下命令执行项目

`./bazel-bin/main name.txt`

- 1

得到如下结果：
 ![20200812154709921.png#pic_center](../_resources/20200812154709921.png#pic_center)

* * *

END
by windSeS
2020-8-12

文章知识点与官方知识档案匹配，可进一步学习相关知识

[CS入门技能树](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)**[Linux进阶](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)**[新增用户](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)24584 人正在系统学习中

[在*Ubuntu* 上安装 *Pro**tobuf* 3 的教程详解](https://download.csdn.net/download/weixin_38672800/12842115)

09-15

[主要介绍了在*Ubuntu* 上安装 *Pro**tobuf* 3遇到问题及解决方法，本文给大家介绍的非常详细，具有一定的参考借鉴价值 ,需要的朋友可以参考下](https://download.csdn.net/download/weixin_38672800/12842115)

[*ubuntu*下安装*pro**tobuf*及*cmake*编译](https://blog.csdn.net/guangguyu/article/details/82149925)

[guangguyu的博客](https://blog.csdn.net/guangguyu)
![readCountWhite.png](../_resources/readCountWhite.png)3012

[1.下载*pro**tobuf*https://github.com/google/*pro**tobuf*/releases/download/v2.6.1/*pro**tobuf*-2.6.1.tar.gz

cd 到下载目录，并解压tar -zxvf *pro**tobuf*-2.6.1.tar.gz
2. cd到解压后的文件夹
cd ../*pro**tobuf*-2.6.1/

注：我是将文件复制到个人用户目录下了 ...](https://blog.csdn.net/guangguyu/article/details/82149925)

评论4条![commentArrowRightWhite.png](../_resources/commentArrowRightWhite.png)[写评论]()

[![3_weixin_55295819](../_resources/3_weixin_55295819)酷爱运动的小伙子](https://blog.csdn.net/weixin_55295819)热评

报错显示：找不到任务："c/c++ :g++ build active file"

[【*CMake*/*Pro**tobuf*】*CMake* 下*使用*  *pro**tobuf*](https://blog.csdn.net/gcs_20210916/article/details/123992953)

[gcs_20210916的博客](https://blog.csdn.net/gcs_20210916)
![readCountWhite.png](../_resources/readCountWhite.png)5704

[两个或多个*pro*to文件不在一个目录，如果直接*使用**pro**tobuf*_generate_cpp来生成，直接会报错。 若将多个*pro*to文件放到同一个目录...然后import *pro*to文件名即可，虽然这样可以，但显然是不适合大型的项目，那如何解决此类问题可参考本文](https://blog.csdn.net/gcs_20210916/article/details/123992953)

[*Ubuntu*16.04安装*使用**pro**tobuf*2(一)  最新发布](https://blog.csdn.net/qq_42695024/article/details/127494873)

[玫瑰花店的博客](https://blog.csdn.net/qq_42695024)
![readCountWhite.png](../_resources/readCountWhite.png)216

[本人也是*pro**tobuf*新手，因项目需要才接触到的。开始按照官方教程整了一整天最新版的*pro*to3，死活配置不成功。所以直接*使用*了*pro*to2。](https://blog.csdn.net/qq_42695024/article/details/127494873)

[*pro**tobuf*3 *ubuntu*20安装和验证](https://blog.csdn.net/qq_41540355/article/details/115048562)

[qq_41540355的博客](https://blog.csdn.net/qq_41540355)
![readCountWhite.png](../_resources/readCountWhite.png)796

[前序 公司的*pro**tobuf*相关文件实在windows下生成的，本文是个人想学习*Linux*下如何生成相关文件，把自己安装步骤和简单实用做一下记录。 我基于*ubuntu* 20.04.1虚拟机，g++ 9.3.0来讲解。 1. 请确保你的编译器支持*c++*11及其以上，尽量支持*c++*14。 2. 下文在涉及编译器版本编写特定代码处会加以说明。*pro**tobuf*3简介 简介引用这位博主的文章。 步骤 下载及安装 本次我先从github中下载cpp版本的源代码的*pro**tobuf*-cpp-version.tar.gz/](https://blog.csdn.net/qq_41540355/article/details/115048562)

[*ubuntu*下快速*使用**pro**tobuf*](https://blog.csdn.net/yyy269954107/article/details/44464195)

[yyy269954107的专栏](https://blog.csdn.net/yyy269954107)
![readCountWhite.png](../_resources/readCountWhite.png)2020

[什么是*pro**tobuf*简单讲就是一种类似于json,xml的通用数据交换格式，但是效率更高，更省空间，目前官方支持*c++*,java,python,ruby。其他语言有一些第三方做的开发包，需要自己选择比较好的。*Ubuntu*下快速*使用*下载 google或者github 编译 2.1 编译*pro*toc工具，这里可以直接看解压出来的*pro**tobuf*-2.6.0文件夹下的README.txt文件，这](https://blog.csdn.net/yyy269954107/article/details/44464195)

[*Bazel**使用*：编译*pro**tobuf*](https://blog.csdn.net/qq_41347613/article/details/107858226)

[qq_41347613的博客](https://blog.csdn.net/qq_41347613)
![readCountWhite.png](../_resources/readCountWhite.png)2142

[*Bazel*安装参考：（https://blog.csdn.net/qq_41347613/article/details/107856853）](https://blog.csdn.net/qq_41347613/article/details/107858226)

[*ubuntu*下*pro**tobuf*安装*使用*（详解）](https://huaweicloud.csdn.net/63566cf2d3efff3090b5f4d1.html)

[m0_46392035的博客](https://blog.csdn.net/m0_46392035)
![readCountWhite.png](../_resources/readCountWhite.png)4048
[1.安装
1.1安装前的环境
以下几个库都有安装，因为
sudo apt-get install autoconf automake libtool curl make g++ unzip
1.2安装
注意：以下命令在超级用户下执行

sudo apt-get install autoconf automake libtool curl make g++ unzip git clone https://github.com/google/*pro**tobuf*.git #安装子模块 cd p...](https://huaweicloud.csdn.net/63566cf2d3efff3090b5f4d1.html)

[在*Ubuntu*上*使用**pro**tobuf*（*C++*）](https://blog.csdn.net/qq_41950508/article/details/127126215)

[qq_41950508的博客](https://blog.csdn.net/qq_41950508)
![readCountWhite.png](../_resources/readCountWhite.png)499

[*pro**tobuf*  *使用*教程；*pro**tobuf* 入门; *pro*toc 编译；运行可执行文件testread，读出addressbook.data中的信息，也就是反序列化。生成的文件主要包含了结构体成员设置和格式转换的接口，下面展示一下如何*使用*。可以看到最后面，电话本（AddressBook）里面定义了。执行完后，会生成一个.cc和一个.h文件。，可以从里面下载指定语言的压缩包。源文件的写法及更多介绍，可以参考。在上面解压的文件夹下，是有一个。这个文件主要的作用是定义了。代表了电话本可以包含多个。](https://blog.csdn.net/qq_41950508/article/details/127126215)

[*Pro**tobuf* 命令学习](https://blog.csdn.net/yangyi2083334/article/details/115621471)

[domorejojo](https://blog.csdn.net/yangyi2083334)
![readCountWhite.png](../_resources/readCountWhite.png)1489

[以下是学习笔记目录： 文章目录*pro**tobuf* 简介0. *pro**tobuf* 安装1. -*pro*to_path （-I）选项2. --go_out 选项3. 指定哪个*pro*to源头文件4. 执行多个*pro*to文件5. 指定grpc选项，生成grpc功能*pro**tobuf* 简介 微服务很流行，go语言也很流行，这两者加起来的grpc服务也很流行。其中 grpc *使用*的是*Pro**toBuf* 作为内容传输中间件，看下它的说明：*pro*tocol buffers 是一种语言无关、平台无关、可扩展的序列化结构数据的方法，](https://blog.csdn.net/yangyi2083334/article/details/115621471)

[*Pro**tobuf* 学习（二）编译*pro*to文件并测试](https://yaoyz.blog.csdn.net/article/details/93591647)

[学习 & 分享 ~](https://blog.csdn.net/qq_31347869)
![readCountWhite.png](../_resources/readCountWhite.png)5323
[Google 官网上的一个典型例子： // AddressBook.*pro*to package tutorial;

message Person { required string name = 1; required int32 id = 2; optional string email = 3; enum PhoneType { MOBILE = 0; HOM...](https://yaoyz.blog.csdn.net/article/details/93591647)

[*pro*to在*ubuntu*18 *vs**code*  *c++*中如何*使用**cmake*链接](https://blog.csdn.net/qq_44880652/article/details/120033582)

[panpanda](https://blog.csdn.net/qq_44880652)
![readCountWhite.png](../_resources/readCountWhite.png)108

[*pro*to在*ubuntu*18 *vs**code*  *c++*中如何*使用**cmake*链接 find_package(*Pro**tobuf* REQUIRED) include_directories(${*Pro**tobuf*_INCLUDE_DIRS}) include_directories(${*CMAKE*_CURRENT_BINARY_DIR})*pro**tobuf*_generate_cpp(*PRO*TO_SRCS *PRO*TO_HDRS foo.*pro*to)*pro**tobuf*_generate_cpp(*PRO*TO_SRCS *PRO*](https://blog.csdn.net/qq_44880652/article/details/120033582)

[*pro*tocol buffer安装（*Ubuntu*环境变量配置详细&*CMake*编译问题）](https://blog.csdn.net/weixin_37643849/article/details/121989377)

[weixin_37643849的博客](https://blog.csdn.net/weixin_37643849)
![readCountWhite.png](../_resources/readCountWhite.png)495

[*pro*tocol buffer安装（*Ubuntu*环境变量配置详细）](https://blog.csdn.net/weixin_37643849/article/details/121989377)

[Python3.5 中plt无法画出图像](https://blog.csdn.net/WxyangID/article/details/54645355)
[WxyangID的博客](https://blog.csdn.net/WxyangID)
![readCountWhite.png](../_resources/readCountWhite.png)1924

[第一： sudo apt-get install tk-dev 正在读取状态信息… 完成 将会同时安装下列软件： libfontconfig1-dev libfreetype6-dev libice-dev libpng12-dev libpthread-stubs0-dev libsm-dev libx11-dev libx11-doc libxau-dev libxcb1-dev](https://blog.csdn.net/WxyangID/article/details/54645355)

[*ubuntu* 下*pro**tobuf*安装和*使用*](https://blog.csdn.net/kenjianqi1647/article/details/106677040)

[灰太狼的小秘密](https://blog.csdn.net/kenjianqi1647)
![readCountWhite.png](../_resources/readCountWhite.png)3748
[一、*pro**tobuf*安装
1、下载*pro**tobuf*安装包
https://github.com/google/*pro**tobuf*.git
2、解压
3、执行autogen.sh
4、配置
./configure
5、编译
make
6、安装
make install
最后执行

ldconfig # refresh shared library cache.](https://blog.csdn.net/kenjianqi1647/article/details/106677040)

[在电脑上配置*pro**tobuf* + *VS*  *Code* 开发环境](https://blog.csdn.net/qq_37387199/article/details/126562515)

[矢车菊二十七号](https://blog.csdn.net/qq_37387199)
![readCountWhite.png](../_resources/readCountWhite.png)227

[工作需要学习*pro**tobuf* 开发，如果能在 Windows 环境下*使用*更便于练习，于是这篇文章介绍一下如何在 Windows 下借助 *VS*  *Code* 配置 *pro**tobuf* 开发环境。](https://blog.csdn.net/qq_37387199/article/details/126562515)

[*pro*tocol buffer的命令*pro*toc整理](https://blog.csdn.net/mengyi0331/article/details/117252656)

[虫字的博客](https://blog.csdn.net/mengyi0331)
![readCountWhite.png](../_resources/readCountWhite.png)271
[简介

Google*Pro*tocol Buffer( 简称 *Pro**tobuf*) 是 Google 公司内部的混合语言数据标准，目前已经正在*使用*的有超过 48,162 种报文格式定义和超过 12,183 个 .*pro*to 文件。他们用于 RPC 系统和持续数据存储系统。*Pro*tocol Buffers 是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。目前](https://blog.csdn.net/mengyi0331/article/details/117252656)

[*Ubuntu*18.04 *pro**tobuf*版本问题](https://blog.csdn.net/feileibeng4501/article/details/121493376)

[feileibeng4501的博客](https://blog.csdn.net/feileibeng4501)
![readCountWhite.png](../_resources/readCountWhite.png)1002

[这里写自定义目录标题*pro**tobuf*版本卸载旧版本*pro**tobuf*安装新*pro**tobuf*-3.30*pro**tobuf*版本 查看版本*pro*toc --version

安装位置 which *pro*toc
卸载旧版本*pro**tobuf*二进制安装卸载： sudo apt remove lib*pro**tobuf*-dev

源码安装卸载： cd *pro**tobuf*-3.5.0 sudo make uninstall sudo make distclean ./configure

安装新*pro**tobuf*-3.30 降](https://blog.csdn.net/feileibeng4501/article/details/121493376)

[*pro**tobuf*学习笔记](https://blog.csdn.net/weixin_44336181/article/details/120985088)

[weixin_44336181的博客](https://blog.csdn.net/weixin_44336181)
![readCountWhite.png](../_resources/readCountWhite.png)256

[简介*Pro**tobuf*是*Pro*tocol Buffers的简称，它是Google公司开发的一种数据描述语言，是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。*pro**tobuf*是类似与json一样的数据描述语言（数据格式）*pro**tobuf*非常适合于RPC数据交换格式

优缺点

优势： 1：序列化后体积相比Json和XML很小，适合网络传输 2：支持跨平](https://blog.csdn.net/weixin_44336181/article/details/120985088)

### “相关推荐”对你有帮助么？

- ![npsFeel1.png](../_resources/npsFeel1.png)

非常没帮助

- ![npsFeel2.png](../_resources/npsFeel2.png)

没帮助

- ![npsFeel3.png](../_resources/npsFeel3.png)

一般

- ![npsFeel4.png](../_resources/npsFeel4.png)

有帮助

- ![npsFeel5.png](../_resources/npsFeel5.png)

非常有帮助

 ©️2022 CSDN  皮肤主题：代码科技   设计师：Amelia_0503    [返回首页](https://blog.csdn.net/)

- [关于我们](https://www.csdn.net/company/index.html#about)

- [招贤纳士](https://www.csdn.net/company/index.html#recruit)

- [商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)

- [寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)

- ![tel.png](../_resources/tel.png)  400-660-0108

- ![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/u013468614/article/details/107935741mailto:webmaster@csdn.net)

- ![cs.png](../_resources/cs.png)  [在线客服](https://csdn.s2.udesk.cn/im_client/?web_plugin_id=29181)

- 工作时间 8:30-22:00

- [公安备案号11010502030143](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010502030143)

- [京ICP备19004658号](http://beian.miit.gov.cn/publish/query/indexFirst.action)

- [京网文〔2020〕1039-165号](https://csdnimg.cn/release/live_fe/culture_license.png)

- [经营性网站备案信息](https://csdnimg.cn/cdn/content-toolbar/csdn-ICP.png)

- [北京互联网违法和不良信息举报中心](http://www.bjjubao.org/)

- [家长监护](https://download.csdn.net/tutelage/home)

- [网络110报警服务](http://www.cyberpolice.cn/)

- [中国互联网举报中心](http://www.12377.cn/)

- [Chrome商店下载](https://chrome.google.com/webstore/detail/csdn%E5%BC%80%E5%8F%91%E8%80%85%E5%8A%A9%E6%89%8B/kfkdboecolemdjodhmhmcibjocfopejo?hl=zh-CN)

- [账号管理规范](https://blog.csdn.net/blogdevteam/article/details/126135357)

- [版权与免责声明](https://www.csdn.net/company/index.html#statement)

- [版权申诉](https://blog.csdn.net/blogdevteam/article/details/90369522)

- [出版物许可证](https://img-home.csdnimg.cn/images/20220705052819.png)

- [营业执照](https://img-home.csdnimg.cn/images/20210414021142.jpg)

- ©1999-2022北京创新乐知网络技术有限公司

 [![3_u013468614](../_resources/3_u013468614)](https://windses.blog.csdn.net/)

 [windSeS](https://windses.blog.csdn.net/)        ![expertNew.png](../_resources/expertNew.png)

 码龄9年    [![enstaf.png](../_resources/enstaf.png) 之江实验室](https://i.csdn.net/#/uc/profile?utm_source=14998968)

96万+
访问

[![blog6.png](../_resources/blog6.png)](https://blog.csdn.net/blogdevteam/article/details/103478461)

等级

7900
积分

2万+
粉丝

1451
获赞

706
评论

6762
收藏

 ![xiguanyangchengLv1.png](../_resources/xiguanyangchengLv1.png)

 ![fenxiangdaren@240.png](../_resources/fenxiangdaren@240.png)

 ![qiandao10@240.png](../_resources/qiandao10@240.png)

 ![linkedin@240.png](../_resources/linkedin@240.png)

 ![chizhiyiheng@240.png](../_resources/chizhiyiheng@240.png)

 ![github@240.png](../_resources/github@240.png)

 ![maimai@240.png](../_resources/maimai@240.png)

 ![2b55a8d39812403cbc62a20e8607e264.png](../_resources/2b55a8d39812403cbc62a20e8607e264.png)

 ![qixiebiaobing4@240.png](../_resources/qixiebiaobing4@240.png)

 ![blinknewcomer@240.png](../_resources/blinknewcomer@240.png)

 ![yuedu7@240.png](../_resources/yuedu7@240.png)

 ![f19b84c244aa4e6d8bf469b4aff1f98c.png](../_resources/f19b84c244aa4e6d8bf469b4aff1f98c.png)

 [私信](https://im.csdn.net/chat/u013468614)

 [已关注]()

 [![csdn-sou.png](../_resources/csdn-sou.png)]()

### 热门文章

- [fatal: unable to access https:// Failed to connect to: Connection refused|git clone问题（完美解决）![readCountWhite.png](../_resources/readCountWhite.png)103316](https://windses.blog.csdn.net/article/details/83116044)
- [python中plot实现即时数据动态显示方法![readCountWhite.png](../_resources/readCountWhite.png)94862](https://windses.blog.csdn.net/article/details/58689735)
- [海龟绘图简易教程|Turtle for Python![readCountWhite.png](../_resources/readCountWhite.png)58913](https://windses.blog.csdn.net/article/details/82622497)
- [Matlab使用Plot函数实现数据动态显示方法总结![readCountWhite.png](../_resources/readCountWhite.png)47037](https://windses.blog.csdn.net/article/details/58678500)
- [增量学习方法![readCountWhite.png](../_resources/readCountWhite.png)35420](https://windses.blog.csdn.net/article/details/58586577)

### 分类专栏

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-16)   自动驾驶规划与控制入门   付费](https://blog.csdn.net/u013468614/category_11897666.html)  22篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-15)   自动驾驶定位与环境建图入门   付费](https://blog.csdn.net/u013468614/category_11333722.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-11)   轻笔记](https://blog.csdn.net/u013468614/category_10942859.html)  25篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-17)   PaperReading](https://blog.csdn.net/u013468614/category_10301292.html)  11篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-14)   无人驾驶技术系统](https://blog.csdn.net/u013468614/category_9577860.html)  16篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-12)   机器人自主增量式学习](https://blog.csdn.net/u013468614/category_9283432.html)  7篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-9)   人在回路的机器学习](https://blog.csdn.net/u013468614/category_9697663.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-2)   读书笔记](https://blog.csdn.net/u013468614/category_10840554.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-18)   机器学习](https://blog.csdn.net/u013468614/category_6752313.html)  23篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   机器人技术](https://blog.csdn.net/u013468614/category_6750971.html)  7篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   coding](https://blog.csdn.net/u013468614/category_6754321.html)  26篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-19)   ros](https://blog.csdn.net/u013468614/category_7884372.html)  7篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-13)   ubuntu](https://blog.csdn.net/u013468614/category_8007099.html)  16篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   附加知识库](https://blog.csdn.net/u013468614/category_8119091.html)  8篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-7)   debug](https://blog.csdn.net/u013468614/category_8259980.html)  20篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-10)   办公软件安装](https://blog.csdn.net/u013468614/category_8816078.html)  4篇

 [![arrowDownWhite.png](../_resources/arrowDownWhite.png)]()

### 最新评论

- [conda安装GPU版pytorch，结果却是cpu版本[找到问题根源，从容解决]](https://windses.blog.csdn.net/article/details/125910538#comments_24680281)

...  [一只程序猿林:](https://blog.csdn.net/qq_53383206)  conda包管理太混乱了

- [conda安装GPU版pytorch，结果却是cpu版本[找到问题根源，从容解决]](https://windses.blog.csdn.net/article/details/125910538#comments_24680278)

...  [一只程序猿林:](https://blog.csdn.net/qq_53383206)  大道至简

- [python中plot实现即时数据动态显示方法](https://windses.blog.csdn.net/article/details/58689735#comments_24680007)

...  [windSeS:](https://windses.blog.csdn.net/)  把plt.pause(0.01)去掉试试

- [python中plot实现即时数据动态显示方法](https://windses.blog.csdn.net/article/details/58689735#comments_24675896)

...  [weixin_48958409:](https://blog.csdn.net/weixin_48958409)  你好，动态显示的不是在一张图上显示的，它生成了多张图，这咋解决

- [避障算法 - VO、RVO 以及 ORCA (RVO2)[转载]](https://windses.blog.csdn.net/article/details/126427821#comments_24665897)

...  [Coco1011:](https://blog.csdn.net/Coco1011)  博主有vo算法源码吗

### 您愿意向朋友推荐“博客详情页”吗？

- ![npsFeel1.png](../_resources/npsFeel1.png)

强烈不推荐

- ![npsFeel2.png](../_resources/npsFeel2.png)

不推荐

- ![npsFeel3.png](../_resources/npsFeel3.png)

一般般

- ![npsFeel4.png](../_resources/npsFeel4.png)

推荐

- ![npsFeel5.png](../_resources/npsFeel5.png)

强烈推荐

### 最新文章

- [基于采样的规划算法之RRT家族（六）：总结](https://windses.blog.csdn.net/article/details/128413984)

- [基于采样的规划算法之RRT家族（五）：时空 RRT](https://windses.blog.csdn.net/article/details/128309038)

- [基于采样的规划算法之RRT家族（四）：Informed RRT*](https://windses.blog.csdn.net/article/details/128227660)

2022

 [12月4篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=12)

 [11月5篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=11)

 [10月6篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=10)

 [09月3篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=09)

 [08月9篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=08)

 [07月26篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=07)

 [06月3篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=06)

 [05月3篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=05)

 [04月1篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=04)

 [03月1篇](https://windses.blog.csdn.net/?type=blog&year=2022&month=03)

[2021年34篇](https://windses.blog.csdn.net/?type=blog&year=2021&month=12)

[2020年27篇](https://windses.blog.csdn.net/?type=blog&year=2020&month=12)

[2019年41篇](https://windses.blog.csdn.net/?type=blog&year=2019&month=12)

[2018年24篇](https://windses.blog.csdn.net/?type=blog&year=2018&month=12)

[2017年6篇](https://windses.blog.csdn.net/?type=blog&year=2017&month=06)

### 目录

1. [一、protobuf安装](https://blog.csdn.net/u013468614/article/details/107935741#t0)

2. [二、命令行使用protobuf](https://blog.csdn.net/u013468614/article/details/107935741#t1)

3.

    1. [2.1 构建proto文件](https://blog.csdn.net/u013468614/article/details/107935741#t2)

    2. [2.2 生成c++头文件](https://blog.csdn.net/u013468614/article/details/107935741#t3)

    3. [2.3 使用protobuf头文件](https://blog.csdn.net/u013468614/article/details/107935741#t4)

4. [三、cmake下使用protobuf](https://blog.csdn.net/u013468614/article/details/107935741#t5)

5.

    1. [3.1 安装cmake](https://blog.csdn.net/u013468614/article/details/107935741#t6)

    2. [3.2 编写CMakeLists文件](https://blog.csdn.net/u013468614/article/details/107935741#t7)

    3. [3.3 cmake编译与运行](https://blog.csdn.net/u013468614/article/details/107935741#t8)

6. [四、VS code下使用protobuf](https://blog.csdn.net/u013468614/article/details/107935741#t9)

7.

    1. [4.1 安装VS code](https://blog.csdn.net/u013468614/article/details/107935741#t10)

    2. [4.2 编译执行](https://blog.csdn.net/u013468614/article/details/107935741#t11)

8. [五、bazel](https://blog.csdn.net/u013468614/article/details/107935741#t12)
9.

    1. [5.1 安装bazel](https://blog.csdn.net/u013468614/article/details/107935741#t13)

    2. [5.2 构建项目](https://blog.csdn.net/u013468614/article/details/107935741#t14)

    3. [5.3 编写BUILD](https://blog.csdn.net/u013468614/article/details/107935741#t15)

    4. [5.4 编译并执行项目](https://blog.csdn.net/u013468614/article/details/107935741#t16)

![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='3374'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='3375'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='3376'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='3377'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='3378'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='3379'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='3380'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='3381'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='3382'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='3383'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='3384'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='3385'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='3386'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='3387'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='3388'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='3389'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='3390'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='3391'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='3392'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='3393'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)