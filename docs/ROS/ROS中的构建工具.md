## 1构建工具分类

### 1.1 catkin_make(ros1)
最早的构建工具，该构建工具**仅调用CMake一次**，并使用CMake的函数 add_subdirectory()在单一环境中处理所有软件包。但该方法也有明显的缺点。归因于单一环境，所有软件包中的所有函数名称、目标和测试都共享单个命名空间，而当软件包的规模更大时很容易导致冲突。

构建工具catkin_make 支持以下软件包的构建：
● 带有package.xml文件的 ROS 1 catkin软件包。

### 1.2 catkin_make_isolated(ros1)

构建工具catkin_make_isolated也是由包含ROS 1构建系统的ROS软件包catkin提供的。该构建工具是在catkin_make之后开发的，用于解决在单一CMake环境中构建多个软件包所涉及的问题。

该构建工具仅支持基于CMake的软件包，并使用CMake软件包通用的命令序列按照拓扑顺序构建每个软件包，其命令顺序为：cmake、make、make install。

相对于catkin_make来说，不再共享单个命名空间，各个包相互隔离，会多次调用cmake命令，每个软件包一次。
虽然每个软件包都可以并行化其目标的构建，但即使它们不存在相互（递归）依赖关系，也会按顺序处理各个软件包。构建速度较慢。

构建工具catkin_make_isolated支持以下软件包的构建：
● 带有package.xml文件的 ROS 1 catkin软件包。
● 带有package.xml文件的纯CMake软件包。


