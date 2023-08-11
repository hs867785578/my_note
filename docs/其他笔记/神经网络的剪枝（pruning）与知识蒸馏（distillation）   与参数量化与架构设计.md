## 剪枝（pruning）

### 1、为什么不能先train一个小的network

一般来说train一个小的network往往不能打到较高的accurancy，所以一般的做法是先train一个大的network，然后做pruning（大乐透假说：先train一个大的network，里边包含很多小的network，每一个小的network不一定都能够train起来，但是先训练一个大的network可以提高小的network训练起来的个数，然后我们进行pruning，可以保留最好的结果，这就是因为initial weight很重要）

### 2、pruning的类型

weight pruning和neural pruning，一般来说用neural pruning，因为weight pruning是不规则的network，无法调用pytorch加速

（distillation）

![](images/神经网络的剪枝（pruning）与知识蒸馏（distillation）%20%20%20与参数量化与架构设计_image_1.png)

为什么这样效果好？为社么不直接train一个小的network，一种可能的解释是，大的network容易找到全局最优，而小的可能陷入局部最优，让小的跟着大的走效果好

### Parameter quantization（参数量化）

1、用更少的储存空间去存参数（比如32位 16位来存）
2、wieght clustering
![](images/神经网络的剪枝（pruning）与知识蒸馏（distillation）%20%20%20与参数量化与架构设计_image_2.png)

### 3、霍夫曼编码
### 4、binary weight

## 架构设计（CNN拆层）

动态计算
### 1、network动态调整深度

![](images/神经网络的剪枝（pruning）与知识蒸馏（distillation）%20%20%20与参数量化与架构设计_image_3.png)

### 2、动态调整宽度

### 3、机器自己决定是否调整深度与宽度
![](images/神经网络的剪枝（pruning）与知识蒸馏（distillation）%20%20%20与参数量化与架构设计_image_4.png)

![](images/神经网络的剪枝（pruning）与知识蒸馏（distillation）%20%20%20与参数量化与架构设计_image_5.png)