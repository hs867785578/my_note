
C++的第三方的反射库非常多，前阵子还用过pfr，不需要用宏去注册，可以直接遍历数据成员的：

```cpp
#include <iostream>
#include "boost/pfr.hpp"

struct my_struct { // no ostream operator defined!
    int i;
    char c;
    double d;
};

int main() {
    my_struct s{100, 'H', 3.141593};
    std::cout << "my_struct has " << boost::pfr::tuple_size<my_struct>::value
        << " fields: " << boost::pfr::io(s) << "\n";
}

// 输出：
my_struct has 3 fields: {100, H, 3.14159}
```