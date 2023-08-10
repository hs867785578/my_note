https://www.bilibili.com/video/BV1Tv4y1N73A/?spm_id_from=333.788.recommend_more_video.-1&vd_source=d31a858cc26ae1ffa19e14058b339f40

- SFINAE（substitution failure is not an error）：替换失败不报错，英文读法为se fei ni a，他的意思是假如有一个特化会导致编译时错误(即出现编译失败)，只要还有别的选择可以被选择，那么就无视这个特化错误而去选择另外的可选选择。
	- 当调用模板函数时编译器会根据传入参数推导最合适的模板函数，在这个推导过程中**如果某一个或者某几个模板函数推导出来是编译无法通过的，只要有一个可以正确推导出来，那么那几个推导得到的可能产生编译错误的模板函数并不会引发编译错误**。这段话很绕，我们接下来用代码说明一下，一看便知。
	-
```c++
struct Test {
    typedef int foo;
};

template <typename T> 
void f(typename T::foo) {} // Definition #1

template <typename T> 
void f(T) {}               // Definition #2

int main() {
    f<Test>(10); // Call #1.
    f<int>(10);  // Call #2. Without error (even though there is no int::foo) thanks to SFINAE.
}

```
  这是wiki上SFINAE的一个经典示例，注释已经解释的相当明白，由于推导模板函数过程中可以找到一个正确的版本，所以即时int::foo是一个语法错误，但是编译器也不会报错。这就是SFINAE要义。在C++11中，标准确立了这种编译的行为，而不像C++98未明确定义它的行为。通过std::enable_if和SFINAE的共同使用，会产生很多很奇妙的实现，STL库中大量的应用了这种组合，下面我们来看看他们组合一起是如何工作的。

- 模板替换的过程：第二步用到了SFINAE，替换失败了没关系，只要有一个替换成功了就不报错，如果想主动让其替换失败，就可以用enable_if了
- 
![](images/C++中的模板元编程_image_1.png)

- type_traits是C++11新引入的模板库（类型特征），里边核心的模板类有enable_if和is_same，这个库中每一个模板类都有一个value值用来表示含义。该库的实现一般都是通过通用模板+模板特例来实现的。

- 在一定程度上，type_traits可以用C++20的concept和require来替代，写法更加简洁。（https://www.zhihu.com/question/542280815）

- 零成本抽象：一般指运行时0成本，意味着添加更高层次的编程概念，比如**泛型、集合**等等，**不会带来运行时成本，只会带来编译器时间成本(代码编译起来会更慢)**。在**零成本抽象上的任何操作**都和使用较低级别的编程概念(对于循环、计数器、 ifs 等等)**手工编写匹配功能一样快**。
