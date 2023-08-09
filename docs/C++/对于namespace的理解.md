namespace为命名空间。
对于Apollo这种大型项目，namespace一般都是嵌套的。
那么，用变量时的原则：
**1.如果两个文件的namespace相同，那么可以不加额外声明。如：**
文件A：
```cpp
namespace apollo{
namespace module{
namespace planning{
  int a=3；
}
}
}
```


文件B（引入了a的头文件）：
```cpp
namespace apollo{
namespace module{
namespace planning{
 cout<< a；
}
}
}
```

此时，文件B是可以直接用文件A中的变量的，不需要额外声明。因为他们在同一个命名空间下。

**2.如果两个文件的namespace不同，按照开始出现不同的命名空间，进行声明。如：**

```cpp
文件A：
namespace apollo{
namespace module{
namespace prediction{
  int a=3；
}
}
}

文件B：
namespace apollo{
namespace module{
namespace planning{

 cout<<  prediction::a；//本来应该声明apollo::module::prediction::a，因为apollo module这两个命名空间相同，所以不需要再额外声明，只需要从prediction开始声明就好。

}
}
}

```


再比如：
```cpp
文件A：
namespace apollo{
namespace common{
  int a=3；
}
}

文件B：
namespace apollo{
namespace module{
namespace planning{

 cout<<common::a；//本来应该声明apollo::common::a，因为apollo这两个命名空间相同，所以不需要再额外声明，只需要从common开始声明就好。

}
}
}

```

再比如：
```cpp
文件A：
namespace apollo{
namespace module{
namespace planning{
  int a=3；
}
}
}

文件B：
namespace apollo{
namespace common{

 cout<<module::planning::a；//本来应该声明apollo::module::planning::a，因为apollo这两个命名空间相同，所以不需要再额外声明，只需要从common开始声明就好。

}
}

```


**3.查找命名空间时是嵌套查找的，比如2.1中在**namespace apollo{namespace module{namespace planning{**需要找**prediction::a，先找apollo::prediction::a，如果没有，找  apollo::module::prediction::a，如果再没有找apollo::module::planning::prediction::a，以此类推。

**4.匿名命名空间**
文件A：
namespace{
int a = 3;
}
文件B：
为了防止跨文件调用，文件B是调用不了文件a中的int a变量的。作用相当于static，标准中建议用namespace 替换static