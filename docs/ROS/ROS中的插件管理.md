# 1 概述

**pluginlib**是一个c++库， 用来从一个ROS功能包中加载和卸载插件(plugin)。插件是指从运行时库中动态加载的类。通过使用Pluginlib，不必将某个应用程序显式地链接到包含某个类的库，Pluginlib可以随时打开包含类的库，而不需要应用程序事先知道包含类定义的库或者头文件。

# 2 使用方式
**需求:**

以插件的方式实现正多边形的相关计算。

**实现流程:**

1. 准备；
    
2. 创建基类；
    
3. 创建插件类；
    
4. 注册插件;
    
5. 构建插件库;
    
6. 使插件可用于ROS工具链;
    
    - 配置xml
        
    - 导出插件
        
7. 使用插件;
    
8. 执行。
## 2.1 创建基类
在 xxx/include/xxx下新建C++头文件: polygon_base.h，所有的插件类都需要继承此基类，内容如下:

```cpp
#ifndef XXX_POLYGON_BASE_H_
#define XXX_POLYGON_BASE_H_

namespace polygon_base
{
  class RegularPolygon
  {
    public:
      virtual void initialize(double side_length) = 0;
      virtual double area() = 0;
      virtual ~RegularPolygon(){}

    protected:
      RegularPolygon(){}
  };
};
#endif
```
**PS:**基类必须提供无参构造函数，所以关于多边形的边长没有通过构造函数而是通过单独编写的initialize函数传参。

## 2.2 创建插件类

在 xxx/include/xxx下新建C++头文件:polygon_plugins.h，内容如下:
```cpp
#ifndef XXX_POLYGON_PLUGINS_H_
#define XXX_POLYGON_PLUGINS_H_
#include <xxx/polygon_base.h>
#include <cmath>

namespace polygon_plugins
{
  class Triangle : public polygon_base::RegularPolygon
  {
    public:
      Triangle(){}

      void initialize(double side_length)
      {
        side_length_ = side_length;
      }

      double area()
      {
        return 0.5 * side_length_ * getHeight();
      }

      double getHeight()
      {
        return sqrt((side_length_ * side_length_) - ((side_length_ / 2) * (side_length_ / 2)));
      }

    private:
      double side_length_;
  };

  class Square : public polygon_base::RegularPolygon
  {
    public:
      Square(){}

      void initialize(double side_length)
      {
        side_length_ = side_length;
      }

      double area()
      {
        return side_length_ * side_length_;
      }

    private:
      double side_length_;

  };
};
#endif

```
## 2.3 注册插件

在 src 目录下新建 polygon_plugins.cpp 文件，内容如下:

```cpp
//pluginlib 宏，可以注册插件类
#include <pluginlib/class_list_macros.h>
#include <xxx/polygon_base.h>
#include <xxx/polygon_plugins.h>

//参数1:衍生类 参数2:基类
PLUGINLIB_EXPORT_CLASS(polygon_plugins::Triangle, polygon_base::RegularPolygon)
PLUGINLIB_EXPORT_CLASS(polygon_plugins::Square, polygon_base::RegularPolygon)
```

该文件会将两个衍生类注册为插件。

## 2.4 构建插件库

```cpp
include_directories(include)
add_library(polygon_plugins src/polygon_plugins.cpp)

```

## 2.5 配置xml

功能包下新建文件:polygon_plugins.xml,内容如下:

```xml
<!-- 插件库的相对路径 -->
<library path="lib/libpolygon_plugins">
  <!-- type="插件类" base_class_type="基类" -->
  <class type="polygon_plugins::Triangle" base_class_type="polygon_base::RegularPolygon">
    <!-- 描述信息 -->
    <description>This is a triangle plugin.</description>
  </class>
  <class type="polygon_plugins::Square" base_class_type="polygon_base::RegularPolygon">
    <description>This is a square plugin.</description>
  </class>
</library>
```

## 2.6导出插件

package.xml文件中设置内容如下:

```xml
<export>
  <xxx plugin="${prefix}/polygon_plugins.xml" />
</export>
```

标签<xxx />的名称应与基类所属的功能包名称一致，plugin属性值为上一步中创建的xml文件。

编译后，可以调用`rospack plugins --attrib=plugin xxx`命令查看配置是否正常，如无异常，会返回 .xml 文件的完整路径，这意味着插件已经正确的集成到了ROS工具链。

## 2.7 使用插件

src 下新建c++文件:polygon_loader.cpp，内容如下:

```cpp
//类加载器相关的头文件
#include <pluginlib/class_loader.h>
#include <xxx/polygon_base.h>

int main(int argc, char** argv)
{
  //类加载器 -- 参数1:基类功能包名称 参数2:基类全限定名称
  pluginlib::ClassLoader<polygon_base::RegularPolygon> poly_loader("xxx", "polygon_base::RegularPolygon");

  try
  {
    //创建插件类实例 -- 参数:插件类全限定名称
    boost::shared_ptr<polygon_base::RegularPolygon> triangle = poly_loader.createInstance("polygon_plugins::Triangle");
    triangle->initialize(10.0);

    boost::shared_ptr<polygon_base::RegularPolygon> square = poly_loader.createInstance("polygon_plugins::Square");
    square->initialize(10.0);

    ROS_INFO("Triangle area: %.2f", triangle->area());
    ROS_INFO("Square area: %.2f", square->area());
  }
  catch(pluginlib::PluginlibException& ex)
  {
    ROS_ERROR("The plugin failed to load for some reason. Error: %s", ex.what());
  }

  return 0;
}
```

## 2.8 执行

修改CMakeLists.txt文件，内容如下:

```
add_executable(polygon_loader src/polygon_loader.cpp)
target_link_libraries(polygon_loader ${catkin_LIBRARIES})
```

编译然后执行:polygon_loader，结果如下:

```
[ INFO] [WallTime: 1279658450.869089666]: Triangle area: 43.30
[ INFO] [WallTime: 1279658450.869138007]: Square area: 100.00
```