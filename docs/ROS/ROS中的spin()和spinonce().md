# 1 概述
spinonce()清空当前回调函数队列，spin()是spinonce()的循环调用。

无论是spin()还是spinOnce()都是用于处理回调函数，这里的回调函数是指注册在ros默认callbackQueue的回调函数。在spin()中，将它看成一个循环，就是在不断在循环中处理所有注册的回调函数。而spinOnce()则是每一次执行的时候处理一次**全部回调函数（回调函数数量=当前队列中的数据数量）**。

# 2 常见用法

```cpp
ros::init(argc, argv, "my_node");
ros::NodeHandle nh;
ros::Subscriber sub = nh.subscribe(...);
...
ros::spin();
```

spinonce主动清空回调函数序列，可以主动设置清空的时间
```cpp
ros::Rate r(10); // 10 hz
while (should_continue)
{
  ... do some work, publish some messages, etc. ...
  ros::spinOnce();
  r.sleep();
}
```

# 3 内部细节

一次 ros::spinOnce()会清空当前回调函数队列，执行一次spinonce()，会依次执行队列中的所有回调函数（**回调函数数量=当前所有订阅者消息队列缓存的消息数之和**）。比如有两个订阅者，在执行spinonce时，订阅者1的消息队列有3个消息，订阅者2消息队列中有5消息，那么会依次执行8个回调函数。
在依次执行每个回调函数时，会将（最新的）消息队列中的队首消息传入到相应的回调函数中。

举个例子：
talker.cpp
```cpp
//talker node
/*
 * @Descripttion: 
 * @version: 
 * @Author: wjh 
 * @Date: 2021-06-26 10:45:46
 * @LastEditors: Wen JiaHao
 * @LastEditTime: 2021-12-06 15:33:24
 */
#include "ros/ros.h"
#include "std_msgs/String.h"
#include "beginner_tutorials/AddTwoInts.h"
#include <ros/console.h>
#include <sstream>

int main(int argc, char **argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter1", 10);
  ros::Publisher chatter_pub2 = n.advertise<std_msgs::String>("chatter2", 10);
  printf("cesh222i-------------\n");
  ros::Rate loop_rate(1);/*每秒发一次数据*/
  int count = 0;
  while (ros::ok())
  {
    std_msgs::String msg;
    std::stringstream ss;
    std::stringstream ss2;
    ss << "hello world chatter1: " << count;
    ss2 << "hello world chatter2: " << count;
    msg.data = ss.str();
    chatter_pub.publish(msg);
    msg.data = ss2.str();
    chatter_pub2.publish(msg);
    ros::spinOnce();
    loop_rate.sleep();
    ++count;
  }

```

lisenter.cpp
```cpp
#include "std_msgs/String.h"
#include "beginner_tutorials/AddTwoInts.h"
#include <pthread.h>
using namespace std;

int ccount=0;
ros::ServiceClient liftAutoClient;

void chatterCallback1(const std_msgs::String::ConstPtr& msg)
{
  ROS_INFO("I heard: [%s]", msg->data.c_str());
  ccount++;
  sleep(5);
}

void chatterCallback2(const std_msgs::String::ConstPtr& msg)
{
  ROS_INFO("I heard: [%s]", msg->data.c_str());
}

int main(int argc, char **argv)
{
  ros::init(argc, argv, "listener");
  ros::NodeHandle n;
  pthread_t idpccount;
  liftAutoClient=n.serviceClient<beginner_tutorials::AddTwoInts>("Add");
  //pthread_create(&idpccount, NULL, pPrint, NULL);
  ros::Subscriber sub = n.subscribe("chatter2", 10, chatterCallback1);
  ros::Subscriber sub2 = n.subscribe("chatter1", 10, chatterCallback2);
  sleep(5);//启动后等待5秒
  printf("after 5 s\n");
  ros::spinOnce();//执行一次回调，你认为是连续5个消息，实际因为每次处理时间为5秒，后面几次的消息被挤出了缓冲区，读到的不是最开始的数据。
  printf("hello\n");
  return 0;
}

```

自后结果：
```bash
wjh@wjh_honor:~/catkin_ws$ rosrun beginner_tutorials listener 
after 5 s
[ INFO] [1639467223.128549614]: I heard: [hello world chatter1: 3]
[ INFO] [1639467223.128777020]: I heard: [hello world chatter2: 3]
[ INFO] [1639467228.129120414]: I heard: [hello world chatter1: 4]
[ INFO] [1639467228.129264989]: I heard: [hello world chatter2: 4]
[ INFO] [1639467233.129756292]: I heard: [hello world chatter1: 8]
[ INFO] [1639467233.129906551]: I heard: [hello world chatter2: 8]
[ INFO] [1639467238.130446465]: I heard: [hello world chatter1: 13]
[ INFO] [1639467238.130619063]: I heard: [hello world chatter2: 13]
[ INFO] [1639467243.131248178]: I heard: [hello world chatter1: 18]
[ INFO] [1639467243.131403797]: I heard: [hello world chatter2: 18]
hello
wjh@wjh_honor:~/catkin_ws$ 
```

休眠5秒后执行listen节点的spinOnce时，sub1和sub2接收缓冲区每个缓存了5个数据
（7->6->5->4->3），因此会执行10次消息回调函数。

另外，可以注意到计数值不是期望的3，4，5，6，7，因为每次打印耗时5s，回调函数会取最新的消息队列的队首。回调2打印3->回调1打印3（顺序不定），5s后，消息队列变为了：
（12->11->10->9->8->7->6->5->4）
回调2打印4->回调1打印4（顺序不定），5s后，消息队列变为了：
（17->16->15->14->13->12->11->10->9->8->7->6->5）由于队列长度为10，所以实际上队列为
（17->16->15->14->13->12->11->10->9->8）
所以第三次打印时，缓冲区的数据5，6，7已经被挤出队列抛弃了。
此时回调2打印8->回调1打印8（顺序不定）

sleep(5)时，消息队列有5个消息，但是每个消息回调执行时间也是5秒，因此下一次执行回调的消息可能早已不是当初的消息。所以之后设计消息接收时，耗时的操作不要放在回调函数里。


同理当队列满时，那么一次spinonce执行的回调函数数量为20。
如果延迟很低，队列没有阻塞，回调函数执行的速度非常快，那么队列应该只保留1个数据，那么一次spinonce执行的回调函数数量为2。

