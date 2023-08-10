V是状态价值函数，Q是状态动作价值函数，V的输入是一个s（比如vector），输出是一个scalar，所以V本身是没有办法做决策的。Q的输入是s，输出是每一个a的价值（或者另一种形式，输入是s和a，输出是价值），所以Q是可以做决策的，如果训练好了Q就可以直接做决策（给定s，决定做什么a），而不需要policy gradient训练一个pi出来

pi的输入是一个s，输出是一个distribution，做决策是在这个distribution中做sample。本质是给s，来决定输出什么a

所以DQN是用Q来做决策的。
而对于V来说更像是一个辅助工具，没有办法做决策，需要和别的网络搭配使用

MC方法和TD方法（MC方法方差大，因为是基于采样的，TD的方法是可能估计不准）
![](images/对于Q(s，a)和V（s）的一些个人理解_image_1.png)

![](images/对于Q(s，a)和V（s）的一些个人理解_image_2.png)

DQN总会有高估的问题，原因如下（如果某一个a被高估了，那么被高估的a有很大可能会被选出来）
![](images/对于Q(s，a)和V（s）的一些个人理解_image_3.png)

解决方法：D（double）DQN，DDNQ相对于DQN其实是没有增加网络的，因为DQN实际训练的时候会有一个target network（Q‘），我们只要稍微改造一下target network，使其在选择a的时候用做决策的Q来选，而不是用自己（Q’）来选就好了。代码可能只需要改动一行

有两个Q，一个用来估计，一个用来决策

![](images/对于Q(s，a)和V（s）的一些个人理解_image_4.png)

dual DQN（改了network的架构）

![](images/对于Q(s，a)和V（s）的一些个人理解_image_5.png)

其他的trainDQN的tips
1、优先经验回放（如果TD error比较大，那么在buffer里会被较大概率sample到）
![](images/对于Q(s，a)和V（s）的一些个人理解_image_6.png)
2、muti-step
![](images/对于Q(s，a)和V（s）的一些个人理解_image_7.png)
3.noisy net
![](images/对于Q(s，a)和V（s）的一些个人理解_image_8.png)

4、Distributional Q-function，基本思想是因为s是随机的，所以给定当前的一组s，a，玩完一组游戏可能得到的累计奖励不一样，可以做一下统计（比如固定当前的s，a，玩多局游戏，统计最后累计奖励的分布），这样我们可以把最后的累计奖励看成是一个分布，我们用Q-function来估计这个distribution，每一个动作对应一个distribution，最后对每一个distribution求平均，取最大的动作来做决策。这是一个非常好的想法（但是不好训练起来）

![](images/对于Q(s，a)和V（s）的一些个人理解_image_9.png)