**target_include_directories()**：指定**目标**包含的头文件路径。
**target_link_libraries()**：指定**目标**链接的库。
**target_compile_options()**：指定**目标**的编译选项。
**目标** 由 *add_library()* 或 *add_executable()* 生成。
这三个指令类似，这里以 **target_include_directories()** 为例进行讲解。

对于一下依赖关系：a依赖b，b依赖c
a<-b<-c
如果b.cpp中调用了c.h，而b.h中不需要调用c.h。那么b在链接c时是**private**的。此时a调用b.h时，是没有办法用c中的函数的。
如果b.h中调用了c.h，而b.cpp中不需要调用c.h。那么b在链接c时是**interface**的。此时a调用b.h时，是可以用c中的函数的。
如果b.h中和b.cpp都调用了c.h。那么b在链接c时是**public**的。此时a调用b.h时，是没有办法用c中的函数的。

对于a链接b，一定是private的，因此a是运行文件，不需要.h文件