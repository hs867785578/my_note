问题一：
目前Apollo的预测交互，有没有考虑自车的决策？

答：没有，目前Apollo的预测交互只是用来服务于障碍物的预测，通过评价障碍车的多条轨迹（每条轨迹带有一定的先验概率，由行为预测+轨迹生成给出，或由LSTM直接给出）与自车的交互cost，来更新障碍车原来预测轨迹的概率分布。

问题二：
目前Apollo预测中车道线的，是由感知模块获取的吗？
答：不是，Apollo依赖于高精度地图，感知模块中的车道线检测一般用于高精地图的制作（标定）。

问题三：
planning中也有scenario模块，预测模块中的scenario（storytelling模块）和planning中有什么区别？

答：这是两个不同的概念，没什么联系。planning中scenario模块是一个状态机，用来判断自车所处的场景，然后调用哪些stage和task。预测模块中的scenario只有两个，一个交叉路口，一个直道。用来判断预测障碍物的优先级（谨慎（caution）、正常（normal）、忽略（ignore））

![](images/Apollo预测_image_1.png)


问题四：
Apollo中有哪些预测器？
答：
![](images/Apollo预测_image_2.png)

![](images/Apollo预测_image_3.png)

问题五：
规划的pipeline是什么？
6.0

![](images/Apollo预测_image_4.png)

![](images/Apollo预测_image_5.png)对于caution用LSTM属于直接预测轨迹（3s），然后进行轨迹的扩展Trajectory Extension（8s）

![](images/Apollo预测_image_6.png)对于normal，用MLP进行间接预测，先预测行为（分类问题），再采样生成轨迹（8s）。

![](images/Apollo预测_image_7.png)

7.0加入交互
![](images/Apollo预测_image_8.png)

问题六：预测中有3s和8s，有什么区别？
答：3s是神经网络预测（evaluator）预测出的轨迹长度，8s是经过后处理（predicator）后轨迹的长度

问题七：
评价指标是什么？
答：ADE和FDE。
![](images/Apollo预测_image_9.png)

问题八：
预测的输入数据，是激光雷达、摄像头的数据＋噪声，还是感知输出的数据？
答：用的是感知处理后的数据，也就是说Apollo的感知和预测是解耦的。

问题九：
Apollo7.0的预测和之前比有什么改进？
答：增加了交互式的预测模型，也就是通过交互来更新预测轨迹的后验概率。

问题十：
目前的预测耗时多少？
答：目前的前向传播预测，线上预测一个目标，在1080显卡上需要10ms量级。预测多个目前不会显著增加，可以多线程。

问题十一：
目前预测的pipeline？

除了之前提到的根据场景选择预测器之外。我们先预测target点（可以通过网格采样的方式，也可以通过在车道内采样的方式，后者考虑到高精地图，效率更高），根据target点（轨迹终点）来生成轨迹（可以通过神经网络生成，也可以根据规则来生成）。

也就是说对于Normal障碍物   先行为预测（接下来选择每一条车道的概率or十字路口选择每一个路口的概率），再生成轨迹（预测器为laneSequencePredictor）。

![](images/Apollo预测_image_10.png)
对于行人障碍物，直接预测轨迹
对于caution障碍物，先用神经网络预测行为，再生成轨迹（预测器为MoveSequencePredictor）
![](images/Apollo预测_image_11.png)

具体参考TNT论文

# **Target-driveN Trajectory Prediction**

![](images/Apollo预测_image_12.png)

![](images/Apollo预测_image_13.png)
对应上图的步骤
![](images/Apollo预测_image_14.png)

![](images/Apollo预测_image_15.png)

![](images/Apollo预测_image_16.png)

![](images/Apollo预测_image_17.png)

将交叉口或者执行道路行为预测（意图预测，evaluator）结果作为先验概率。采样速度生成预测轨迹，然后评价，选择，更新先验概率（该步骤在predictor上完成）。

![](images/Apollo预测_image_18.png)

![](images/Apollo预测_image_19.png)

问题十二：
在十字路口中，车道线都是虚拟交错的。怎那么保证预测的轨迹一定在车道线内？

答：这取决于用的预测器（evaluator），如果用的是lane sequence evaluator，那么是按照车道中心线走的，如果是别的预测器可能不会按照中心线走。

问题十三：
预测需要多长时间的历史轨迹?越长越好吗？
答：2s，不一定越长越好，历史轨迹越长需要的处理时间就越多。

问题十四：
目前caution障碍物最多几个？有效障碍物的距离范围是多少？
答：6个，60m

![](images/Apollo预测_image_20.png)

问题十五：
预测多久执行一次？预测轨迹中每个预测点间隔多少s
答：100ms，100ms

问题十六？
直接预测和间接预测的区别？目前Apollo用的哪一种？

答：直接预测是通过之前的轨迹，直接输入到神经网络预测后边几秒的轨迹。简介预测是通过先预测意图（比如要换道哪条车道，概率是多少），再根据意图采样生成轨迹。Apollo对于行人采用直接预测，对于车辆采用间接预测。

![](images/Apollo预测_image_21.png)

问题十七：
Apollo的evaluator和predictor有什么区别？
答：evaluator为前处理，预测短时轨迹或者意图。predictor为后处理，扩展短时轨迹成长时轨迹，或者根据意图采样生成长时域轨迹。
主要按照直接预测和间接预测来划分。

对于直接预测，evaluator一般由LSTM结合语义地图直接给出一段较短的预测轨迹（3s），比如vehicle中的interaction 和caution，比如SEMANTIC_LSTM_EVALUATOR和JOINTLY_PREDICTION_PLANNING_EVALUATOR。

对于间接预测evaluator一般由MLP给出各个行为意图的概率，比如JUNCTION_MLP_EVALUATOR和CRUISE_MLP_EVALUATOR和MLP_EVALUATOR。

![](images/Apollo预测_image_22.png)

predictor中，对于短时域的轨迹进行扩充![](images/Apollo预测_image_23.png)，生成长时域的轨迹

对于之前的意图，采样生成长时域的轨迹。

![](images/Apollo预测_image_24.png)  [](../_resources/f14c23dc792c1a95f0a6cd0e9cf10499.webp)![](images/Apollo预测_image_2.png)

**free_move_predictor**

首先介绍free_move_predictor，free_move_predictor是自由移动评估器，主要是预测不在路上的所有类型的障碍物以及行人，之前行人通过pedestrian_interaction_evaluator评估之后，只有2s的预测轨迹，后面会通过free_move_predictor补充6s的轨迹，一共预测8s。free_move_predictor是通过当前的速度和朝向拟合的8s轨迹，因此变化非常大，表现为预测线会甩来甩去的。

![](images/Apollo预测_image_25.png)

**lane_sequence_predictor**
lane_sequence_predictor会根据评估的车道，过滤一些不太可能的车道，然后再选择一条曲率最低的路径。

![](images/Apollo预测_image_26.png)
**move_sequence_predictor     **

move_sequence_predictor和lane_sequence_predictor有轻微不同，主要不同在生成轨迹的部分(DrawMoveSequenceTrajectoryPoints)，进行了多项式拟合EvaluateQuarticPolynomial，不知道是不是刚体的原因？