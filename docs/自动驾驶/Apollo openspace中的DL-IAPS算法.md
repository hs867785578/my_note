
主要解决路径规划中的非线性约束问题。
在路径规划避障时主要有两个非线性约束：
1. 位置约束：不能与障碍物的距离过近，有两次项
2. 曲率约束
![](images/Apollo%20openspace中的DL-IAPS算法_image_1.png)

原问题：
![](images/Apollo%20openspace中的DL-IAPS算法_image_2.png)

为了去掉非线性约束，做以下求解。
基本思路：迭代求解，每一次迭代，将：
1. 目标函数二次化
2. 等式约束线性化
3. 不等式约束二次化
![](images/Apollo%20openspace中的DL-IAPS算法_image_3.png)
![](images/Apollo%20openspace中的DL-IAPS算法_image_4.png)

化简后的问题：

![](images/Apollo%20openspace中的DL-IAPS算法_image_5.png)

![](images/Apollo%20openspace中的DL-IAPS算法_image_6.png)


![](images/Apollo%20openspace中的DL-IAPS算法_image_7.png)

![](images/Apollo%20openspace中的DL-IAPS算法_image_8.png)

trust region就是位置约束，一个二次项，需要把他线性化。