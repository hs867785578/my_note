1.定义只有一个元素的元组： a=（1，）

2.类中__call__函数，把类当作函数来调用：  Demo（“名字”）（） --》打印出名字![](images/python技巧_image_1.png)


3.遍历字典的方式for i in dict ->只能遍历出键值
![](images/python技巧_image_2.png)

遍历键的方法：![](images/python技巧_image_3.png)

遍历值的方法：![](images/python技巧_image_4.png)

既遍历键又遍历值：![](images/python技巧_image_5.png)

内嵌for in 循环：![](images/python技巧_image_6.png)

用for in 转换字典的键和值![](images/python技巧_image_7.png)

join方法![](images/python技巧_image_8.png)

python生成器：用于储存大数据时减少内存（用把[]改为()）,通过生成器的next方法来访问数据
![](images/python技巧_image_9.png)

yield关键字：![](images/python技巧_image_10.png)

![](../_resources/c9f541d8d6a18e920f3512360808811c.png)

python文件的读写![](images/python技巧_image_11.png)

python中with的使用方法，调用with的对象必须要有__enter__和__exit__方法![](images/python技巧_image_12.png)

![](images/python技巧_image_13.png)

![](images/python技巧_image_14.png)

![](images/python技巧_image_15.png)

python中对象赋值，深拷贝，浅拷贝：
1.对象赋值，一个变 两个都会变![](images/python技巧_image_16.png)
2.    浅拷贝，两个对象地址不一样，但是可变数据类型的地址一样![](images/python技巧_image_17.png)

![](images/python技巧_image_18.png)

![](images/python技巧_image_19.png)

3.深拷贝![](images/python技巧_image_20.png)

python字典排序：
1.根据键排序：![](images/python技巧_image_21.png)