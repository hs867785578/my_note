# C++ Primer：static const与static constexpr的类内数据成员初始化（练习7.58解答）

 ![original.png](../_resources/original.png)

 [北冥有鱼wyh](https://blog.csdn.net/qq_34801642)  ![newCurrentTime2.png](../_resources/newCurrentTime2.png)  于 2020-03-18 17:11:48 发布  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  10423  [![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏   51]()

 分类专栏：  [# C++ Primer](https://blog.csdn.net/qq_34801642/category_9641460.html)  文章标签：  [c++](https://so.csdn.net/so/search/s.do?q=c%2B%2B&t=blog&o=vip&s=&l=&f=&viparticle=)

 [版权<div style="display: none;"></div>

]()

 [![resize,m_fixed,h_224,w_224](../_resources/resize,m_fixed,h_224,w_224)      C++ Primer  专栏收录该内容](https://blog.csdn.net/qq_34801642/category_9641460.html)

 45 篇文章  6 订阅

 [订阅专栏]()

 <div style="display: none;"></div>

* * *

# 1. 背景

  在C++Primer 中文版（第5版）第270页中，对静态数据成员的类内初始化作如下说明：

>
> 我们可以为静态成员提供const整数类型的类内初始值，不过要求静态成员必须是字面值常量类型的constexpr。
>

  上面的句子不好理解。故找到如下对应的英文版说明：

>

> we can provide in-class initializers for static members that have const integral type and must do so for static members that are constexprs of literal type.

>

  如上英文的大致意思：

>
> 我们可以为const整型的静态数据成员提供类内初始值。constexprs的静态成员必须提供类内初始值。
>

  个人理解：

>

1. > 算术类型包括整型和浮点型。整型包括bool、char、int、short、long等。浮点型包括float和double等（详见C++Primer第30页）。

2. > const**> 整型**> 类型的静态数据成员（即static const的**> 整型**> 数据成员）可以使用类内初始值来初始化。
3. > constexprs的静态成员（即static constexpr的数据成员）**> 必须**> 使用类内初始值来初始化。
>

# 2. 实例

## 2.1 static数据成员

```
#include <iostream>
using namespace std;
class A
{
public:
    //整型的静态成员
    static bool b;
    static char c;
    static int i;
    //浮点型的数据成员
    static float f;
    static double d;

    // static int i1 = 1;       // 错误：带有类内初始值设定项的成员必须为常量
    // static double d1 = 3.14; // 错误：带有类内初始值设定项的成员必须为常量
};

bool A::b = true;
char A::c = 'a';
int A::i = 1;
float A::f = 1.5;
double A::d = 2.5;

int main()
{
    cout << A::b << endl;
    cout << A::c << endl;
    cout << A::i << endl;
    cout << A::f << endl;
    cout << A::d << endl;
    return 0;
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29
- 30
- 31
- 32

![format,png#pic_center](../_resources/format,png#pic_center-1)

总结：

1. 一般的static数据成员只能在类内声明，在类外定义和初始化。

## 2.2 static const数据成员

```
#include <iostream>
using namespace std;
class A
{
public:
    //整型的静态成员
    static const bool b1;
    static const char c1;
    static const int i1;
    //浮点型的数据成员
    static const float f1;
    static const double d1;

    //整型的静态成员
    static const bool b2 = false;
    static const char c2 = 'b';
    static const int i2 = 2;
    //浮点型的数据成员
    // static const float f2 = 3.5;  // 错误："const float" 类型的成员不能包含类内初始值设定项
    // static const double d2 = 4.5; // 错误："const double" 类型的成员不能包含类内初始值设定项

    // char m1[i1];// 错误：i1的常量还未初始化
    char m2[i2];
};

const bool A::b1 = true;
const char A::c1 = 'a';
const int A::i1 = 1;
const float A::f1 = 1.5;
const double A::d1 = 2.5;

const bool A::b2;
const char A::c2;
const int A::i2;

int main()
{
    cout << A::b1 << endl;
    cout << A::c1 << endl;
    cout << A::i1 << endl;
    cout << A::f1 << endl;
    cout << A::d1 << endl;
    cout << "---------------------------------------" << endl;
    cout << A::b2 << endl;
    cout << A::c2 << endl;
    cout << A::i2 << endl;
    return 0;
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29
- 30
- 31
- 32
- 33
- 34
- 35
- 36
- 37
- 38
- 39
- 40
- 41
- 42
- 43
- 44
- 45
- 46
- 47
- 48

![format,png#pic_center](../_resources/format,png#pic_center)

总结：

1. static const数据成员可以在类内声明，在类外定义和初始化。在类内声明时，static const数据成员还未初始化，不算真正的const。

2. static const**整型**数据成员可以在类内声明和初始化，在类外定义。在类内声明和初始化时，static const数据成员是真正的const。

3. 若编译时static const数据成员可用它的**值**替代（如表示数组个数等），它可以不需要定义。若不能替代（如作为参数等），必须含有一条定义语句。建议不管是否可替代都在类外定义一次。

4. 若static const数据成员不是**整型（bool、char、int、short、long等）**，则不能在类内初始化。

## 2.3 static constexpr数据成员

```
#include <iostream>
using namespace std;
class A
{
public:
    //整型的静态成员
    // static constexpr bool b1; // 错误：constexpr 静态数据成员声明需要类内初始值设定项
    // static constexpr char c1; // 错误：constexpr 静态数据成员声明需要类内初始值设定项
    // static constexpr int i1;  // 错误：constexpr 静态数据成员声明需要类内初始值设定项
    //浮点型的数据成员
    // static constexpr float f1;  // 错误：constexpr 静态数据成员声明需要类内初始值设定项
    // static constexpr double d1; // 错误：constexpr 静态数据成员声明需要类内初始值设定项

    //整型的静态成员
    static constexpr bool b2 = false;
    static constexpr char c2 = 'b';
    static constexpr int i2 = 2;
    //浮点型的数据成员
    static constexpr float f2 = 3.5;
    static constexpr double d2 = 4.5;

    // char m1[i1]; // 错误：i1的常量还未初始化
    char m2[i2];
};

// constexpr bool A::b1 = true;
// constexpr char A::c1 = 'a';
// constexpr int A::i1 = 1;
// constexpr float A::f1 = 1.5;
// constexpr double A::d1 = 2.5;
constexpr bool A::b2;
constexpr char A::c2;
constexpr int A::i2;
constexpr float A::f2;
constexpr double A::d2;

int main()
{
    cout << A::b2 << endl;
    cout << A::c2 << endl;
    cout << A::i2 << endl;
    cout << A::f2 << endl;
    cout << A::d2 << endl;
    return 0;
}
```

![newCodeMoreWhite.png](../_resources/newCodeMoreWhite.png)

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17
- 18
- 19
- 20
- 21
- 22
- 23
- 24
- 25
- 26
- 27
- 28
- 29
- 30
- 31
- 32
- 33
- 34
- 35
- 36
- 37
- 38
- 39
- 40
- 41
- 42
- 43
- 44
- 45

![format,png#pic_center](../_resources/format,png#pic_center-2)

总结：

1. static constexpr数据成员**必须**在类内声明和初始化。在类内声明和初始化时，static constexpr数据成员是真正的const。

2. 若编译时static constexpr数据成员可用它的**值**替代（如表示数组个数等），它可以不需要定义。若不能替代（如作为参数等），必须含有一条定义语句。建议不管是否可替代都在类外定义一次。

3. 若static constexpr数据成员即使不是整型，也可进行类内初始化时。

# 3. 习题解析

练习7.58：下面的静态数据成员的声明和定义有错误吗？请解释原因。

```
#include <iostream>
#include <vector>
using namespace std;

class Example
{
public:
    static double rate = 6.5;
    static const int vecSize = 20;
    static vector<double> vec(vecSize);
};

double Example::rate;
vector<double> Example::vec;
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14

解答：

1. 一般的static数据成员只能类内声明，类外定义和初始化。rate不是static const或static constexpr，只是一般static数据成员，不能在类内初始化。

```
static double rate = 6.5;
//改为
static double rate;
```

- 1
- 2
- 3

1. vecSize的类型是static const int，可类内初始化。vecSize仅用于表示vector容器的个数，编译时可直接用20替代，故在类外定义或者不定义都是可行的，但最好在类外定义一次以防万一。

```
static const int vecSize = 20; // 正确
// 最好在类外添加定义
static const int vecSize;
```

- 1
- 2
- 3

1. vecSize已在类内初始化，可用于表示vector的容量。vec只是一般static数据成员，不能在类内初始化。

```
static vector<double> vec(vecSize);
// 改为
// 类内
static vector<double> vec;
// 类外
vector<double> Example::vec(vecSize);
```

- 1
- 2
- 3
- 4
- 5
- 6

修改后：

```
#include <iostream>
#include <vector>
using namespace std;

class Example
{
public:
    static double rate;
    static const int vecSize = 20;
    static vector<double> vec;
};

double Example::rate;
static const int vecSize;
vector<double> Example::vec(vecSize);
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15

[![3_qq_34801642](../_resources/3_qq_34801642)北冥有鱼wyh](https://blog.csdn.net/qq_34801642)

 [关注](#)

- [![newHeart2021Black.png](../_resources/newHeart2021Black.png)   21]()

- [![newUnHeart2021Black.png](../_resources/newUnHeart2021Black.png)]()

- [![newCollectActive.png](../_resources/newCollectActive.png)   51](#)

- [![newRewardBlack.png](../_resources/newRewardBlack.png)](#)

- [![newComment2021Black.png](../_resources/newComment2021Black.png)  6]()

-

- [![newShareBlack.png](../_resources/newShareBlack.png)](#)

 [专栏目录]()

[*C++*  *const* volatile *constexpr*  *static*](https://blog.csdn.net/weixin_44039270/article/details/106837250)

[weixin_44039270的博客](https://blog.csdn.net/weixin_44039270)
![readCountWhite.png](../_resources/readCountWhite.png)159

[*C++*  *const* volatile与*constexpr*前言volatile多线程下的volatile 前言 这是面试官比较喜欢问的问题，咱们把它解决掉，开始。 volatile 遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问。 当要求使用 volatile 声明的变量的值的时候，系统总是重新从它所在的内存读取*数据*，即使它前面的指令刚刚从该处读取过*数据*。 volatile 指出 该变量是随时可能发生变化的，每次使用它的时候必须从 该变量的地址中读取。 多线](https://blog.csdn.net/weixin_44039270/article/details/106837250)

[*C++*在*类*内声明并定义静态*成员*常量数组（*static*  *constexpr*  *const* int member[]）](https://blog.csdn.net/hikali8/article/details/120245241)

[hikali8的博客](https://blog.csdn.net/hikali8)
![readCountWhite.png](../_resources/readCountWhite.png)3769

[对于*C++*23标准，如果我们想要某个*类*的*成员*变量能被此*类*的所有对象访问，且该变量不可修改，那么我们一般会选择将该变量声明为静态*成员*常量。例如此代码：

class Decrypter{ *static*  *const* int mask = 0x8a; public: void decrypt(){ ... } };

其中，我们把Decript::mask声明为了一个静态*成员*常量。对于Decripter*类*的所有对象而言，都能访问到同一个mask，节约了内存空间。](https://blog.csdn.net/hikali8/article/details/120245241)

评论6条![commentArrowRightWhite.png](../_resources/commentArrowRightWhite.png)[写评论]()

[![3_wq1129](../_resources/3_wq1129)wq1129](https://blog.csdn.net/wq1129)热评

static constexpr类内如何声明数组呢？如
1. 1
#include  <iostream>
2. 2
using  namespace std;
3. 3
class  A
4. 4
{
5. 5
public:
6. 6
 static  constexpr  int Pn[2] = {1，1}；*//这样好像不对，是应该怎样啊？*
7. 7
};

 [ 关键字*constexpr*(*C++*)_大道之道的博客_*c++*  *constexpr*](https://blog.csdn.net/huixizhu/article/details/114292698)

 12-24

 [ 关键字*constexpr*是在*C++*11中引入的,并且在*C++*14中得到了改进。像*const*一样,它可以应用于变量:当任何代码试图去修改该值时,都会引发编译器错误;与*const*不同,*constexpr*也可以应用于函数和*类*构造函数。*constexpr*表示该值或返回值是恒定的...](https://blog.csdn.net/huixizhu/article/details/114292698)

 [ *constexpr*_Gamer_code的博客_*constexpr*](https://blog.csdn.net/m0_52902391/article/details/120308866)

 12-25

 [ *const*inti=520;// 是一个常量表达式 *const*intj=i+1;// 是一个常量表达式 *constexpr*inti=520;// 是一个常量表达式 *constexpr*intj=i+1;// 是一个常量表达式 对于*C++* 内置*类*型的*数据*,可以直接用 *constexpr* 修饰,但如果是自定义的...](https://blog.csdn.net/m0_52902391/article/details/120308866)

[*C++*17string_view的使用介绍与实现  最新发布](https://blog.csdn.net/a4364634611/article/details/127506277)

[a4364634611的博客](https://blog.csdn.net/a4364634611)
![readCountWhite.png](../_resources/readCountWhite.png)101

[值得一提的是标准并没有规定basic_string_view有对应的构造函数，按照MSVC的实现来说，是basic_string内部实现了一个转换函数可转换到basic_string_view。(*C++*20 中移除)(*C++*20 中移除)(*C++*20 中移除)(*C++*20 中移除)(*C++*20 中移除)(*C++*20)描述一个能指代常量连续仿 char 对象序列的对象，序列首元素在零位置。即使在 *C++*23 中引入的正式要求前，所有既存实现中。的迭代器*类*型上的所有要求亦应用于。是到常字符序列中的视图。](https://blog.csdn.net/a4364634611/article/details/127506277)

[

[*c++*11]静态*成员*在*类*中的*初始化*总结(*static*，*static*  *const*，*static*  *constexpr*)](https://blog.csdn.net/qq_50868258/article/details/123139071)

[qq_50868258的博客](https://blog.csdn.net/qq_50868258)
![readCountWhite.png](../_resources/readCountWhite.png)1138
[

[*c++*11]静态*成员*在*类*中的*初始化*总结(*static*，*static*  *const*，*static*  *constexpr*) 阅读《*c++*  *primer* (5th)》的7.6章(270页)关于静态*成员*的*类*内*初始化*，看的我云里雾里，感觉是中文写的有问题，翻了英文版后，个人认为是原版作者在这块没有阐述清楚。 一般的，*类*的静态*成员*必须放在*类*外定义（也就是说*类*的静态*成员*不应该放在*类*内*初始化*） 对此有两个特例：

允许*static*  *const* int *类*型有*类*内初始值（允许但不强求）*static*  *constexpr**类*](https://blog.csdn.net/qq_50868258/article/details/123139071)

[`计算机知识` `*C++*` *constexpr*常量](https://blog.csdn.net/weixin_42712593/article/details/121245001)

[WChang的博客](https://blog.csdn.net/weixin_42712593)
![readCountWhite.png](../_resources/readCountWhite.png)151
[1](https://blog.csdn.net/weixin_42712593/article/details/121245001)

[*C++*之*static*、*const*、*constexpr*、volatile关键字](https://blog.csdn.net/weixin_43518637/article/details/110669381)

[有梦想的小新人的博客](https://blog.csdn.net/weixin_43518637)
![readCountWhite.png](../_resources/readCountWhite.png)929

[（1）*static*全局变量 在全局变量前加上关键字*static*，全局变量就定义成一个全局静态变量。内存中的位置：全局/静态存储区，在整个程序运行期间一直存在。*初始化*：未经*初始化*的全局静态变量会被自动*初始化*为0（自动对象的值是任意的，除非他被显式*初始化*）； 作用域：全局静态变量在声明他的文件之外是不可见的，准确地说是从定义之处开始，到文件结尾。 与全局变量区别：*static*全局变量在定义该变量的当前源文件内有效，在同一源程序的其它源文件中不能使用它。而普通的全局变量在各个源文件中都是有效的（当一个源程序由多](https://blog.csdn.net/weixin_43518637/article/details/110669381)

[*static*、*static*  *const*、*static*  *constexpr* 的区别](https://blog.csdn.net/a499957739/article/details/126381391)

[a499957739的博客](https://blog.csdn.net/a499957739)
![readCountWhite.png](../_resources/readCountWhite.png)461

[*static*、*static*  *const*、*static*  *constexpr* 的区别](https://blog.csdn.net/a499957739/article/details/126381391)

[关键字 之*static*，*const*，*constexpr*，volatile](https://blog.csdn.net/weixin_46535567/article/details/124628036)

[四库全书的酷](https://blog.csdn.net/weixin_46535567)
![readCountWhite.png](../_resources/readCountWhite.png)586

[文章目录1.*static*关键字1.1 *static*全局变量1.2 *static*局部变量1.3 *static*函数1.4 *类*的*static**成员*1.5 *类*的*static**数据**成员*1.6 *类*的*static*函数*成员*2.*const*关键字2.1 *初始化*与*const*、修饰普通*类*型的变量*初始化*与*const*：修饰普通*类*型的变量：2.2 *const*与文件有效期2.3 *const*修饰引用2.4 *const*修饰指针2.5 *const*修饰参数传递2.6 *const*修饰函数返回值2.7 *const*修饰*类*的*数据**成员*2.8 *const*修饰*类*的](https://blog.csdn.net/weixin_46535567/article/details/124628036)

[*C++*中*static*  *const*与*static*  *constexpr*的*类*内变量*成员**初始化*](https://blog.csdn.net/mxyhktk/article/details/112016564)

[mxyhktk的博客](https://blog.csdn.net/mxyhktk)
![readCountWhite.png](../_resources/readCountWhite.png)2322

[1.一般*static**类*内*成员*变量 class A { public:*static* int i1;*static* bool b1;*static* char c1;*static* float f1;*static* double d1; //以下错误，不能将一般*static**类*型的变量在*类*内*初始化*,只能进行声明 //*static* int i2 = 2; //*static* bool b2 = true; //*static* char c2 = 'c'; //*static* float f2 =](https://blog.csdn.net/mxyhktk/article/details/112016564)

[【*C++*】*static*、*const*、*constexpr*、全局变量、局部变量](https://blog.csdn.net/weixin_41646851/article/details/107506523)

[weixin_41646851的博客](https://blog.csdn.net/weixin_41646851)
![readCountWhite.png](../_resources/readCountWhite.png)1950

[*const*定义的常量在超出其作用域之后其空间会被释放，而*static*定义的静态常量在函数执行后不会释放其存储空间。 *static*  *static*表示的是静态的。*类*的静态*成员*函数、静态*成员*变量是和*类*相关的，而不是和*类*的具体对象相关的。即使没有具体对象，也能调用*类*的静态*成员*函数和*成员*变量。一般*类*的静态函数几乎就是一个全局函数，只不过它的作用域限于包含它的文件中。在*C++*中，*static*静态*成员*变量不能在...](https://blog.csdn.net/weixin_41646851/article/details/107506523)

[*static*  *const*与*static*  *constexpr*的*类*内*数据**成员**初始化*](https://blog.csdn.net/qq_21237549/article/details/125437717)

[GXQ的博客](https://blog.csdn.net/qq_21237549)
![readCountWhite.png](../_resources/readCountWhite.png)357

[参考：https://blog.csdn.net/qq_34801642/article/details/104948850实例 2.1*static**数据**成员*#include using namespace std; class A { public: //整型的静态*成员**static* bool b;*static* char c;*static* int i; //浮点型的*数据**成员**static* float f;*static* double d;// *static* int i1 = 1; //](https://blog.csdn.net/qq_21237549/article/details/125437717)

[*constexpr*变量、*const*exp函数和常量表达式](https://blog.csdn.net/weixin_44064908/article/details/124156379)

[博客](https://blog.csdn.net/weixin_44064908)
![readCountWhite.png](../_resources/readCountWhite.png)572

[*constexpr*变量、*const*exp函数和常量表达式 常量表达式： 值不会改变且在编译过程中就可得到结果*const* int max = 20;//max是常量表达式*const* int min = max-19;//min是常量表达式*constexpr*用途:便于编译器验证变量是否为常量表达式*constexpr* int max =20;//20是常量表达式*constexpr* int min = max-19;//max-19是常量表达式*constexpr* int sz =size();//仅仅](https://blog.csdn.net/weixin_44064908/article/details/124156379)

[*c++*nullptr（空指针常量）、*constexpr*（常量表达式）](https://conscience-still.blog.csdn.net/article/details/109266403)

[良知犹存的博客](https://blog.csdn.net/lyn631579741)
![readCountWhite.png](../_resources/readCountWhite.png)325
[总述

又来更新了，今天带来的是nullptr空指针常量、*constexpr*（常量表达式）*C++*的两个用法。Result result_fun =nullptr;*constexpr**static* uint32_t try_times = 100;这是两个在工作中常用的*C++*操作，但是你知道nullptr和*constexpr*由来以及它们的更多用法吗？

下面听我一一道来。
作者：良知犹存
转载授权以及围观：欢迎添加微信公众号：羽林君
一、nullptr
...](https://conscience-still.blog.csdn.net/article/details/109266403)

[*C++*之*constexpr*详解  热门推荐](https://blog.csdn.net/janeqi1987/article/details/103542802)

[janeqi1987的专栏](https://blog.csdn.net/janeqi1987)
![readCountWhite.png](../_resources/readCountWhite.png)3万+

[*constexpr*表达式是指值不会改变并且在编译过程就能得到计算结果的表达式。声明为*constexpr*的变量一定是一个*const*变量，而且必须用常量表达式*初始化*：*constexpr* int mf = 20; //20是常量表达式*constexpr* int limit = mf + 1; // mf + 1是常量表达式*constexpr* int sz = size(); //之后当siz...](https://blog.csdn.net/janeqi1987/article/details/103542802)

[c/*c++*/*c++*11/*c++*14 *static*  *const*  *constexpr*区别](https://blog.csdn.net/knowledgebao/article/details/125225576)

[knowledgebao的博客](https://blog.csdn.net/knowledgebao)
![readCountWhite.png](../_resources/readCountWhite.png)191

[c/*c++*/*c++*11/*c++*14 *static*  *const*  *constexpr*区别](https://blog.csdn.net/knowledgebao/article/details/125225576)

[*C++*  *static*  *const*  *constexpr*(未完）](https://blog.csdn.net/weixin_42442319/article/details/118567333)

[weixin_42442319的博客](https://blog.csdn.net/weixin_42442319)
![readCountWhite.png](../_resources/readCountWhite.png)315

[内存中的存储空间主要分为三个部分： （1）程序区 （2）静态存储区 （3）动态存储区 1.*static*1.1. 静态*数据**成员*1.2. 静态*成员*函数 2. *const*3. *constexpr*](https://blog.csdn.net/weixin_42442319/article/details/118567333)

[*c++*11总结20——*constexpr*](https://blog.csdn.net/www_dong/article/details/118583687)

[www_dong的博客](https://blog.csdn.net/www_dong)
![readCountWhite.png](../_resources/readCountWhite.png)350

[1. 概念*c++*11引入的关键字，用于编译时获取常量或常量函数的结果。*constexpr*修饰普通变量时，变量必须经过*初始化*且初始值必须是一个常量表达式。

2. *c++*11限制*c++*11之前的常量表达式不允许包含函数调用或对象构造，如下所示：
int rValue() { return 1; }

int m_iValue[rValue() + 2]; //*c++*03中error*c++*11引入关键字*constexpr*，允许编程者保证函数或对象的构造函数是编译时常量，代码改写如下](https://blog.csdn.net/www_dong/article/details/118583687)

### “相关推荐”对你有帮助么？

- ![npsFeel1.png](../_resources/npsFeel1.png)

非常没帮助

- ![npsFeel2.png](../_resources/npsFeel2.png)

没帮助

- ![npsFeel3.png](../_resources/npsFeel3.png)

一般

- ![npsFeel4.png](../_resources/npsFeel4.png)

有帮助

- ![npsFeel5.png](../_resources/npsFeel5.png)

非常有帮助

 ©️2022 CSDN  皮肤主题：技术工厂   设计师：CSDN官方博客    [返回首页](https://blog.csdn.net/)

- [关于我们](https://www.csdn.net/company/index.html#about)

- [招贤纳士](https://www.csdn.net/company/index.html#recruit)

- [商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)

- [寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)

- ![tel.png](../_resources/tel.png)  400-660-0108

- ![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/qq_34801642/article/details/104948850mailto:webmaster@csdn.net)

- ![cs.png](../_resources/cs.png)  [在线客服](https://csdn.s2.udesk.cn/im_client/?web_plugin_id=29181)

- 工作时间 8:30-22:00

- [公安备案号11010502030143](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010502030143)

- [京ICP备19004658号](http://beian.miit.gov.cn/publish/query/indexFirst.action)

- [京网文〔2020〕1039-165号](https://csdnimg.cn/release/live_fe/culture_license.png)

- [经营性网站备案信息](https://csdnimg.cn/cdn/content-toolbar/csdn-ICP.png)

- [北京互联网违法和不良信息举报中心](http://www.bjjubao.org/)

- [家长监护](https://download.csdn.net/tutelage/home)

- [网络110报警服务](http://www.cyberpolice.cn/)

- [中国互联网举报中心](http://www.12377.cn/)

- [Chrome商店下载](https://chrome.google.com/webstore/detail/csdn%E5%BC%80%E5%8F%91%E8%80%85%E5%8A%A9%E6%89%8B/kfkdboecolemdjodhmhmcibjocfopejo?hl=zh-CN)

- [账号管理规范](https://blog.csdn.net/blogdevteam/article/details/126135357)

- [版权与免责声明](https://www.csdn.net/company/index.html#statement)

- [版权申诉](https://blog.csdn.net/blogdevteam/article/details/90369522)

- [出版物许可证](https://img-home.csdnimg.cn/images/20220705052819.png)

- [营业执照](https://img-home.csdnimg.cn/images/20210414021142.jpg)

- ©1999-2022北京创新乐知网络技术有限公司

 ![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='2453'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='2454'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='2455'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2456'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='2457'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='2458'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2459'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='2460'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='2461'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='2462'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='2463'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='2464'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2465'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='2466'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='2467'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2468'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='2469'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='2470'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='2471'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='2472'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)