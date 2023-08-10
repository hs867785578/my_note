参数服务器
# 1. 通过launch来加载yaml定义的参数

![](images/ROS的参数服务器_image_1.png)

# 2. 通过nodehandle来读取参数


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520112926567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0tldmluX1hpZTg2,size_16,color_FFFFFF,t_70)  
param()函数从参数服务器取参数值给变量param_val。如果无法获取，则将默认值赋给变量。
这个函数的功能和getParam()函数类似，区别是param()函数还提供了一个默认值。

![](images/ROS的参数服务器_image_2.png)