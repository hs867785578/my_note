logistic regression 其实解决的是一个二分类问题(不是回归问题)，之所以叫逻辑回归是有一些历史原因存在的，其中一个原因就是，logistic regression的输出其实是0-1的连续值，（线性方程+sigmoid）通过比较输出与0.5的大小来进行分类。

而softmax用于多分类问题时，保证的输出之和为1，当时可能有3、5、7....个输出，这可以看作每个类别的概率，我们选出最大值即为给出的预测类别（类比logistic regression，logistic regression的输出是两维，只能解决二分类问题，两个输出同样可以看作两个类别的概率）

![](images/logistic%20regression的一些见解_image_1.png)
