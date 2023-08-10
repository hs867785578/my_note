1.tuple的创建

2.get的使用

在 C++11 的标准中，我们可以通过 std::get< Index >(tuple) （以常量整数值为索引号）操作 tuple 中的参数，而到了 C++14 之后的标准，新增了 std::get< Type >(tuple) （以数据类型为索引）的方式操作 tuple 中的参数。

![](images/C++中的tuple、get、tie_image_1.png)


![](images/C++中的tuple、get、tie_image_2.png)

1. tie的用法
![](images/C++中的tuple、get、tie_image_3.png)