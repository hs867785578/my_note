1. **在path planning时是否考虑动态障碍物？**
答：其实是考虑了动态障碍物，目前新石器的方案在求解后端优化的时候，需要给一个粗解（期望），这个期望是考虑了动态障碍物的。
在确定boundary的时候不考虑动态障碍物，只考虑静态障碍物（小于1m/s）。

1. **为什么Apollo不要期望，直接可以根据boundary来求后端的优化path，而新石器需要一个粗解（考虑动态障碍物）来作为后端优化的期望。**

答：主要是因为Apollo的场景比较简单，一条lane大概一般就是3.5m，变化不是很大，所以一般不需要期望，或者直接用车道中心作为期望。但是新石器的场景相对而言，非结构化更强，一条路的宽度可能变化比较大（忽宽忽窄），因此最好给一个期望，在期望中考虑绕行的次数和动态障碍物，防止绕行次数过多。

1. **目前freespace的形式**
答：目前freespace给出的形式一般是一堆离散的点。

1. **新石器的insidedecider和outsidedecider有什么用？**
答：新石器中的insideplanner和outsideplanner是为了隔离stage和task设计的。

以Apollo为例，apollo中是scenario->stage->task的架构，其中是否换道、是否借道的决策是在task中的decider判断的。通过给task中的decider传入&frame和&referenceline info，来填充referenceline_info中的boundary信息。

新石器中不太一样，新石器中insideplanner和outsideplanner合起来有点类似于referenceline info。新石器中是否换道、借到的决策是由scenario中决定的（有两个decider，外部的decider判断是否换道、借到，是语义级别decider，内部的decider位于task中，用来确定boundary），外部的decider判断是否换道、借道，并填充insideplanner。内部task中的decider根据insideplanner中的信息来填充boundary。总之，insideplanner储存的是语义级别的决策信息，outsideplanner是根据 insideplanner的信息，来确定boundary。