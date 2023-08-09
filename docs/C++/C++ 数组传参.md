# 传统传参（C语言兼容）

```cpp
void f1(int arr[]) {}
void f2(int arr[8]) {}
void f3(int(arr)[]) {}
void f4(int(arr)[9]) {}
void f5(int* arr) {}
```

以上的定义是完全等价的, 特别是`void f2(int arr[8]) {}`这种写法，非常具有迷惑性，也是初学者容易犯错的地方。它会让人觉得，参数是个长度为8的int数组，然后在函数内部，完全把参数`arr`当做数组来访问。比如：

```cpp
void printArray(int arr[8]) {
    int length = sizeof(arr)/sizeof(arr[0]);
    for (int i = 0 ; i < length; ++i){
        std::cout << arr[i] << std::endl;
    }
}
```

而实际上，`sizeof(arr)`的值是`sizeof(int*)`，这样写出来的代码肯定有问题。

```c++
void func(int arr[8]) {
    // arr 实际上是 int* 类型的指针

    // 访问数组元素
    int value = arr[0];

    // 修改数组元素
    arr[0] = 10;
}

void func(int arr[]) {
    // arr 实际上是 int* 类型的指针

    // 访问数组元素
    int value = arr[0];

    // 修改数组元素
    arr[0] = 10;
}

```

这种定义方法，在函数内部无法得到数组的真实长度，一般推荐的方法是增加一个参数表示长度，比如：

```cpp
void printArray(int arr[], int length) {
    for (int i = 0 ; i < length; ++i){
        std::cout << arr[i] << std::endl;
    }
}
```

**记住`f1, f2, f3, f4`的定义完全等价于`f5`，使用上基本就不会犯错了。**

准确地说，这种定义方法并不是把数组作为参数，只是定义了个指针类型的参数，写的形式像数组罢了。把数组传给如此定义的函数，数组就退化成一个指针，同时丢失了长度信息。

# C++推荐的数组传参

如果想传参时就确定好数组的大小，需要用到数组指针或数组引用。
比如：
```cpp
//实参必须是含有10个整数的数组。
void print(int (&arr)[10]) {
	for (auto i:arr) {
		cout << i << endl;
	}
}
```

用**数组指针或数组引**用作为函数参数，有如下优点：

1. 参数仍然保留着数组的信息，包括长度，所以我们可以使用基于范围的for循环。
2. 防止参数误传。如果参数是长度为8的数组指针或引用，传递长度为10的数组作为参数，编译是无法通过的。
3. 方便。

## 数组指针作为参数

```cpp
//arr可有可无
void printArray(int(*arr)[8]) {
    for (auto i : *arr){
        std::cout << i << std::endl;
    }
}
// 二维数组
void printArray(int(*arr)[2][3]) {
    for(auto& i : *arr) { // must be reference here
        for(auto j : i) {
            std::cout << j << std::endl;
        }
    }
}
int main(int argc, const char *argv[])
{
    int a[8] = {1,2,3,4,5,6,7,8};
    int b[2][3] = {{11,22,33}, {44,55,66}};
    printArray(&a);
    printArray(&b);
    return 0;
}
```

## 数组引用作为参数

```cpp
//arr可有可无
void printArray(int(&arr)[8]) {
    for (auto i : arr){
        std::cout << i << std::endl;
    }
}
// 二维数组
void printArray(int(&arr)[2][3]) {
    for(auto& i : arr) { // must be reference here
        for(auto j : i) {
            std::cout << j << std::endl;
        }
    }
}
int main(int argc, const char *argv[])
{
    int a[8] = {1,2,3,4,5,6,7,8};
    int b[2][3] = {{11,22,33}, {44,55,66}};
    printArray(a);
    printArray(b);
    return 0;
}
```

## 搭配模板

```cpp
// 编译期获得数组长度，不用特意指定N的值，编译器自动推导
// see <<Effective Modern C++>>
template<typename T, std::size_t N>
constexpr std::size_t arraySize(T(&)[N]) noexcept {
    return N;
}
// 打印任意长度一维数组，type T is printable
template<typename T, std::size_t N>
void printArray(T(&arr)[N]) noexcept {
    for (auto& i : arr){
        std::cout << i << std::endl;
    }
}
```