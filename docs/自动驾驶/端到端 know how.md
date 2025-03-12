## 1 RL开发过程中的trick

### 1.1 reward系数搜索

每个reward的scale都不一样，所以每一项需要乘以相应的系数，这个系数属于超参数需要人工设计，可以把这个系数搜索抽象为一个优化问题： 输入多条rollout的轨迹和GT轨迹，目标是距离GT made最小（最近）的rollout轨迹的reward最大。可以采用暴力搜索，和贝叶斯优化等方法。

### 1.2 overfit/test ont train实验
RL不依赖GT，在RL阶段，可以用少量的case 做overfit实验，来检验reward设计是否合理。如果少量case能够overfit成功，说明reward的导向正确， 如果不能overfit成功，说明reward设计有问题。（注意在这个过程中，不能引入IL 的loss）


### 1.3 reward可视化工具

有一个可视化每一条rollout的轨迹、GT轨迹、GT noise、voc的轨迹的每一项reward的工具，可以帮助我们快速分析reward设计是否合理，reward是否存在bug等。这个工具可以使用bokeh设计。


### 1.4 GRPO训练
采用policy gradient的RL方法时，可以引入GRPO，其中除以方差的操作可以把纵向和横向拉到一个reward尺度，对横向表现，例如居中避让有很大提升。


### 1.5 reward设计
#### **1.** **`oc_reward`** **(Object Collision Reward)**

**含义**：避免与其他物体碰撞的奖励函数 **计算方式**：

- 使用旋转IoU (Intersection over Union)来检测自车与其他车辆/物体是否碰撞
    
- 根据mode参数可以返回不同形式的惩罚：
    
    - 'iou'：负的IoU值作为惩罚
        
    - 'bool'：碰撞为-1，无碰撞为0
        
    - 'risk'：基于碰撞和相对速度计算风险惩罚（速度越高惩罚越大）
        
- 可以忽略在ground truth轨迹中已经发生碰撞的物体
    

#### **2.** **`rbc_reward`** **(Road Boundary Collision Reward)**

**含义**：避免与道路边界碰撞的奖励函数 **计算方式**：

- 检测自车是否与道路边界（实线或虚线）发生碰撞
    
- 如果发生碰撞，给予负奖励，且惩罚大小与车速成正比
    

#### **3.** **`slbc_reward`** **(Solid Line Boundary Collision Reward)**

**含义**：避免与实线车道线碰撞的奖励函数 **计算方式**：

- 检测自车是否与实线车道线发生碰撞
    
- 如果发生碰撞，给予负奖励
    

#### **4.** **`dlbc_reward`** **(Dashed Line Boundary Collision Reward)**

**含义**：避免与虚线车道线碰撞的奖励函数 **计算方式**：

- 检测自车是否与虚线车道线发生碰撞
    
- 如果发生碰撞，给予负奖励（通常比实线车道线惩罚轻）
    

#### **5.** **`acc_reward`** **(Acceleration Reward)**

**含义**：限制过大加速度的奖励函数 **计算方式**：

- 当加速度大于阈值（默认3.0）时，给予负奖励
    
- 惩罚大小与超出阈值的加速度成正比
    
- 奖励范围被限制在[-1.0, 0.0]
    

#### **6.** **`jerk_reward`** **(Jerk Reward)**

**含义**：限制过大加加速度(jerk)的奖励函数 **计算方式**：

- 当jerk大于阈值（默认4.0）时，给予负奖励
    
- 惩罚大小与超出阈值的jerk成正比
    
- 奖励范围被限制在[-1.0, 0.0]
    

#### **7.** **`efficiency_reward`** **(Efficiency Reward)**

**含义**：鼓励车辆沿预测车道线方向高效行驶的奖励函数 **计算方式**：

- 计算车辆在车道方向上的速度分量
    
- 根据不同模式计算奖励：
    
    - 'velocity'：在适当速度范围内奖励，过慢或过快都有惩罚
        
    - 'diff_velocity'：根据速度变化给予奖励
        
    - 'gt_based'：与ground truth轨迹速度比较，鼓励车辆按照或略快于ground truth速度行驶
        

#### **8.** **`neg_speed_reward`** **(Negative Speed Reward)**

**含义**：惩罚车辆在车道方向上的负速度（倒车或严重偏离） **计算方式**：

- 当车辆在车道方向上的速度为负且低于阈值时，给予负奖励
    

#### **9.** **`deviation_lp_reward`** **(Lane Position Deviation Reward)**

