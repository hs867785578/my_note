在ros中：
cmakelists.txt和package.xml是用来声明**包名称**的。

在cpp文件中的main函数中，ros::init(argc, argv, "node_name")是用来声明**节点名称**的。特别地，该函数要求**node_name不能带有命名空间namespace，也就是不能取/nodename/namespace这种名字**。这种节点初始化方法一般与NodeHandle句柄一起使用。

在cpp文件中的main函数中，ros::NodeHandle node_handle(“~”)是用来声名该节点的**命名空间**的。一般NodeHandle紧跟ros::init定义。

```cpp
# 初始化一个NodeHandle：
ros::NodeHandle nh;
# 初始化一个NodeHandle，指定全局空间/global
ros::NodeHandle nh("/global");
# 初始化一个NodeHandle，指定普通空间normal
ros::NodeHandle nh("normal")
# 初始化一个NodeHandle，指定私有空间private
ros::NodeHandle nh("~private")
```

全局命名空间：
```cpp
# 假设node所在的命名空间为/ns，使用全局命名空间，
ros::init(argv, argc, "node_name")
ros::NodeHandle nh("/global")
# 此时使用nh创建名字为my_topic的话题
nh.advertise<...>("my_topic")
# 实际话题的名字为 /global/my_topic
# 此时nh设置的各种东西名字都与node name和node namespace无关
```

相对命名空间（默认ns为全局命名空间/）：
```cpp
# 假设node所在的命名空间为/ns，使用普通命名空间，
ros::init(argv, argc, "node_name")
ros::NodeHandle nh("normal")
# 此时使用nh创建名字为my_topic的话题
nh.advertise<...>("my_topic")
# 实际话题的名字为 /ns/normal/my_topic
# 此时nh设置的各种东西名字都与node name无关，但与node所在的命名空间有关

```

私有命名空间：
```cpp
# 假设node所在的命名空间为/ns，使用私有命名空间，
ros::init(argv, argc, "node_name")
ros::NodeHandle nh("~private")
# 此时使用nh创建名字为my_topic的话题
nh.advertise<...>("my_topic")
# 实际话题的名字为 /ns/node_name/private/my_topic
# 此时nh设置的各种东西名字与node name和ns相关

```

```cpp
# 一、使用默认构造函数
ros::NodeHandle nh;
# 等价于 
# 二、使用普通命名空间 ""
ros::NodeHandle nh("");
# 句柄一和二的命名空间为 /node_namespace/

# 三、使用私有命名空间 "~"
ros::NodeHandle nh("~")
# 句柄三的命名空间为 /node_namespace/node_name/

```
https://blog.csdn.net/qq_41035283/article/details/120679237

注意：如果**topic中直接使用了全局命名空间，那么，此时不管句柄是否私有化，发布的话题都是全局的 /person_info**
```cpp
ros::Publisher person_info_pub = n.advertise<learning_topic::Person>("/person_info", 10);
//实际话题的名字为 /person_info
```

补充demo
```cpp
// launch 文件中 ns"node_namespace"
ros::init(argc, argv, "node_name"); // node name
ros::NodeHandle n; //n 命名空间为/node_namespace
ros::NodeHandle n1("sub"); // n1命名空间为/node_namespace/sub
ros::NodeHandle n2(n1,"sub2");// n2命名空间为/node_namespace/sub/sub2
ros::NodeHandle pn1("~"); //pn1 命名空间为/node_namespace/node_name
ros::NodeHandle pn2("~sub"); //pn2 命名空间为/node_namespace/node_name/sub
ros::NodeHandle pn3("~/sub"); //pn3 命名空间为/node_namespace/node_name/sub
ros::NodeHandle gn("/global"); // gn 命名空间为/global
```


# 1. 概述
ros中的命名空间只要分为三类：全局命名空间、相对命名空间、私有命名空间，用于区分不同的topic

# 2.全局名称

**以“/”开头的节点名**
话题名表示其属于全局命名空间，被称为全局名称，在整个程序中是独一无二的，具有绝对的意味

例如
/turtlesim
/turtle1/cmd_vel
/A/B/C/D/E

当以**全局名称订阅一个topic的时候，节点只会寻找这个全局名称，比如你订阅了/A/cmd_vel**，节点不会接收
/cmd_vel或者/B/cmd_vel或者/X/A/cmd_vel

# 3. 相对名称

**相对名称没有“**/**”**

例如  
**turtlesim**  
**turtle1/cmd_vel**  
**A/B/C/D/E**

ROS会为**相对名称提供一个默认的命名空间**(**默认为“/”**)，但你可以为每个节点手动设置默认命名空间。主要有命令行和launch两种方式：

1.命令行方式：
    大部分ROS程序接受叫__ns的命令行参数，此参数将为程序指定一个默认命名空间。
    例如：rosrun turtlesim turtlesim_node __ns:=/my
    turtlesim变为/my/turtlesim

2.一般在launch文件里使用命名空间
```
<launch>
	<group ns="ABC">
    <include file="$(find velodyne_pointcloud)/launch/32e_points.launch">
    </include>
	</group>
</launch>
```

这里就将整个launch里的节点都带上了默认命名空间/ABC(包括订阅的节点)

为什么要使用相对名称
１．当有很多层命名空间的时候，写这些话题名会让你很难受，比如/a/b/c/d/e/cmd_vel，你可能花费大把的时间用来写名称，并且容易导致失误。这时候，一个好的选择是使用相对名称。
２．方便移植，当你移植别人的节点到你的程序的时候，如果别人使用的都是全局名称，你不得不在其基础上进行修改来防止节点发生名称冲突。如果别人使用的是相对名称，你可以通过定义不同的命名空间来快速的移植，也有有利于系统结构的清晰。

# 4. 私有名称

**私有名称以一个波浪字符（“～”）开头**  
私有名称也不能够完全确定它们自身所在的命名空间，和相对名称的区别是：**私有名称使用的不是默认命名空间，而是用它们的节点名称作为命名空间**

例如
```cpp
ros::init(argc, argv, "name");//声明node name
ros::NodeHandle nh("~");   //创造句柄的时候指明了其命名空间为**私有命名空间**
ros::Subscriber sub = nh.subscribe("topic", ...);  //订阅的话题是 name/topic
ros::Publisher  pub = nh.advertise("topic", ...);  //发布的话题是 nmae/topic
```

**为什么要使用私有名称**  
每个节点内部都有一些资源，这些资源只与本节点相关，不会与其他节点打交道，这时候为了安全我们使用私有名称。