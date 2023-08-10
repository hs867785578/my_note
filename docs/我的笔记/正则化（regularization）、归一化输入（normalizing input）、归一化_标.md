正则化（regularization）是为了防止过拟合的，即把权重放到代价函数里，常用的的L1、L2正则化，相当于给权重加了约束
![](images/正则化（regularization）、归一化输入（normalizing%20input）、归一化_标_image_1.jpg)

归一化输入（normalizing input）指的是将输入的图片的像素范围由 [0,255]->[0,1]->[-1,1]

其中[0,255]->[0,1]由transformer.ToTensor（）完成，[0,1]->[-1,1]又transformer.normaliza（mean=[0.5]，std=0.5）完成，这样使像素==变为了均值为0，方差为1的数据，有利于加快训练==

![](images/正则化（regularization）、归一化输入（normalizing%20input）、归一化_标_image_2.png)

标准化batch（batchnormalization ）和归一化输入（normalizing input）的作用一样