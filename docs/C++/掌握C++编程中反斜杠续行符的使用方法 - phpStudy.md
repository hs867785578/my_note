   [phpStudy](http://www.phpstudy.net/)

- [网站首页](https://m.xp.cn/)

- [软件下载](https://m.xp.cn/a.php/211.html)

- [PHP教程](https://m.xp.cn/php/)

- [编程技术](https://m.xp.cn/b.php/61980.html#)

- [教程手册](https://m.xp.cn/b.php/61980.html#)

- [php中文网](http://www.php.cn/)

1. [首页](https://m.xp.cn/)

2. / [PHP教程](https://m.xp.cn/php/)

3. /  掌握C++编程中反斜杠续行符的使用方法

##  掌握C++编程中反斜杠续行符的使用方法

* * *

1) 用在宏定义中：
#define CV_ARE_SIZES_EQ(mat1, mat2) \
((mat1)->rows == (mat2)->rows && (mat1)->cols == (mat2)->cols)
2) 用在printf中，有时候printf中语句太长，需要切分，则需用到反斜杠；
3) 用“//”只能注释当前行的语句，想要将下一行一起注释掉，则可以在该行最后加上反斜杠。
另外，反斜杠除了强制换行的作用之外，还有转义符的意思。如：“\n”表示换行符，"\t" "\b"等，此时反斜杠表示转义，执行反斜杠后面的符号表示的意思。

但若要取反斜杠的本意，则需要在反斜杠之前再加一个反斜杠才能正确表示。比如我要在程序中读取F:\OpenCV2.0\vs2008\videos\videos1.avi ,，我不能直接将这样表示，而应该在每一个反斜杠前面再加一个反斜杠，表示为：F:\\OpenCV2.0\\vs2008\\videos\\videos1.avi ，这样才能正确读取你要的文件。

总结一下，目前个人了解的反斜杠的作用是两种：
1 是作为转义字符，将进行的操作是紧跟其后的字符的操作。
2 与回车键组合进行强制换行。在要强制换行的地方输入反斜杠然后回车，系统编译的时候会自动将反斜杠下面的一行与前面的一行解释成一个语句。
**续行符
**在普通代码行后面加不加都一样(VC是自动判断续行的),但是在宏定义里面就特别有用,因为宏定义规定必须用一行完成:
#define SomeFun(x, a, b) if(x)x=a+b;else x=a-b;
这一行定义是没有问题的,但是这样代码很不容易被理解,以后维护起来麻烦,如果写成:
#define SomeFun(x, a, b)
if (x)
x = a + b;
else
x = a - b;

 这样理解是好理解了,但是编译器会出错,因为它会认为#define SomeFun(x, a, b)是完整的一行,if (x)以及后面的语句与#define SomeFun(x, a, b)没有关系.

这时候我们就必须使用这样的写法:
#define SomeFun(x, a, b)\
if (x)\
x = a + b;\
else\
x = a - b;
注意:最后一行不要加续行符啊.VC的预处理器在编译之前会自动将\与换行回车去掉,这样一来既不影响阅读,又不影响逻辑,皆大欢喜

* * *

«
»
* * *

 [PHP教程](https://m.xp.cn/php/)

[PHP简介](https://m.xp.cn/php/1.html)[PHP基本语法](https://m.xp.cn/php/8.html)[PHP类型](https://m.xp.cn/php/12.html)[PHP变量](https://m.xp.cn/php/24.html)[PHP运算符](https://m.xp.cn/php/34.html)[PHP控制结构](https://m.xp.cn/php/47.html)[PHP函数](https://m.xp.cn/php/67.html)[PHP类与对象](https://m.xp.cn/php/74.html)[PHP异常处理](https://m.xp.cn/php/110.html)[函数库分类](https://m.xp.cn/php/function.php)

快速导航

   [PHP](https://m.xp.cn/list.php?id=1)[MySQL](https://m.xp.cn/list.php?id=2)[HTML](https://m.xp.cn/list.php?id=3)[CSS](https://m.xp.cn/list.php?id=4)[JavaScript](https://m.xp.cn/list.php?id=5)[MSSQL](https://m.xp.cn/list.php?id=6)[AJAX](https://m.xp.cn/list.php?id=10)[.NET](https://m.xp.cn/list.php?id=8)[JSP](https://m.xp.cn/list.php?id=9)[Linux](https://m.xp.cn/list.php?id=14)[Mac](https://m.xp.cn/list.php?id=17)[ASP](https://m.xp.cn/list.php?id=7)[服务器](https://m.xp.cn/list.php?id=11)[SQL](https://m.xp.cn/list.php?id=15)[jQuery](https://m.xp.cn/list.php?id=16)[C#](https://m.xp.cn/list.php?id=18)[C++](https://m.xp.cn/list.php?id=19)[java](https://m.xp.cn/list.php?id=20)[Android](https://m.xp.cn/list.php?id=21)[IOS](https://m.xp.cn/list.php?id=22)[oracle](https://m.xp.cn/list.php?id=23)[MongoDB](https://m.xp.cn/list.php?id=24)[SQLite](https://m.xp.cn/list.php?id=26)[wamp](http://wamp.phpstudy.net/)[交通频道](https://m.xp.cn/jiaotong/)

* * *

  Copyright © 2016 phpStudy | 豫ICP备2021030365号-3