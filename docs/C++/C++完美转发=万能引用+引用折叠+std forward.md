https://zhuanlan.zhihu.com/p/335994370
https://zhuanlan.zhihu.com/p/260508149

1. 阅读这篇博文需要了解C++中的左值（lvalue）和右值（rvalue）的概念，详情参见我的另外一篇博文：[C++移动语义及拷贝优化](https://theonegis.github.io/cxx/C-%E7%A7%BB%E5%8A%A8%E8%AF%AD%E4%B9%89%E5%8F%8A%E6%8B%B7%E8%B4%9D%E4%BC%98%E5%8C%96/)
2. 万能引用和完美转发多涉及到模板的使用，如若不是自己写模板，则可不用关心

## 万能引用（Universal Reference）

首先，我们来看一个例子：
```cpp
1. `#include <iostream>`

3. `using std::cout;`
4. `using std::endl;`

7. `template<typename T>`
8. `void func(T& param) {`
9.     `cout << param << endl;`
10. `}`

13. `int main() {`
14.     `int num = 2019;`
15.     `func(num);`
16.     `return 0;`
17. `}`
```

这样例子的编译输出都没有什么问题，但是如果我们修改成下面的调用方式呢？
```cpp
1. `int main() {`
2.     `func(2019);`
3.     `return 0;`
4. `}`
```

则会得到一个大大的编译错误。因为上面的模板函数只能接受左值或者左值引用（左值一般是有名字的变量，可以取到地址的），我们当然可以重载一个接受右值的模板函数，如下也可以达到效果。

```cpp
1. `template<typename T>`
2. `void func(T& param) {`
3.     `cout << "传入的是左值" << endl;`
4. `}`
5. `template<typename T>`
6. `void func(T&& param) {`
7.     `cout << "传入的是右值" << endl;`
8. `}`

12. `int main() {`
13.     `int num = 2019;`
14.     `func(num);`
15.     `func(2019);`
16.     `return 0;`
17. `}`

```

输出结果：

1. `传入的是左值`
2. `传入的是右值`

第一次函数调用的是左值得版本，第二次函数调用的是右值版本。但是，有没有办法只写一个模板函数**即可以接收左值又可以接收右值呢**？

C++ 11中有万能引用（Universal Reference）的概念：**使用`T&&`类型的形参既能绑定右值，又能绑定左值。**

但是注意了：**只有发生类型推导的时候，T&&才表示万能引用**；否则，表示右值引用。

所以，上面的案例我们可以修改为：
```cpp
1. `template<typename T>`
2. `void func(T&& param) {`
3.     `cout << param << endl;`
4. `}`

7. `int main() {`
8.     `int num = 2019;`
9.     `func(num);`
10.     `func(2019);`
11.     `return 0;`
12. `}`

```

## 引用折叠（Universal Collapse）

万能引用说完了，接着来聊引用折叠（Univers Collapse），因为完美转发（Perfect Forwarding）的概念涉及引用折叠。一个模板函数，根据定义的形参和传入的实参的类型，我们可以有下面四中组合：

- 左值-左值 T& & # 函数定义的形参类型是左值引用，传入的实参是左值引用。此时类型推导为int &&
```cpp
1. `template<typename T>`
2. `void func(T& param) {`
3.     `cout << param << endl;`
4. `}`

7. `int main() {`
8.    // `int& num = 2019;` 报错，左值引用不能指向右值
9.       int a = 10;
10.      int& num = a;
11.     `func(num);`
13.     `return 0;`
14. `}`
```
- 左值-右值 T& && # 函数定义的形参类型是左值引用，传入的实参是右值引用。此时类型推导为int & &&
```cpp
1. `template<typename T>`
2. `void func(T& param) {`
3.     `cout << param << endl;`
4. `}`

7. `int main() {`
8.     `int&& num = 2019;`
9.     `func(num);`
11.     `return 0;`
12. `}`
```
- 右值-左值 T&& & # 函数定义的形参类型是右值引用，传入的实参是左值引用。此时类型推导为int && &
```cpp
1. `template<typename T>`
2. `void func(T&& param) {`
3.     `cout << param << endl;`
4. `}`

7. `int main() {`
8.    // `int& num = 2019;` 报错，左值引用不能指向右值
9.       int a = 10;
10.      int& num = a;
11.     `func(num);`
13.     `return 0;`
14. `}`
15. 
```
- 右值-右值 T&& && # 函数定义的形参类型是右值引用，传入的实参是右值引用。。此时类型推导为int && &&
```cpp
1. `template<typename T>`
2. `void func(T&& param) {`
3.     `cout << param << endl;`
4. `}`

7. `int main() {`
8.     `int&& num = 2019;`
9.     `func(num);`
11.     `return 0;`
12. `}`
```

但是C++中不允许对引用再进行引用，对于上述情况的处理有如下的规则：

所有的折叠引用最终都代表一个引用，要么是左值引用，要么是右值引用。规则是：**如果任一引用为左值引用，则结果为左值引用。否则（即两个都是右值引用），结果为右值引用**。

即就是**前面三种情况代表的都是左值引用，退化成int&，而第四种代表的右值引用，退化成int &&**。

## 完美转发（Perfect Forwarding）

下面接着说完美转发（Perfect Forwarding），首先，看一个例子：
```cpp
1. `#include <iostream>`

3. `using std::cout;`
4. `using std::endl;`

6. `template<typename T>`
7. `void func(T& param) {`
8.     `cout << "传入的是左值" << endl;`
9. `}`
10. `template<typename T>`
11. `void func(T&& param) {`
12.     `cout << "传入的是右值" << endl;`
13. `}`

16. `template<typename T>`
17. `void warp(T&& param) {`
18.     `func(param);`
19. `}`

22. `int main() {`
23.     `int num = 2019;`
24.     `warp(num);`
25.     `warp(2019);`
26.     `return 0;`
27. `}`
```

猜一下，上面的输出结果是什么？

1. `传入的是左值`
2. `传入的是左值`

是不是和我们预期的不一样，下面我们来分析一下原因：

`warp()`函数本身的形参是一个万能引用，即可以接受左值又可以接受右值；第一个`warp()`函数调用实参是左值，所以，`warp()`函数中调用`func()`中传入的参数也应该是左值；第二个warp()函数调用实参是右值，根据上面所说的引用折叠规则，warp()函数接收的参数类型是右值引用，那么为什么却调用了调用func()的左值版本了呢？这是因为在warp()函数内部，左值引用类型变为了右值，因为参数有了名称，我们也通过变量名取得变量地址。

那么问题来了，怎么保持函数调用过程中，变量类型的不变呢？这就是我们所谓的“完美转发”技术，在C++11中通过`std::forward()`函数来实现。我们修改我们的`warp()`函数如下：
```cpp
1. `template<typename T>`
2. `void warp(T&& param) {`
3.     `func(std::forward<T>(param));`
4. `}`
```

则可以输出预期的结果。

例子2
```cpp
template<typename T>
void print(T & t){
    std::cout << "Lvalue ref" << std::endl;
}

template<typename T>
void print(T && t){
    std::cout << "Rvalue ref" << std::endl;
}

template<typename T>
void testForward(T && v){ 
    print(v);//v此时已经是个左值了,永远调用左值版本的print
    print(std::forward<T>(v)); //本文的重点
    print(std::move(v)); //永远调用右值版本的print

    std::cout << "" << std::endl;
}

int main(int argc, char * argv[])
{
    int x = 1;
    testForward(x); //实参为左值
    testForward(std::move(x)); //实参为右值
}
```

```text
Lvalue ref
Lvalue ref
Rvalue ref

Lvalue ref
Rvalue ref
Rvalue ref

```