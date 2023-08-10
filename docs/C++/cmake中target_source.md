cmake中的target_source（project_name a.cpp b.cpp），相当于add_library（project_name a.cpp b.cpp）中添加源文件a.cpp b.cpp的功能，好处是可以控制PRIVATE和PUBLIC

一个典型的子模块的构建过程
![](images/cmake中target_source_image_1.png)
