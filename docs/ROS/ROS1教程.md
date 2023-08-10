[赵虚左教程](http://www.autolabor.com.cn/book/ROSTutorials/di-2-zhang-ros-jia-gou-she-ji/22hua-ti-tong-xin/213-hua-ti-tong-xin-zhi-python-shi-xian.html)


- ros1自带了一些工具比如catkin_create_pkg、catkin_make
- 同时也可以用catkin tools的一些工具完成同样的事情，比如 catkin create pkg、catkin build

## 1 ROS1的功能包创建
同样需要ros_ws/src文件夹，在src下创建功能包

### 1.1用ros自带的catkin_create_pkg
命令：
```bash
# catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

### 1.2用catkin_tools

catkin create pkg
命令：
```bash
# catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
catkin create pkg beginner_tutorials --catkin-deps std_msgs rospy roscpp
```

## 2 功能包的查看

命令：
```bash
rospkg list
```

## 3 功能包的构建

### 3.1使用catkin_make

```
catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=Yes -DCMAKE_BUILD_TYPE=Debug/Release
```

### 3.2使用catkin_tools

安装：
```
sudo apt-get install python-catkin-tools
```
编译所有节点(只要在工作空间中任何一级目录都能用，它会自动向上搜索工作空间)：
```
catkin config --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_BUILD_TYPE=Debug
catkin build
```

单独编译一个包
```
catkin build package_name
```

清理所有包的编译
```
catkin clean
```

清除某一个包的编译
```
catkin clean package_name
```

## 4 Cpp编写ros节点

### 1.发布方

```c++
/*
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)

         PS: 二者需要设置相同的话题


    消息发布方:
        循环发布信息:HelloWorld 后缀数字编号

    实现流程:
        1.包含头文件 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 ROS 句柄
        4.实例化 发布者 对象
        5.组织被发布的数据，并编写逻辑发布数据

*/
// 1.包含头文件 
#include "ros/ros.h"
#include "std_msgs/String.h" //普通文本类型的消息
#include <sstream>

int main(int argc, char  *argv[])
{   
    //设置编码
    setlocale(LC_ALL,"");

    //2.初始化 ROS 节点:命名(唯一)
    // 参数1和参数2 后期为节点传值会使用
    // 参数3 是节点名称，是一个标识符，需要保证运行后，在 ROS 网络拓扑中唯一
    ros::init(argc,argv,"talker");
    //3.实例化 ROS 句柄
    ros::NodeHandle nh;//该类封装了 ROS 中的一些常用功能

    //4.实例化 发布者 对象
    //泛型: 发布的消息类型
    //参数1: 要发布到的话题
    //参数2: 队列中最大保存的消息数，超出此阀值时，先进的先销毁(时间早的先销毁)
    ros::Publisher pub = nh.advertise<std_msgs::String>("chatter",10);

    //5.组织被发布的数据，并编写逻辑发布数据
    //数据(动态组织)
    std_msgs::String msg;
    // msg.data = "你好啊！！！";
    std::string msg_front = "Hello 你好！"; //消息前缀
    int count = 0; //消息计数器

    //逻辑(一秒10次)
    ros::Rate r(1);

    //节点不死
    while (ros::ok())
    {
        //使用 stringstream 拼接字符串与编号
        std::stringstream ss;
        ss << msg_front << count;
        msg.data = ss.str();
        //发布消息
        pub.publish(msg);
        //加入调试，打印发送的消息
        ROS_INFO("发送的消息:%s",msg.data.c_str());

        //根据前面制定的发送贫频率自动休眠 休眠时间 = 1/频率；
        r.sleep();
        count++;//循环结束前，让 count 自增
        //暂无应用
        ros::spinOnce();
    }


    return 0;
}
```

### 2.订阅方

```c++
/*
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)


    消息订阅方:
        订阅话题并打印接收到的消息

    实现流程:
        1.包含头文件 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 ROS 句柄
        4.实例化 订阅者 对象
        5.处理订阅的消息(回调函数)
        6.设置循环调用回调函数

*/
// 1.包含头文件 
#include "ros/ros.h"
#include "std_msgs/String.h"

void doMsg(const std_msgs::String::ConstPtr& msg_p){
    ROS_INFO("我听见:%s",msg_p->data.c_str());
    // ROS_INFO("我听见:%s",(*msg_p).data.c_str());
}
int main(int argc, char  *argv[])
{
    setlocale(LC_ALL,"");
    //2.初始化 ROS 节点:命名(唯一)
    ros::init(argc,argv,"listener");
    //3.实例化 ROS 句柄
    ros::NodeHandle nh;

    //4.实例化 订阅者 对象
    ros::Subscriber sub = nh.subscribe<std_msgs::String>("chatter",10,doMsg);
    //5.处理订阅的消息(回调函数)

    //     6.设置循环调用回调函数
    ros::spin();//循环读取接收的数据，并调用回调函数处理

    return 0;
}
```

### 3.配置 CMakeLists.txt

```cmake
add_executable(Hello_pub
  src/Hello_pub.cpp
)
add_executable(Hello_sub
  src/Hello_sub.cpp
)

target_link_libraries(Hello_pub
  ${catkin_LIBRARIES}
)
target_link_libraries(Hello_sub
  ${catkin_LIBRARIES}
)
```

## 5 python编写ros节点

### 1.发布方

```python
#! /usr/bin/env python
"""
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)

         PS: 二者需要设置相同的话题


    消息发布方:
        循环发布信息:HelloWorld 后缀数字编号

    实现流程:
        1.导包 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 发布者 对象
        4.组织被发布的数据，并编写逻辑发布数据


"""
#1.导包 
import rospy
from std_msgs.msg import String

if __name__ == "__main__":
    #2.初始化 ROS 节点:命名(唯一)
    rospy.init_node("talker_p")
    #3.实例化 发布者 对象
    pub = rospy.Publisher("chatter",String,queue_size=10)
    #4.组织被发布的数据，并编写逻辑发布数据
    msg = String()  #创建 msg 对象
    msg_front = "hello 你好"
    count = 0  #计数器 
    # 设置循环频率
    rate = rospy.Rate(1)
    while not rospy.is_shutdown():

        #拼接字符串
        msg.data = msg_front + str(count)

        pub.publish(msg)
        rate.sleep()
        rospy.loginfo("写出的数据:%s",msg.data)
        count += 1
```

### 2.订阅方

```python
#! /usr/bin/env python
"""
    需求: 实现基本的话题通信，一方发布数据，一方接收数据，
         实现的关键点:
         1.发送方
         2.接收方
         3.数据(此处为普通文本)


    消息订阅方:
        订阅话题并打印接收到的消息

    实现流程:
        1.导包 
        2.初始化 ROS 节点:命名(唯一)
        3.实例化 订阅者 对象
        4.处理订阅的消息(回调函数)
        5.设置循环调用回调函数



"""
#1.导包 
import rospy
from std_msgs.msg import String

def doMsg(msg):
    rospy.loginfo("I heard:%s",msg.data)

if __name__ == "__main__":
    #2.初始化 ROS 节点:命名(唯一)
    rospy.init_node("listener_p")
    #3.实例化 订阅者 对象
    sub = rospy.Subscriber("chatter",String,doMsg,queue_size=10)
    #4.处理订阅的消息(回调函数)
    #5.设置循环调用回调函数
    rospy.spin()
```

### 3.添加可执行权限

终端下进入 scripts 执行:`chmod +x *.py`

### 4.配置 CMakeLists.txt

```
catkin_install_python(PROGRAMS
  scripts/talker_p.py
  scripts/listener_p.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```


## 6 launch文件的编写

1. 选定功能包右击 ---> 添加 launch 文件夹
    
2. 选定 launch 文件夹右击 ---> 添加 launch 文件
    
3. 编辑 launch 文件内容
    
    ```
    <launch>
        <node pkg="helloworld" type="demo_hello" name="hello" output="screen" />
        <node pkg="turtlesim" type="turtlesim_node" name="t1"/>
        <node pkg="turtlesim" type="turtle_teleop_key" name="key1" />
    </launch>
    ```
    
    - node ---> 包含的某个节点
        
    - pkg -----> 功能包
        
    - type ----> 被运行的节点文件
        
    - name --> 为节点命名
        
    - output-> 设置日志的输出目标
        
4. 运行 launch 文件
    
    `roslaunch 包名 launch文件名`
    
5. 运行结果: 一次性启动了多个节点

launch中的标签：node、include（包含其他launch文件）、param（参数）、rosparam（从yaml文件导入的参数）、arg（动态传参，类似于函数的参数，可以增强launch文件的灵活性）

include
```xml
## hello_movebase.launch
<launch>
    <!-- 启动仿真环境加载小车 -->
    <include file="$(find hello_navigation)/launch/myAGV.launch" />
    <!-- 启动map_server -->
    <include file="$(find hello_navigation)/launch/params_mapserver.launch" />
    <!-- 启动AMCL -->
    <include file="$(find hello_navigation)/launch/params_amcl.launch" />
    <!-- 注意map_server和AMCL可以被动态的slam所实时代替，可以对比一下hello_dynamicslam.launch和本文件-->
    <!-- 启动movebase -->
    <include file="$(find hello_navigation)/launch/params_movebase.launch" />
</launch>
```

node、rosparam:
```xml
## params_movebase.launch
<launch>
    <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen" clear_params="true">
        <rosparam file="$(find hello_navigation)/config/costmap_common_params.yaml" command="load" ns="global_costmap" />
        <rosparam file="$(find hello_navigation)/config/costmap_common_params.yaml" command="load" ns="local_costmap" />
        <rosparam file="$(find hello_navigation)/config/local_costmap_params.yaml" command="load" />
        <rosparam file="$(find hello_navigation)/config/global_costmap_params.yaml" command="load" />
        <rosparam file="$(find hello_navigation)/config/teb_local_planner_params.yaml" command="load" />
        <param name="base_local_planner" value="teb_local_planner/TebLocalPlannerROS" />
    </node>
</launch>
```

param
```xml
## params_amcl.launch
<launch>
    <node pkg="amcl" type="amcl" name="amcl" output="screen">
        <!-- Publish scans from best pose at a max of 10 Hz -->
        <param name="odom_model_type" value="diff"/><!-- 里程计模式为差分 -->
        <param name="odom_alpha5" value="0.1"/>
        <param name="transform_tolerance" value="0.2" />
        <param name="gui_publish_rate" value="10.0"/>
        <param name="laser_max_beams" value="30"/>
        <param name="min_particles" value="500"/>
        <param name="max_particles" value="5000"/>
        <param name="kld_err" value="0.05"/>
        <param name="kld_z" value="0.99"/>
        <param name="odom_alpha1" value="0.2"/>
        <param name="odom_alpha2" value="0.2"/>
        <!-- translation std dev, m -->
        <param name="odom_alpha3" value="0.8"/>
        <param name="odom_alpha4" value="0.2"/>
        <param name="laser_z_hit" value="0.5"/>
        <param name="laser_z_short" value="0.05"/>
        <param name="laser_z_max" value="0.05"/>
        <param name="laser_z_rand" value="0.5"/>
        <param name="laser_sigma_hit" value="0.2"/>
        <param name="laser_lambda_short" value="0.1"/>
        <param name="laser_lambda_short" value="0.1"/>
        <param name="laser_model_type" value="likelihood_field"/>
        <!-- <param name="laser_model_type" value="beam"/> -->
        <param name="laser_likelihood_max_dist" value="2.0"/>
        <param name="update_min_d" value="0.2"/>
        <param name="update_min_a" value="0.5"/>
        <param name="odom_frame_id" value="odom"/><!-- 里程计坐标系 -->
        <param name="base_frame_id" value="base_coordinate"/><!-- 添加机器人基坐标系 -->
        <param name="global_frame_id" value="map"/><!-- 添加地图坐标系 -->  
        <param name="resample_interval" value="1"/>
        <param name="transform_tolerance" value="0.1"/>
        <param name="recovery_alpha_slow" value="0.0"/>
        <param name="recovery_alpha_fast" value="0.0"/>
    </node>
</launch>
```

arg:

```xml
<launch>
    <!-- 设置地图的配置文件 -->
    <arg name="map" default="hello_map.yaml" />
    <!-- 运行地图服务器，并且加载设置的地图-->
    <node name="map_server" pkg="map_server" type="map_server" args="$(find hello_navigation)/map/$(arg map)"/>
</launch>
```