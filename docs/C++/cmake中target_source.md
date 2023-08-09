cmake中的target_source（project_name a.cpp b.cpp），相当于add_library（project_name a.cpp b.cpp）中添加源文件a.cpp b.cpp的功能，好处是可以控制PRIVATE和PUBLIC

一个典型的子模块的构建过程
![](../_resources/5b20f1598038e096fe2b1d081ba7c6eb.png)