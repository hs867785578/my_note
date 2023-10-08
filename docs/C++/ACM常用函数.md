
## 字符串转换
### C风格

```cpp

字符串转整型
int       atoi( const char *str );
long      atol( const char *str );
long long atoll( const char *str );
 
字符串转浮点型
double atof( const char* str );//历史原因，转为double
```

### C++风格

```cpp
int       stoi( const std::string& str, std::size_t* pos = 0, int base = 10 );
long      stol( const std::string& str, std::size_t* pos = 0, int base = 10 );
long long stoll( const std::string& str, std::size_t* pos = 0, int base = 10 );

字符串转浮点型
float     stof( const std::string& str, std::size_t* pos = 0 );
double    stod( const std::string& str, std::size_t* pos = 0 );
```

### 万能转string

```cpp
Defined in header <string>
std::string to_string( int value );
std::string to_string( long value );
std::string to_string( long long value );
std::string to_string( unsigned value );
std::string to_string( unsigned long value );
std::string to_string( unsigned long long value );
std::string to_string( float value );
std::string to_string( double value );
std::string to_string( long double value );
```

## 内存相关

### memset
```cpp
memset(arr, 0x3f, sizeof arr);//每个元素赋值成0x3f3f3f3f，即1061109567，通常看成int的无穷大
memset(arr, 0x7f, sizeof arr);//每个元素赋值成0x7f7f7f7f，即2139062143，通常也看成int的无穷大，但没有上一条常用，原因在于2139062143过大，再做加法时容易爆int


memset(arr, 0, sizeof arr);//每个元素都为0
memset(arr, 0x3f, sizeof arr); //每个元素都为0.0047,由于没有用处，固没用
memset(arr, 0x7f, sizeof arr); //浮点数的正无穷大，这个有用


memset(arr, val, sizeof(type) * num);//type为数组的类型，num为初始化的个数

```

### memcpy

```cpp
int a[100], b[100];
// 相当于b = a
memcpy(b, a, sizeof a); 
```

### strlen

```cpp
char str[100];
scanf("%s", str);
int len = strlen(str); // 传入需要知道长度的字符串的头指针
```

### strcmp
```cpp
char a[10] = "abc", b[10] = "abd";
int k = strcmp(a, b); // 传入需要比较的两个字符串的头指针
// 若 a > b，则 k > 0
// 若a == b，则 k = 0
// 若 a < b，则 k < 0
```

### strcpy

```cpp
char a[10] = "aaaaaa";
char b[10] = "bbbbbb";
strcpy(b, a); // 传入两个需要操作的字符串头指针，相当于b = a

```

### strcat
```cpp
char a[10] = "aaa";
char b[10] = "bbb";
strcat(b, a); //传入需要操作的两个字符串的头指针， 相当于b += a
puts(b); //输出 bbbaaa
strcat(b, &a[2]); //相当于b += &a[2]
puts(b); //输出 bbbaaaa

```

## 判断类型

```cpp
//1.isalpha

isalpha()用来判断一个字符是否为字母，如果是字符则返回非零，否则返回零。

    cout << isalpha('a');//返回非零

    cout << isalpha('2');//返回0

//2.isalnum

isalnum()用来判断一个字符是否为数字或者字母，也就是说判断一个字符是否属于a~z||A~Z||0~9。

        cout << isalnum('a');//输出非零

    cout << isalnum('2');//非零

    cout << isalnum('.');//零

//3. isdigit 判断是否是数字

//3.islower/tolower

islower()用来判断一个字符是否为小写字母，也就是是否属于a~z。
tolower()用来讲大写字母转为小写
    cout << islower('a');//非零

    cout << islower('2');//输出0

    cout << islower('A');//输出0

//4.isupper/toupper

isupper()和islower相反，用来判断一个字符是否为大写字母。
toupper()用来将小写字母转为大写
        cout << isupper('a');//返回0

    cout << isupper('2');//返回0

    cout << isupper('A');//返回非零

// 5. isspace

// 6. 

//C++ 模板
#include <iostream>
#include <type_traits>  // 需要包含这个头文件来使用 std::is_same

int main() {
    int a = 10;

    if (std::is_same<decltype(a), int>::value) {
        std::cout << "a is of type int" << std::endl;
    } else {
        std::cout << "a is NOT of type int" << std::endl;
    }

    return 0;
}

```

## 其他

### string.find

s.find(a)快速查找字符串s中是否有子串a。如果有返回s的起始位置，如果没有返回 string :: npos;

示例：力扣796

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        return s.size() == goal.size() && (s + s).find(goal) != string :: npos;
    }
};
//只需要检查 goal 是否为s+s 的子字符串即可
```


## 容器

```cpp
vector, 变长数组，倍增的思想
    size()  返回元素个数
    empty()  返回是否为空
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

pair<int, int>
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

string，字符串
    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址

queue, 队列
    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)

    set/multiset
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    map/multimap
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位
    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反
```