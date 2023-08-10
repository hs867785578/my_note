[小鱼教程](https://d2lros2foxy.fishros.com/#/)

## 1 ROS2的功能包创建
同样需要ros_ws/src文件夹，在src下创建功能包

三种方式创建功能包：

- ament_python，适用于python程序
- cmake，适用于C++
- ament_cmake，适用于C++程序,是cmake的增强版

命令：
```bash
ros2 pkg create <package-name>  --build-type  {cmake,ament_cmake,ament_python}  --dependencies <依赖名字>
```

在ros2中，python包的setup.py对应于cmake的cmakelists.txt，在ros1中python不需要setup.py,而是使用cmakelist.txt的install

在ros2中，launch文件不再是xml，而是.launch.py的python文件，更加灵活

## 2功能包的查看

**2.列出可执行文件**

列出所有

```bash
ros2 pkg executables
```

列出`turtlesim`功能包的所有可执行文件

```bash
ros2 pkg executables turtlesim
```

![image-20210915172702101](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/2.%E5%8A%9F%E8%83%BD%E5%8C%85%E4%B8%8E%E5%B7%A5%E4%BD%9C%E7%A9%BA%E9%97%B4/imgs/image-20210915172702101.png)

列出所有的包

```bash
ros2 pkg list
```

输出某个包所在路径的前缀

```bash
ros2 pkg prefix  <package-name>
```

比如小乌龟

```bash
ros2 pkg prefix turtlesim
```

## 3 功能包的构建
安装 colcon
```bash
sudo apt-get install python3-colcon-common-extensions
```
构建命令colcon build

构建完成后，在`src`同级目录我们应该会看到 `build` 、 `install` 和 `log` 目录:

```
├── build
├── install
├── log
└── src

```
- `build` 目录存储的是中间文件。对于每个包，将创建一个子文件夹，在其中调用例如CMake
- `install` 目录是每个软件包将安装到的位置。默认情况下，每个包都将安装到单独的子目录中。
- `log` 目录包含有关每个colcon调用的各种日志信息。

### 只编译一个包

```bash
colcon build --packages-select YOUR_PKG_NAME 
```

### 不编译测试单元

```bash
colcon build --packages-select YOUR_PKG_NAME  --cmake-args -DBUILD_TESTING=0
```

### 运行编译的包的测试

```bash
colcon test
```

### 允许通过更改src下的部分文件来改变install（重要）

每次调整 python 脚本时都不必重新build了

```bash
colcon build --symlink-install
```

## 4 CPP编写ROS2节点(面向过程版本)

注意：只有在ros2中才有用面向对象编写代码的方法，（继承Node类）

### 1.创建工作空间和功能包

#### 1.1 工作空间

工作空间就是文件夹，所以很简单。

```bash
cd d2lros2/chapt2/
mkdir -p chapt2_ws/src/
```

#### 1.2 创建example_cpp功能包

创建example_cpp功能包，使用ament-cmake作为编译类型，并为其添加rclcpp依赖。

```bash
cd chapt2_ws/src
ros2 pkg create example_cpp --build-type ament_cmake --dependencies rclcpp
```

大家可以手写一下这个代码，感受一下。现在小鱼来讲一讲这条命令的含义和参数。

- pkg create 是创建包的意思
- --build-type 用来指定该包的编译类型，一共有三个可选项`ament_python`、`ament_cmake`、`cmake`
- --dependencies 指的是这个功能包的依赖，这里小鱼给了一个ros2的python客户端接口`rclpy`

打开终端，进入`chapt2_ws/src`运行上面的指令，创建完成后的目录结构如下：

```
.
└── src
    └── example_cpp
        ├── CMakeLists.txt
        ├── include
        │   └── example_cpp
        ├── package.xml
        └── src

5 directories, 2 files
```

### 2.创建节点

接着我们在`example_cpp/src`下创建一个`node_01.cpp`文件，创建完成后的目录结构如下：

![image-20220603171631334](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/4.%E4%BD%BF%E7%94%A8RCLCPP%E7%BC%96%E5%86%99%E8%8A%82%E7%82%B9/imgs/image-20220603171631334.png)

### 3.编写代码

#### 3.1 编写代码

继续跟着小鱼一起输入代码，输入的时候可以边输边理解。

```c++
#include "rclcpp/rclcpp.hpp"


int main(int argc, char **argv)
{
    /* 初始化rclcpp  */
    rclcpp::init(argc, argv);
    /*产生一个node_01的节点*/
    auto node = std::make_shared<rclcpp::Node>("node_01");
    // 打印一句自我介绍
    RCLCPP_INFO(node->get_logger(), "node_01节点已经启动.");
    /* 运行节点，并检测退出信号 Ctrl+C*/
    rclcpp::spin(node);
    /* 停止运行 */
    rclcpp::shutdown();
    return 0;
}
```

#### 3.2 修改CmakeLists

在`node_01.cpp`中输入上面的内容后，还需要修改一下CMakeLists.txt。将其添加为可执行文件，并使用`install`指令将其安装到`install`目录。

在CmakeLists.txt最后一行加入下面两行代码。

```
add_executable(node_01 src/node_01.cpp)
ament_target_dependencies(node_01 rclcpp)
```

添加这两行代码的目的是让编译器编译node_01这个文件，接着在上面两行代码下面添加下面的代码。

```
install(TARGETS
  node_01
  DESTINATION lib/${PROJECT_NAME}
)
```

### 4.编译运行节点

在`chapt2_ws`下依次输入下面的命令

#### 4.1 编译节点

```bash
colcon build
```

#### 4.2 source环境

```bash
source install/setup.bash
```

#### 4.3 运行节点

```bash
ros2 run example_cpp node_01
```

不出意外，你可以看到

![image-20220603172524241](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/4.%E4%BD%BF%E7%94%A8RCLCPP%E7%BC%96%E5%86%99%E8%8A%82%E7%82%B9/imgs/image-20220603172524241.png)

### 5.测试

当节点运行起来后，可以再尝试使用`ros2 node list` 指令来查看现有的节点。这个时候你应该能看到：

![image-20220603172729457](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/4.%E4%BD%BF%E7%94%A8RCLCPP%E7%BC%96%E5%86%99%E8%8A%82%E7%82%B9/imgs/image-20220603172729457.png)

### 6.总结

至此，相信你已经掌握了如何编写一个C++版本的ros2节点了，但是这仅仅是编写ROS2节点方式之一，相比之下，小鱼更推荐你使用面向对象的方式编写节点，在进阶篇小鱼将会向你展示其写法。


## 5 python编写ros2节点（面向过程版本）

### 1.创建python功能包

创建一个名字叫做`example_py` python版本的功能包。

```bash
cd chapt2/chapt2_ws/src/
ros2 pkg create example_py  --build-type ament_python --dependencies rclpy
```

创建完成后的目录结构

```
.
├── example_py
│   └── __init__.py
├── package.xml
├── resource
│   └── example_py
├── setup.cfg
├── setup.py
└── test
    ├── test_copyright.py
    ├── test_flake8.py
    └── test_pep257.py

3 directories, 8 files
```

### 2.编写程序

编写ROS2节点的一般步骤

```
1. 导入库文件
2. 初始化客户端库
3. 新建节点
4. spin循环节点
5. 关闭客户端库
```

在`example_py`下创建`node_02.py`接着我们开始编写代码。跟着小鱼一起边理解输入下面的代码，注释不用输。

```python
import rclpy
from rclpy.node import Node

def main(args=None):
    """
    ros2运行该节点的入口函数
    编写ROS2节点的一般步骤
    1. 导入库文件
    2. 初始化客户端库
    3. 新建节点对象
    4. spin循环节点
    5. 关闭客户端库
    """
    rclpy.init(args=args) # 初始化rclpy
    node = Node("node_02")  # 新建一个节点
    node.get_logger().info("大家好，我是node_02.")
    rclpy.spin(node) # 保持节点运行，检测是否收到退出指令（Ctrl+C）
    rclpy.shutdown() # 关闭rclpy
```

代码编写完成用Crtl+S进行保存。接着修改`setup.py`。

```python
    entry_points={
        'console_scripts': [
            "node_02 = example_py.node_02:main"
        ],
    },
)
```

`setup.py`这段配置是声明一个ROS2的节点，声明后使用`colcon build`才能检测到，从而将其添加到`install`目录下。

完成上面的工作后，就可以编译运行了。

### 3.编译运行节点

打开vscode终端，进入`town_ws`

#### 3.1 编译节点

```bash
cd chapt2/chapt2_ws/
colcon build
```

> ```
> --- stderr: example_py                   
> /usr/lib/python3/dist-packages/setuptools/command/install.py:34: SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.
>   warnings.warn(
> ---
> ```
> 
> 如果在编译中看到上述错误没关系，不影响使用，ros2官方正在修复。 错误原因是setuptools版本太高造成，使用下面的指令可以进行版本的回退。
> 
> ```
> sudo pip install setuptools==58.2.0 --upgrade
> ```

#### 3.2 source环境

```bash
source install/setup.bash
```

### 3.3 运行节点

```bash
ros2 run example_py node_02
```

运行结果

![image-20220603174606170](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/5.%E4%BD%BF%E7%94%A8RCLPY%E7%BC%96%E5%86%99%E8%8A%82%E7%82%B9/imgs/image-20220603174606170.png)

### 4.测试

当节点运行起来后，可以再尝试使用`ros2 node list` 指令来查看现有的节点。这个时候你应该能看到：

![image-20220603174623023](https://d2lros2foxy.fishros.com/humble/chapt2/get_started/5.%E4%BD%BF%E7%94%A8RCLPY%E7%BC%96%E5%86%99%E8%8A%82%E7%82%B9/imgs/image-20220603174623023.png)

这说明你的节点已经运行起来了。

### 5.总结

本节我们学习了使用Python在工作空间的功能包里编写一个节点，代码是相同的，但是多了一些配置。

当然除了使用这种方法编写一个节点，还有其他方式，小鱼将其放到了进阶篇来讲。


## 6 CPP编写ROS2节点(面向对象版本)

在`d2lros2/chapt2/chapt2_ws/src/example_cpp/src`下新建`node_03.cpp`，接着输入下面的代码。

```c++
#include "rclcpp/rclcpp.hpp"

/*
    创建一个类节点，名字叫做Node03,继承自Node.
*/
class Node03 : public rclcpp::Node
{

public:
    // 构造函数,有一个参数为节点名称
    Node03(std::string name) : Node(name)
    {
        // 打印一句
        RCLCPP_INFO(this->get_logger(), "大家好，我是%s.",name.c_str());
    }

private:
   
};

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    /*产生一个node_03的节点*/
    auto node = std::make_shared<Node03>("node_03");
    /* 运行节点，并检测退出信号*/
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

接着修改`CMakeLists.txt`，添加下方代码。

```
add_executable(node_03 src/node_03.cpp)
ament_target_dependencies(node_03 rclcpp)

install(TARGETS
  node_03
  DESTINATION lib/${PROJECT_NAME}
)
```

接着即可自行编译测试

```
colcon build --packages-select example_cpp
source install/setup.bash
ros2 run example_cpp node_03
```

![image-20220604003321895](https://d2lros2foxy.fishros.com/humble/chapt2/advanced/2.%E4%BD%BF%E7%94%A8%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%96%B9%E5%BC%8F%E7%BC%96%E5%86%99ROS2%E8%8A%82%E7%82%B9/imgs/image-20220604003321895.png)


## 7 python编写ros2节点（面向对象版本）

在`d2lros2/d2lros2/chapt2/chapt2_ws/src/example_py/example_py`下新建`node_04.py`，输入下面的代码

```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node


class Node04(Node):
    """
    创建一个Node04节点，并在初始化时输出一个话
    """
    def __init__(self,name):
        super().__init__(name)
        self.get_logger().info("大家好，我是%s!" % name)


def main(args=None):
    rclpy.init(args=args) # 初始化rclpy
    node = Node04("node_04")  # 新建一个节点
    rclpy.spin(node) # 保持节点运行，检测是否收到退出指令（Ctrl+C）
    rclpy.shutdown() # 关闭rclpy
```

接着修改`setup.py`

```python
    entry_points={
        'console_scripts': [
            "node_02 = example_py.node_02:main",
            "node_04 = example_py.node_04:main"
        ],
    },
```

> 注意格式和结尾的`,`符号，`console_scripts`是个数组。

编译测试

```
colcon build --packages-select example_py
source install/setup.bash
ros2 run example_py node_04
```

![image-20220604003653900](https://d2lros2foxy.fishros.com/humble/chapt2/advanced/2.%E4%BD%BF%E7%94%A8%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%96%B9%E5%BC%8F%E7%BC%96%E5%86%99ROS2%E8%8A%82%E7%82%B9/imgs/image-20220604003653900.png)


## 8 launch文件的编写

[通过实例学习ros2的launch](https://github.com/chargerKong/learning_ros2_launch_by_example)

### 1.Launch启动工具介绍

#### 1.1 问题描述

对于一个机器人系统来说，往往由很多个不同功能的节点组成，启动一个机器人系统时往往需要启动多个节点，同时根据应用场景和机器人的不同，每个节点还会有不同的配置项。

如果每个节点我们都开一个新终端，敲`ros2 run`指令并写一堆参数，这是多么浪费生命且令人绝望的事情。

除了启动，你会发现，一个个关闭也是很难受的。

### 1.2 解决方案
可不可以编写一个类似于脚本的文件来管理节点的启动呢？

ROS2设计时就为我们想好了，为我们设计了一套完整的语法和规则的文件来帮助我们组织节点的启动，这个武器就叫launch文件。

**launch文件允许我们同时启动和配置多个包含 ROS 2 节点的可执行文件**

> 在ROS1中launch文件只有一种格式以.launch结尾的xml文档，不熟悉的同学写起来被xml语法折磨的死去活来。不过在ROS2中不要担心，因为在ROS2你可以使用Python代码来编写launch文件

### 2.编写第一个ROS2的launch文件

#### 2.1 三种编写launch文件的方法

ROS2的launch文件有三种格式，python、xml、yaml。其中ROS2官方推荐的时python方式编写launch文件。 原因在于，相较于XML和YAML，**Python是一个编程语言，更加的灵活，我们可以利用Python的很多库来做一些其他工作**（比如创建一些初始化的目录等）。

> 除了灵活还有另外一个原因是ros2/launch（一般launch共功能）和ros2/launch_ros（ROS 2 launch的特性）是用 Python 编写的，我们使用python编写launch文件可以使用 XML 和 YAML 中不能用的launch功能。 要说使用python版本的launch有什么坏处，那就是写起来比yaml要冗余

#### 2.2 使用Python编写Launch

我们的目标是编写一个launch文件，最后使用launch指令，同时启动服务端和客户端节点。

##### 2.2.1 创建功能包和launch文件

创建文件夹和功能包，接着touch一个launch文件，后缀为`.py`。

```
mkdir -p chapt4/chapt4_ws/src
cd chapt4/chapt4_ws/src
ros2 pkg create robot_startup --build-type ament_cmake --destination-directory src
mkdir -p src/robot_startup/launch
touch src/robot_startup/launch/example_action.launch.py
```

##### 2.2.2 启动多个节点的示例

我们需要导入两个库，一个叫做LaunchDescription，用于对launch文件内容进行描述，一个是Node，用于声明节点所在的位置。

> 注意这里要定一个名字叫做`generate_launch_description`的函数，ROS2会对该函数名字做识别。

```python
# 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    """launch内容描述函数，由ros2 launch 扫描调用"""
    action_robot_01 = Node(
        package="example_action_rclcpp",
        executable="action_robot_01"
    )
    action_control_01 = Node(
        package="example_action_rclcpp",
        executable="action_control_01"
    )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription(
        [action_robot_01, action_control_01])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
```

##### 2.2.3 将launch文件拷贝到安装目录

如果你编写完成后直接编译你会发现install目录下根本没有你编写的launch文件，后续launch自然也找不到这个launch文件。

因为我们用的是`ament_cmake`类型功能包，所以这里要使用cmake命令进行文件的拷贝

```
install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME})
```

如果是`ament_python`功能包版，需要修改setup.py

```python

