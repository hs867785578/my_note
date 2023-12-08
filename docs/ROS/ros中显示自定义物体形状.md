
资源网站:https://www.cgtrader.com

rviz中可以给marker附加mesh文件，让rviz显示。直接上代码。
注意，如果想显示dae文件原来的颜色，marker.mesh_use_embedded_materials = true;这段代码一定要加上。
```cpp
#include <ros/ros.h>
#include <visualization_msgs/Marker.h>
#include "tf/transform_datatypes.h"

int main(int argc, char** argv) {
  ros::init(argc, argv, "car_model_node");
  ros::NodeHandle n;
  ros::Publisher marker_pub =
      n.advertise<visualization_msgs::Marker>("visualization_marker", 1);

  visualization_msgs::Marker marker;
  marker.header.frame_id = "map";
  marker.header.stamp = ros::Time::now();

  marker.ns = "basic_shapes";
  marker.id = 0;

  marker.type = visualization_msgs::Marker::MESH_RESOURCE;
  marker.action = visualization_msgs::Marker::MODIFY;

  geometry_msgs::Pose pose;
  pose.position.x = 0.0;
  pose.position.y = 0.0;
  pose.position.z = 0.0;

  tf::Quaternion q;
  q = tf::createQuaternionFromRPY(0.0, 0.0, 0.0);
  pose.orientation.x = q.x();
  pose.orientation.y = q.y();
  pose.orientation.z = q.z();
  pose.orientation.w = q.w();
  marker.pose = pose;

  tf::Quaternion q1(0.0, -0.7071, -0.7071, 0.0);
  quaternionTFToMsg(tf::Quaternion(pose.orientation.x, pose.orientation.y,
                                   pose.orientation.z, pose.orientation.w) *
                        q1,
                    marker.pose.orientation);

  marker.scale.x = 1.0;
  marker.scale.y = 1.0;
  marker.scale.z = 1.0;

  // marker.color.r = 1.0;
  // marker.color.g = 1.0;
  // marker.color.b = 1.0;
  marker.color.a = 1.0;
  marker.mesh_use_embedded_materials = true;

  marker.mesh_resource = "package://car_model/meshes/bmw_x5.dae";

  while (ros::ok()) {
    marker_pub.publish(marker);
    ros::spinOnce();
  }
}


```

```
cmake_minimum_required(VERSION 3.0.2)
project(car_model)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON) 
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  visualization_msgs
  tf
)

catkin_package(
 INCLUDE_DIRS include
 CATKIN_DEPENDS roscpp rospy std_msgs visualization_msgs tf
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(car_model_node src/main.cpp)
target_link_libraries(car_model_node ${catkin_LIBRARIES})
```

package.xml
```
<?xml version="1.0"?>
<package format="2">
  <name>car_model</name>
  <version>0.0.0</version>
  <description>The car_model package</description>

  <maintainer email="hanshuo@todo.todo">hanshuo</maintainer>

  <license>TODO</license>

  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>rospy</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>visualization_msgs</build_depend>
  <build_depend>tf</build_depend>
  <build_export_depend>visualization_msgs</build_export_depend>
  <build_export_depend>roscpp</build_export_depend>
  <build_export_depend>rospy</build_export_depend>
  <build_export_depend>std_msgs</build_export_depend>
  <exec_depend>roscpp</exec_depend>
  <exec_depend>rospy</exec_depend>
  <exec_depend>std_msgs</exec_depend>
  <exec_depend>tf</exec_depend>
  <exec_depend>visualization_msgs</exec_depend>


  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- Other tools can request additional information be placed here -->

  </export>
</package>

```