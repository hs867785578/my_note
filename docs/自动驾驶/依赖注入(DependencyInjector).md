## 概念

依赖注入是一种设计模式。举一个很简单的例子来说明：

	现在你是一个聚会的组织者，在本地聚会上，你需要准备烤面包、烤鸡、奶油蛋糕。那么你自己（组织者）作为一个独立的对象（class），需要依赖烤面包、烤鸡、奶油蛋糕对象。因此烤面包、烤鸡、奶油蛋糕作为三个依赖需要注入到你这个对象中。
	依赖注入可以使自身和自身的依赖项解耦，比如烤面包、烤鸡、奶油蛋糕可以交给不同的人去做。最后由组织者进行汇总即可，组织者不需要知道烤面包、烤鸡、奶油蛋糕是谁做的、是如何做的。而且可以很方便的对烤面包、烤鸡、奶油蛋糕这个三个依赖更换制作方法和制作人。
	自动驾驶中一个很简单的例子，每个模块，比如planning、control依赖一些上下文信息，比如planning依赖预测提供的目标信息、地图等。所以预测提供的目标信息、地图作为依赖需要注入到planning中。而这个注入的过程就是依赖注入。

## C++实现依赖注入的例子

IOC(控制反转)，是为了类对象间解耦合的一种设计思想。
DI(依赖注入),是一种IOC的实现,指的是依赖关系由外部注入，等等。

简单来说就是类A依赖类B，类B往往是一个抽象类（依赖倒置），类A不直接在内部new类B派生实现类的对象，因为这样会为类A引入了对类B具体派生类的直接依赖，转而由外部第三方DI容器生成对应的实现类对象，并通过构造函数之类注入类A对象里的类B成员变量中,而具体的实现类的选择则由外部配置来控制反转。

IOC和DI在java 后台开发领域非常常见，但是在C++目前也没有特别泛用的IOC框架（且更倾向用模板编译期行为来提高性能）。但是在日常开发中还是需要这种IOC的思想的。

根据SOILD原则，在代码开发，我们一般都会有抽象接口及多个接口实现子类，我们需要根据不同业务场景调用不同的接口实现子类，这种依赖关系实际是根据外部场景注入到实际业务类中。可以很方便业务的拓展及单元测试。

归根到底都是，寻找业务中容易变化的点，寻找解耦的点, 让其更加可控，让程序员改代码的时候容易些，同时对系统的影响更小。

## Apollo中的依赖注入

以planning模块为例，其依赖注入是一个名为DependencyInjector的类，在这个类中存储着planning需要用到的历史信息。

![](images/依赖注入(DependencyInjector)_image_1.png)

在planningcomponent中需要用到该注入器。用来提供规划的历史信息。

其他的一些消息比如map、routing等是通过cyber中的订阅器reader来读取的。
![](images/依赖注入(DependencyInjector)_image_2.png)

### 简介

- **planning包**：表示apollo/modules/planning文件夹下的内容
- **planning组件**：表示apollo/modules/planning/planning_component.h文件中定义的PlanningComponent类、或者对应的对象
- **planning模块**：表示来自apollo/modules/planning/planning_base.h中的PlanningBase类、对象，以及其子类，包括OnLanePlanning、NaviPlanning类、对象等，本文针对OnLanePlanning类展开讨论，即代指OnLanePlanning模块

 
 接下来介绍**planning组件**的逻辑梳理

### 数据流

要想更为清晰的理解planning模块的工作逻辑，需要对整个模块工作时的数据流进行初步了解。Apollo6.0 代码库的planning模块数据流大致如下图所示。