**含义**：鼓励车辆保持在预测车道线附近的奖励函数 **计算方式**：

- 计算车辆与预测车道线的横向距离
    
- 当距离超过阈值（默认2.0米）时，给予负奖励
    
- 惩罚大小与超出阈值的距离成正比
    

#### **10.** **`cruise_fast_than_gt_reward`** **(Cruise Faster Than Ground Truth Reward)**

**含义**：鼓励车辆在某些情况下比ground truth轨迹行驶更快的奖励函数 **计算方式**：

- 计算预测轨迹与ground truth轨迹的速度差
    
- 当预测轨迹速度大于ground truth速度时，给予正奖励
    

#### **11.** **`deviation_lc_reward`** **(Lane Center Deviation Reward)**

**含义**：鼓励车辆居中行驶的奖励函数 **计算方式**：

- 计算车辆与车道中心线的偏离距离
    
- 偏离越大，给予的负奖励越大
    
- 奖励范围被限制在[-1.0, 0.0]
    

#### **12.** **`traj_diff_reward`** **(Trajectory Difference Reward)**

**含义**：评估预测轨迹与ground truth轨迹差异的奖励函数 **计算方式**：

- 计算预测轨迹点与ground truth轨迹点的欧氏距离
    
- 距离越大，给予的负奖励越大
    
- 奖励范围被限制在[-10.0, 0.0]
    

## **综合概括**

这些reward函数共同构成了一个全面的奖励系统，用于训练自动驾驶系统：

1. **安全性**：通过`oc_reward`、`rbc_reward`、`slbc_reward`和`dlbc_reward`避免碰撞
    
2. **舒适性**：通过`acc_reward`和`jerk_reward`限制过大的加速度和加加速度
    
3. **效率性**：通过`efficiency_reward`和`cruise_fast_than_gt_reward`鼓励高效行驶
    
4. **规范性**：通过`deviation_lp_reward`和`deviation_lc_reward`鼓励车辆在车道内居中行驶
    
5. **相似性**：通过`traj_diff_reward`鼓励生成的轨迹与专家轨迹相似
    

所有这些reward函数都与`sample_collide_within_gt_mask`相乘，这是一个用于过滤那些在ground truth轨迹中已经发生碰撞的样本的掩码，确保模型不会因为避免不可避免的碰撞而受到惩罚。

## 2 IL开发过程中的trick
### 2.1 梯度分析

#### 背景

- 目前EBM DLP模型需要支持的场景、任务非常多。
- 不同场景的数据、不同任务的Loss function彼此之间会非常深远的影响。
- 希望有个一个方法能够量化各组<场景-任务>之间的冲突程度，用于指导训练策略、数据配比的修改、脏数据的排查、模型结构的调整。
- 目前发版巡航指标不准出的概率较大，原因也不是特别清楚，所以先以巡航指标作为最小闭环。
#### 方法

##### 探针梯度设计

1. 设计一个训练配置，能够让模型在目标场景的MCT指标上稳定变好。
    
2. 然后固定模型权重，统计多个batch的样本数据，将平均后的梯度保留下来作为探针梯度。
    
3. 这个梯度的方向，就可以在当前的权重下被认为是对目标场景优化有利的方向。
    

##### 采样梯度设计

1. 找到探针梯度后，基于同样的模型权重，以及效果不符合预期的训练配置，对训练的样本进行遍历
    
2. 每个样本反传得到的梯度即为采样梯度。
    
3. 每个采样梯度和探针梯度进行metric计算。
    

#### Metric设计
1. cosine_similarity：用于量化采样梯度和探针梯度优化方向的差异。
2. gradient_dot_product：用于量化实际训练过程中，对探针梯度干扰的的严重程度。相比于cosine_similarity，额外考虑了采样梯度的模长。
    
####  截至目前观察到的现象/结论

分析top cosine_similarity、gradient_dot_product后

1. 发现top case大多对于巡航而言是脏数据。
2. 定位发现到了DLP训练数据中的自动数据的dlp_loss没有被正确mask的bug，目前已经修复。
3. 定位发现了辅助监督的loss，存在梯度过大的问题，目前已经修复。
4. 定位发现了DLP训练数据中相当一部分数据未经过巡航脏数据清洗，目前已经修复，对巡航指标有明显改善。
5. 定位发现了cruise_base数据集可能对巡航正向的作用有限，目前替换成cruise_cruise数据，对巡航指标有明显改善。
6. 定位发现了经过脏数据清洗的数据仍旧存在脏数据的情况，目前已经反馈给cruise的相关同学。