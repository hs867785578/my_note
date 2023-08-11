
首先两者都是适用于非线性模型的，cost也是非线性的

## ILQR
iLQR是将**模型一阶线性化**，**cost function 二阶泰勒**近似，然后利用LQR求极值，在新极值的条件下，  
再次将环境一阶线性化，cost function 二阶泰勒近似，求极值，指导损失函数收敛；

## DDP

DDP是将**模型二阶线性化**，**cost function 二阶泰勒**近似