from setuptools import setup
from glob import glob
import os

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name, 'launch'), glob('launch/*.launch.py')),
    ],
    },
)
```

##### 2.2.4 编译测试

使用colcon指令编译我们的程序

```
colcon build
```

编译完成后，在`chapt5/chapt5_ws/install/robot_startup/share/robot_startup/launch`目录下你应该就可以看到被cmake拷贝过去的launch文件了。

接着运行`

```
# source 第四章的工作目录，这样才能找到对应的节点，不信你可以不source试试
source ../../chapt4/chapt4_ws/install/setup.bash
source install/setup.bash
ros2 launch robot_startup example_action.launch.py
# 新终端
ros2 node list #即可看到两个节点
```

![image-20220616135356671](https://d2lros2foxy.fishros.com/humble/chapt5/get_started/1.%E5%90%AF%E5%8A%A8%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-Launch/imgs/image-20220616135356671.png)

### 3 添加参数&修改命名空间
接着我们尝试使用launch运行参数节点，并通过launch传递参数，和给节点以不同的命名空间。

新建`chapt5/chapt5_ws/src/robot_startup/launch/example_param_rclcpp.launch.py`。

编写内容如下

```
# 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    """launch内容描述函数，由ros2 launch 扫描调用"""
    parameters_basic1 = Node(
        package="example_parameters_rclcpp",
        namespace="rclcpp",
        executable="parameters_basic",
        parameters=[{'rcl_log_level': 40}]
    )
    parameters_basic2 = Node(
        package="example_parameters_rclpy",
        namespace="rclpy",
        executable="parameters_basic",
        parameters=[{'rcl_log_level': 50}]
    )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription(
        [parameters_basic1, parameters_basic2])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
```

编译运行测试

```
# source 第四章的工作目录，这样才能找到对应的节点，不信你可以不source试试
source ../../chapt4/chapt4_ws/install/setup.bash
source install/setup.bash
ros2 launch robot_startup example_param_rclcpp.launch.py
# 新终端
ros2 node list #即可看到两个节点
```

![image-20220616140109862](https://d2lros2foxy.fishros.com/humble/chapt5/get_started/1.%E5%90%AF%E5%8A%A8%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-Launch/imgs/image-20220616140109862.png)