### 1.3 catkin_tools(ros1)
构建工具[catkin_tools](https://link.zhihu.com/?target=https%3A//catkin-tools.readthedocs.io/)是由用于构建ROS 1软件包的独立Python软件包提供的。该构建工具是在catkin_make / catkin_make_isolated之后开发的，用于并行构建多个软件包并提供显著的可用性改进。该构建工具支持构建CMake软件包并且单独构建这些软件包，以及支持跨软件包并行处理过程。

使用方式为catkin build，该构建方式同时具有1.1和1.2的优点

构建工具catkin_tools 支持以下软件包的构建：
● 带有package.xml文件的ROS 1 catkin软件包。
● 带有package.xml文件的纯CMake软件包。

### 1.4 ament_tools(ros2) 已弃用
构建工具ament_tools是由一个独立的、用于构建ROS 2软件包的Python 3软件包提供的。该构建工具是为引导ROS 2项目而开发的，因此仅针对Python 3且适用于Linux、MacOS和Windows。
- **除了支持CMake软件包构建之外，该构建工具还支持Python软件包的构建**，并且可以在无显式软件包清单的情况下推断其元信息（例如用于构建FastRTPS软件包）。
- 像 **catkin_make_isolated 和 catkin_tools一样，该构建工具会执行“隔离”构建**（每个软件包需要一次CMake调用），并且还可以对相互没有（递归）依赖关系的软件包进行并行构建（像catkin_tools一样）。虽然该构建工具比catkin_tools覆盖了更多的构建系统和平台，但它没有catkin_tools构建工具所拥有的任何可用性功能，如配置文件、输出处理等。

构建工具ament_tools支持以下软件包的构建：
● 带有package.xml文件的 ROS 2 ament_cmake软件包（仅支持格式 2）。
● 带有package.xml文件的纯CMake软件包。

● 没有清单文件的纯CMake软件包（从CMake文件中提取软件包名称和依赖包）。

● 带有package.xml文件的Python软件包。
● 没有清单文件的Python软件包（从setup.py文件中提取软件包名称和依赖包）。

### 1.2 colcon(ros2)

colcon是为ROS 2开发的新构建工具，与catkin_tools类似，但目前可用性功能较少，但能够在所有主要平台（Linux、macOS、Windows）上构建任何类型的软件包（catkin、ament、CMake、Python setuptools、gradle、bazel、cargo等

## 2.Colcon使用进阶

基础篇中小鱼带你用gcc编译了ROS2节点。对你来说，使用CMake（GCC或Makefile）和 Python Setup打包工具依然可以完成ROS2代码的编译，那为什么还需要Colcon呢？

带着这个问题，我们来进一步的学习Colcon。

### 1.ROS生态中的构建系统和构建工具

#### 1.1 构建系统与构建工具(构建工具调用构建系统)

两者的区分点在于针对的对象不同，**构建系统**之针对一个单独的包进行构建，而**构建工具**重点在于按照依赖关系依次调用构建系统完成一系列功能包的构建。

ROS中用到的构建系统：`CMake`、`ament_cmake`、`catkin`、`Python setuptools`。

ROS中用到的构建工具：`colcon`、`catkin_make`、`catkin_make_isolated`、`catkin_tools`。

很明显colcon作为构建工具，通过调用`CMake`、`Python setuptools`完成构建。

#### 1.2 常见构建系统

##### 1.2.1 CMake

[CMake](https://cmake.org/) 是一个跨平台构建系统生成器。项目使用独立于平台的文件指定其生成过程。用户通过使用CMake为其平台上的本机工具生成构建系统来构建项目。

通常用法有：`cmake`、`make`、`make intsall`

##### 1.2.2 Python setuptools

`setuptools`是Python包的打包常用工具。Python 包使用文件来描述依赖项，以及如何构建和安装内容。在ROS2中，功能包可以是“普通”Python包，而在ROS1中，任何Python功能都是从CMake文件触发setup.py进行打包。

通常的用法有：`python setup.py`

##### 1.2.3 catkin

[catkin](http://wiki.ros.org/catkin)基于CMake，并提供了一组方便的函数，使编写CMake包更容易。它自动生成 CMake 配置文件以及 pkg 配置文件。它还提供了注册不同类型测试的函数。

#### 1.3 常见构建工具

##### 1.3.1 catkin_make

该工具仅调用 CMake 一次，并使用 CMake 的函数在单个上下文中处理所有包。虽然这是一种有效的方法，因为所有包中的所有目标都可以并行化，但它具有明显的缺点。由于所有函数名称、目标和测试都共享一个命名空间，并且规模更大，这很容易导致冲突。

##### 1.3.2 colcon

![image-20220604133925270](https://d2lros2foxy.fishros.com/humble/chapt2/advanced/3.Colcon%E4%BD%BF%E7%94%A8%E8%BF%9B%E9%98%B6/imgs/image-20220604133925270.png)

[colcon](http://colcon.readthedocs.io/)是一个命令行工具，用于改进构建，测试和使用多个软件包的工作流程。它自动化了流程，处理了订购并设置了使用软件包的环境。

[colcon 文档](https://colcon.readthedocs.io/en/released/index.html)

##### 1.3.3 ament_tools

`ament_tools`由用于构建 ROS 2 包的独立 Python 3 包提供。它是为引导ROS 2项目而开发的，因此仅针对Python 3，并且可以在Linux，MacOS和Windows上运行。

`ament_tools`支持构建以下软件包：

- 带有`package.xml`文件的 ROS 2 包。
- 带有`package.xml`普通的 CMake 包。
- 没有清单文件的普通 CMake 包（从 CMake 文件中提取包名称和依赖项）。
- 带有`package.xml`文件的 Python 包。
- 没有清单文件的 Python 包（从`setup.py`文件中提取包名称和依赖项）。

### 2.Colcon构建进阶

我们平时用的最多的场景是编译功能包，所以这里小鱼重点介绍build时候的一些参数。

#### 2.1 build参数

##### 2.1.0 构建指令

- `--packages-select` ，仅生成单个包（或选定的包）。
- `--packages-up-to`，构建选定的包，包括其依赖项。
- `--packages-above`，整个工作区，然后对其中一个包进行了更改。此指令将重构此包以及（递归地）依赖于此包的所有包。

##### 2.1.1.指定构建后安装的目录

可以通过 `--build-base`参数和`--install-base`，指定构建目录和安装目录。

##### 2.1.2.合并构建目录

`--merge-install`，使用 作为所有软件包的安装前缀，而不是安装基中的软件包特定子目录。--install-base

如果没有此选项，每个包都将提供自己的环境变量路径，从而导致非常长的环境变量值。

使用此选项时，添加到环境变量的大多数路径将相同，从而导致环境变量值更短。

##### 2.1.3.符号链接安装

启用`--symlink-install`后将不会把文拷贝到install目录，而是通过创建符号链接的方式。

##### 2.1.4.错误时继续安装

启用`--continue-on-error`，当发生错误的时候继续进行编译。

##### 2.1.5 CMake参数

`--cmake-args`，将任意参数传递给CMake。与其他选项匹配的参数必须以空格为前缀。

##### 2.1.6 控制构建线程

- `--executor EXECUTOR`，用于处理所有作业的执行程序。默认值是根据所有可用执行程序扩展的优先级选择的。要查看完整列表，请调用 colcon extensions colcon_core.executor --verbose`。
    
    - `sequential` [`colcon-core`]
        
        一次处理一个包。
        
    - `parallel` [`colcon-parallel-executor`]
        
        处理多个作业**平行**.
        
- --parallel-workers NUMBER
    
    - 要并行处理的最大作业数。默认值为 [os.cpu_count()](https://docs.python.org/3/library/os.html#os.cpu_count) 给出的逻辑 CPU 内核数。

##### 2.1.7 开启构建日志

使用`--log-level`可以设置日志级别，比如`--log-level info`。