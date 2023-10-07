sstream中有三个常用的类：
1. stringstream(同时支持>> 和<<)
2. istringstream(只支持>>)
3. ostringstream(只支持<<)

```cpp
#include <string>
#include <sstream>//
#include <iostream>
using namespace std;
//ostringstream 用于执行C风格字符串的输出操作
void ostringstream_test()
{
	//ostringstream 只支持 << 操作符
	std::ostringstream oss;
	oss << "this is test" << 123456;
 	std::cout<<oss.str()<<std::endl;
	oss.str("");//清空之前的内容
	//oss.clear();//并不能清空内存
	//浮点数转换限制
	double tmp = 123.1234567890123;
	oss.precision(12);
	oss.setf(std::ios::fixed);//将浮点数的位数限定为小数点之后的位数
	oss << tmp;
 
	std::cout << oss.str() << "\r\n" << std::endl;
}
 
//istringstream 用于执行C风格字符串的输入操作
void istringstream_test()
{
	//istringstream 只支持 >> 操作符
	std::string str = "welcome to china";
	std::istringstream iss(str);
 
	//把字符串中以空格隔开的内容提取出来
	std::string out;
	while(iss >> out)
	{
		std::cout << out << std::endl;
	}
	std::cout << "\r\n" << std::endl;
}
 
//stringstream 同时支持C风格字符串的输入输出操作
void stringstream_test()
{
	//输入
	std::stringstream ss;
	ss << "hello this is kandy " << 123;
	std::cout << ss.str() << "\r\n" << std::endl;
 
	//输出
	std::string out;
	while(ss >> out)
	{
		std::cout << out.c_str() << std::endl;
	}
	std::cout << "\r\n" << std::endl;
}
 
int main()
{
	ostringstream_test();	
	istringstream_test();
	stringstream_test();
	system("pause");
	return 0;
}
```