C++中类型声明应该遵循以下原则：**先找到变量名，先往右找，再往左找，遇到括号依次循环，遇到const可以先跳过**。
![](images/C++中嵌套类型声明的读法_image_1.png)


举例：

v是一个数组，数组里存放的是指针，每个指针指向int
![](images/C++中嵌套类型声明的读法_image_1.png)

v是一个指针，指向一个数组，数组里存的是int（数组指针）
![](images/C++中嵌套类型声明的读法_image_2.png)

v是一个数组，数组里存放的是指针，每个指针指向int返回型的函数
![](images/C++中嵌套类型声明的读法_image_3.png)

v是一个指针，指向一个数组，数组里存的是指针，指向int返回型的函数
![](images/C++中嵌套类型声明的读法_image_4.png)
