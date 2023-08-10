、1. **C++中的继承分为public、protected、private，三种继承关系的继承关系如下图所示**
![](images/C++中的继承_image_1.png)

1. **要想实现多态只能用public继承，如下图所示**
![](images/C++中的继承_image_2.png)

      [C++的私有继承和EBO_fl2011sx的博客-CSDN博客](https://blog.csdn.net/fl2011sx/article/details/124038512)

       主要原因如下：
实际上，C++中的继承，实际上是把父类全部继承过来，也就是说直接把父类在子类中定义：

class A{
}；
class B：A{
}；
实际上，B中存在一个A的对象：
class B{
A a；
}；

那么，这样就很好理解了，如果是private继承，那么有：
class B{
private：
    A a；
}；

所以对于A中原来的public，protected，private元素，在B中取交集，分别变为private，private，不可见（记住就行了，虽然不可见，但是是实际存在B中的内存的）

那么，这样就很好理解了，如果是protected继承，那么有：
class B{
protected：
    A a；
}；

所以对于A中原来的public，protected，private元素，在B中取交集，分别变为protected，protected，不可见（记住就行了，虽然不可见，但是是实际存在B中的内存的）

那么，这样就很好理解了，如果是public继承，那么有：
class B{
public：
    A a；
}；

所以对于A中原来的public，protected，private元素，在B中取交集，分别变为public，protected，不可见（记住就行了，虽然不可见，但是是实际存在B中的内存的）

所以说如果实现多态，把子类的地址赋给父类型的指针（即把子类中父类的部分，传给外边的父类指针），即
A *a = new B（）；

**如果是public继承，B中的A是public的，所以可以在外部取地址，自然就可以把B中A的地址赋给A的指针。**
**如果是protected继承，B中的A是protected的，不能在外部取地址，所以不能实现把B中A的地址赋给A的指针，自然不能实现多态。**
**如果是private继承，B中的A是private的，不能在外部取地址，所以不能实现把B中A的地址赋给A的指针，自然不能实现多态。**

1. **C++对象内存模型的很好的教程**

https://www.bilibili.com/video/BV1v64y1q7JT/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=d31a858cc26ae1ffa19e14058b339f40

1. **父类和子类同名的情况**
并不覆盖，可以通过父类：：成员名的方式，在子类中访问父类的同名对象，如下图所示。
![](images/C++中的继承_image_3.png)

其实自己的类中，也可以用类名+：：访问，只是可以省略。
![](images/C++中的继承_image_4.png)

![](images/C++中的继承_image_5.png)

对于C来说，其对象模型是
class C{
B b
}
对于B来说
class B{
A a
}
所以，在孙类C中访问A中的**变量，需要加B：：A：：，或者C：：B：：A：：**
同理，在C中访问A的方法也是一样。
<font color=red>注意：如果ABC中同时又test()方法，那么C::test()，访问的是C的test，C::B::test()访问的是B的test。</font>

1. **在子类中可以通过pubulic/protected/private + using 来修改继承过来的父类的成员变量的权限。**
![](images/C++中的继承_image_6.png)

1. **继承的对象模型（父类和子类的this指针是同一个）**
![](images/C++中的继承_image_7.png)![](images/C++中的继承_image_8.png)

1. **C++中继承的默认规则：父类由父类自己的构造函数初始化，子类构造函数只初始化剩下的部分。**

2. **如果子类重载了父类的同名函数，那么父类的所有同名函数都会被隐藏，如下边这种情况，如果不注释，会报错，这是因为子类重载了func，所以默认调用子类的func函数，但是子类的func又不需要参数，所以报错。**

**![](images/C++中的继承_image_9.png)**

1. **虚拟继承**
虚拟继承主要是为了解决菱形继承的情况使用的。
![](images/C++中的继承_image_10.png)

1. **在定义虚函数时，只需要在声明时增加virtual，在函数具体定义时不能加**
![](images/C++中的继承_image_11.png)
1. **在子类中定义虚函数时，可以和override连用，同时不声明virtual(因为父类声明过了)**

![](images/C++中的继承_image_12.png)
基类
![](images/C++中的继承_image_13.png)
派生类
![](images/C++中的继承_image_14.png)
如果基类加上virtual就不会报错了。

1. **父类的析构函数一定是virtual的。**

如下图所示，如果~AA()不是virtual的，那么delete a其实调用的是~AA（）。实际上，由于我们想要实现的是多态，所以，我们希望调用的是~BB（），因为~BB（）中会自动调用~AA（）。这时，把~AA（）声明为virtual，那么~BB（）自动变为virtual，基类的指针a就会指向~BB（）。

![](images/C++中的继承_image_15.png)

1. **一个好的习惯，delete后要指向nullptr**
![](images/C++中的继承_image_16.png)

1. **纯虚函数和抽象类**
![](images/C++中的继承_image_17.png)
![](images/C++中的继承_image_18.png)

1. **动态类型转换**
![](images/C++中的继承_image_19.png)

1. **typeinfo（int）.name()返回的类型名称，根据编译器不同而不同，msvc返回int，gcc返回i**
**![](images/C++中的继承_image_20.png)**

![](images/C++中的继承_image_21.png)
![](images/C++中的继承_image_22.png)
![](images/C++中的继承_image_23.png)