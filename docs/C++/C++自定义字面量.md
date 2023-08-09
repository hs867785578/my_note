
## 1. c++中的字面量
在C++11之前，字面量是不可以自定义的，只有系统规定的一些字面量，包括：
- 整数字面量（u或l或不加）如1 2  3u  454545l
- 浮点数字面量（f或不加） 如1.23
- 布尔字面量 如true
- 字符字面量：单引号，通常字符字面量，例如'a'  '\\n' 、UTF-8 字符字面量，例如 u8'a'，UTF-16 字符字面量，例如 u'猫'
- 字符串字面量：双引号，如"hello"，其是const char\[6\]，并保有字符 'H'、'e'、'l'、'l'、'o' 及 '\0'。
- 空指针字面量：nullptr（C++11）

## 2.用户自定义字面量

在c++11后，用户可以自定义字面量：
- 用户定义整数字面量，例如 12_km

- 用户定义浮点字面量，例如 0.5_Pa

- 用户定义字符字面量，例如 'c'_X

- 用户定义字符串字面量，例如 "abd"_L 或 u"xyz"_M

```cpp
long double operator ""_w(long double);
[std::string](http://zh.cppreference.com/w/cpp/string/basic_string) operator ""_w(const char16_t*, size_t);
unsigned    operator ""_w(const char*);
 
int main()
{
    1.2_w;    // 调用 operator ""_w(1.2L)
    u"one"_w; // 调用 operator ""_w(u"one", 3)
    12_w;     // 调用 operator ""_w("12")
    "two"_w;  // 错误：没有适用的字面量运算符
}
```