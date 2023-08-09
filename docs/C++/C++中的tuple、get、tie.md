1.tuple的创建

2.get的使用

在 C++11 的标准中，我们可以通过 std::get< Index >(tuple) （以常量整数值为索引号）操作 tuple 中的参数，而到了 C++14 之后的标准，新增了 std::get< Type >(tuple) （以数据类型为索引）的方式操作 tuple 中的参数。

![](../_resources/28449f8b66532663027f4638b8f615f9.png)

![](../_resources/7264aca4a0cab6af67b32b26d0d96657.png)

1. tie的用法
![](../_resources/7014a1c32130621258c1d505e1a67e9a.png)