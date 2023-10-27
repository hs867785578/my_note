
核心思想：前端粗搜错，后端优化。后端优化时，把障碍物建模成超平面集合，自车也用四个超平面来表示，避障约束为两个集合不相交，通过设定一个函数来表示两个车的距离。引入对偶变量λ和μ。

OBCA:用超平面表示障碍物。

H-OBCA相对于OBCA，引入了分层的思想，先用混合a星搜一个renference。

TDR-OBCA相对于之前的H-OBCA，不仅引入了分层的思想，而且引入一个**dual variable warm start** （求解一个QP问题）以及将mpc 中的一些硬约束加入到了cost function中，使得mpc的求解更为快速和鲁棒。
![](images/Apollo中TDR-OBCA算法_image_1.png)

前端：
Hbyrid A\*和ReedShepp算法规划开放空间无碰撞的路径。但是这一算法存在的问题是由于Hybrid A\*对转向角度离散化，所以**两个节点的曲率是突变**的；ReedShepp曲线也是，在两个曲线交点位**置曲率也是突变的**，这样的轨迹是不适合控制车辆跟踪轨迹行驶的。所以还需要规划出一条平滑无碰撞的路径。

后端：
OBCA优化。OBCA算法是基于模型预测控制（MPC）建立模型，并用优化算法进行求解。

1. 构建MPC的状态方程
![](images/Apollo中TDR-OBCA算法_image_2.png)

2. 构建障碍物约束
在OBCA算法中是用超平面构建障碍物集，对于二维空间，超平面就是条直线。
比如对于障碍物的Bonding Box有4条边，则用4条直线描述障碍物占据的空间。
比如对于一条边线的超平面：
![](images/Apollo中TDR-OBCA算法_image_3.png)
![](images/Apollo中TDR-OBCA算法_image_4.png)
![](images/Apollo中TDR-OBCA算法_image_5.png)

![](images/Apollo中TDR-OBCA算法_image_6.png)

![](images/Apollo中TDR-OBCA算法_image_7.png)

自车占据空间的也可以为4个超平面相交的集合表示，假设自车中心位于原点的空间表示为
![](images/Apollo中TDR-OBCA算法_image_8.png)

![](images/Apollo中TDR-OBCA算法_image_9.png)

![](images/Apollo中TDR-OBCA算法_image_10.png)

![](images/Apollo中TDR-OBCA算法_image_11.png)


![](images/Apollo中TDR-OBCA算法_image_12.png)![](images/Apollo中TDR-OBCA算法_image_13.png)