![](https://pic4.zhimg.com/80/v2-aebe95247c690555fb7dc0a0a65e52a3_720w.webp)

PlanningComponent 逻辑图

- 一个planning模块，作为planning的核心结构，负责规划工作
- 多个输入、输出通道(channel)，负责与其他模块进行数据通信
- 一个数据缓存区(DependencyInjector)，负责全程数据的缓存与交互
- 两个处理流，分别实现初始化(Init)，和消息响应处理(Proc)

以下逐个进行简要介绍

### 组件对象 PlanningComponent

- planning组件，即PlanningComponent类、对象，其继承自cyber::Component<M0, M1, M2, NullType>类，可在CyberRT中完成注册工作，实现消息响应机制的托管，是工程实现和算法实现之间的桥梁。
- planning组件是一个支持3个channel输入的消息回调型组件，支持的消息类型分别如下：

1. 预测消息：prediction::PredictionObstacles
2. 底盘消息：canbus::Chassis
3. 定位消息：localization::LocalizationEstimate

- CyberRT系统收到上述任何一个消息后都会调起Planning组件的Proc函数进行消息响应（与回调型组件对应的是定时器型组件，即CyberRT中的定时器会按着一定的频率调用Proc函数）。
- planning模块，是实际的planning核心框架，作为planning组件的成员变量，实现具体的规划功能，具体逻辑本文暂不展开讲解。

### 组件数据缓存 DependencyInjector

- DependencyInjector：依赖注入器，这是一个过于专业的名词，来自软件设计模式的**依赖倒置**原则的一种具体实现方式，起到模块解耦作用。
- DependencyInjector本质就是一个数据缓存中心，叫DataCacheCenter可能更贴切些。
- DependencyInjector对象内部管理者planning模块工作过程中的实时数据和几乎全部历史数据，以便于规划任务的前后帧之间的承接，以及异常处理的回溯。
- DependencyInjector是以空对象的形式引入到planning组件中，进而引入到planning模块中用来承载中间数据。 DependencyInjector的类结构如下图所示：

![](https://pic4.zhimg.com/80/v2-63f58d25b17c0665a958b962895a071b_720w.webp)

DependencyInjector结构图

- 依赖注入结构中主要有6类成员变量，分别如下：
- **PlanningContext** planning_context_：负责planning上下文的缓存，比如是否触发重新路由的ReroutingStatus信息
- **History** history_：负责障碍物状态的缓存，包括运动状态、决策结果。该数据与routing结果绑定，routing变更后会清理掉历史数据。
- **FrameHistory** frame_history_：是一个可索引队列，负责planning的输入、输出等主要信息的缓存，以**Frame**类进行组织，内部包含LocalView结构体（负责输入数据的融合管理）。与上述的History是不同的是，该缓数据自模块启动后就开始缓存所有的Frame对象，不受routing变动的影响。
- **EgoInfo** ego_info_：提供车辆动、静信息，即车辆运动状态参数（轨迹、速度、加速度等）和车辆结构参数（长宽高等）
- **apollo::common::VehicleStateProvider** vehicle_state_：车辆状态提供器，用于获取车辆实时信息
- **LearningBasedData** learning_based_data_：基于学习的数据，用于学习建模等

### 组件工作流

planning组件主要完成两项任务：初始化工作Init，消息响应工作Proc，具体过程可参考下图的1~6步

![](https://pic4.zhimg.com/80/v2-aebe95247c690555fb7dc0a0a65e52a3_720w.webp)

PlanningComponent逻辑图

#### 初始化

- 初始化工作由CyberRT系统的launch命令触发，大致分为2步，核心代码整理截图如下

![](https://pic3.zhimg.com/80/v2-1d1f97028915a8b09737fd2cdd1787a6_720w.webp)

Init()

- 1、planning模块的初始化，根据标志位选择具体的PlanningBase子类，并调用Init函数初始化，图中1所示
- 2、建立消息通道(channel)，包括输入通道traffic_light、routing等（图中2-1）和输出通道rerouting、planning等（图中2-2）

#### 消息响应

- 消息响应由CyberRT在接收到关联消息（上文有提到3类中的任何1类）到达后触发，工作大致分为4步，核心代码整理截图如下

![](https://pic3.zhimg.com/80/v2-8ad98516974c0b47bec2b00ea5caf706_720w.webp)

Proc()

- 3、工作前检查，包括重新进行路由的检查（图中3-1），如果需要会发送rerouting消息；输入数据融合与检查（图中3-2），数据来源包括外部传参和内部缓存数据，一并汇总到LocalView结构体中，实现融合，并最终会保存到Frame对象中
- 4、执行规划任务（图中4），此处会调用PlanningBase子类的RunOnce函数进行一次规划任务，生成规划后的轨迹
- 5、发布规划结果（图中5），利用已经创建的planning通道发送规划结果
- 6、更新历史缓存（图中6），将最终轨迹更新到history中（其他缓存在PlanningBase子类中完成的）以备不时之需
