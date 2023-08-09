using的四种用法：

# 1. 声明命名空间

using namespace std;
或者
using std::cout;


# 2. 类型别名

using a = int

# 3. 改变子类中的父类（成员变量、成员函数、类型声明）的访问权限

```cpp
class T5Base {
public:
    T5Base() :value(55) {}
    using Type = std::vector<int>;
    virtual ~T5Base() {}
    void test1() { cout << "T5Base test1..." << endl; }
protected:
    int value;
};
 
class T5Derived : private T5Base {
public:
    //using T5Base::test1;
    //using T5Base::value;
    //using T5Base::Type;
    void test2() { cout << "value is " << value << endl; }
};

T5Base::Type a{1,2,3};//ok
T5Derived::Type b{1,2,3}//error,private继承使Type变为了private。通过::只能调用public的类型声明。但是可以通过在T5Derived的public中using T5Base::Type改变权限，使其可用。
```

基类中成员变量 value 是protected，在 private 继承之后，对于外界这个值为 private，也就是说T5Derived 的对象无法使用这个 value。

如果想要通过对象使用，需要在public下通过 using T5Base::value 来引用，这样 T5Derived 的对象就可以直接使用。

同样的，对于基类中的成员函数 test1()，在private继承后变为 private，T5Derived 的对象同样无法访问，通过 using T5Base::test1 就可以使用了。

# 4.在模板中使用类型别名（代替typedef）

```cpp
//方法一 不使用using，需要先定义一个类，来使用模板别名
template <typename T>
struct map_s
{
    typedef std::map <std::string, T> map;
}

map_s<int>::map map1;
map1.insert({"key", 1});
 
//方法二 使用using，不需要额外定义类，来声明模版别名
template <typename T>
using map_s = std::map<std::string, T>;
map_s<int> map2;
map2.insert({"key", 2});
 
 
//using 包含了typedef的所有功能
typedef unsigned int unit_t; 
using unit_t = unsigned int;
 
typedef std::map<std::string, int> map;
using map = std::map<std::string, int>;
 
typedef int(*Functype)(int, int); //typedef 定义函数指针
using Functype = int(*)(int, int);
```
# 建议：
- 所有的继承都是用public继承，这样不会改变权限。
- 类中的using 声明类型都放到public中，是外界能够访问。
- 

