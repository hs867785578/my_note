本文是Apollo项目系列文章中的一篇，会解析自动驾驶系统中最核心的模块 - 决策规划模块。

# 前言

Apollo系统中的Planning模块实际上是整合了决策和规划两个功能，该模块是自动驾驶系统中最核心的模块之一（另外三个核心模块是：**定位**，**感知**和**控制**）。

关于决策规划的理论值得我们研究好久。所以接下来会通过几篇文章来专门讲解Planning模块。

这些文章会以百度Apollo的Planning模块实现为基础。当然，我也希望我们能够不单单局限于Apollo项目实现。如果可以，尽可能的一起了解一下目前这些技术的其他研究成果。

本文是讲解Planning模块的第一篇文章，会对Planning模块的整体架构做一些解析。

其他关于Apollo相关的文章如下：

- [解析百度Apollo自动驾驶平台](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fbaidu-apollo%2F)

- [解析百度Apollo之Routing模块](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fapollo-routing%2F)

本文以Apollo项目2019年初的版本为基础进行讲解。

- 版本：3.5

- 获取代码时间：2019年1月24日

# Apollo系统与Planning模块

下图是Apollo系统的整体架构图。从这幅图中我们可以看出，整个系统可以分为5层。从下至上依次是：

- 车辆认证平台：经过Apollo认证的电子线控车辆。

- 硬件开发平台：包含了计算单元以及各种传感器，例如：GPS，摄像头，雷达，激光雷达等等。

- 开放软件平台：这就是Apollo开源代码的主体，也是自动驾驶最核心的部分。

- 云服务平台：包含了各种云端服务，实现自动驾驶的车辆一定不是孤立的，而是跑在基于互联网的云端上的。

- 量产交付方案：专门为各种场景量产的解决方案。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_1.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_1.png)

我们可以再将最核心的开放软件平台这一层放大近看一下。

下面这幅图描述了这其中的模块和它们之间的交互关系。

这其中的黄线代表了数据流，黑线代表了控制流。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_2.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_2.png)


Planning模块负责整个车辆的驾驶决策，而驾驶决策需要根据当前所处的地理位置，周边道路和交通情况来决定。Planning不直接控制车辆硬件，而是借助于控制模块来完成。

从这幅图中可以看出，对于Planning模块来说：

- 它的上游模块是：定位，地图，导航，感知和预测模块。

- 它的下游模块是控制模块。

# 模块概述

决策规划模块的主要责任是：**根据导航信息以及车辆的当前状态，在有限的时间范围内，计算出一条合适的轨迹供车辆行驶**。

这里有好几个地方值得注意：

1. 车辆的行驶路线通常由Routing模块提供，Routing模块会根据目的地以及地图搜索出一条代价尽可能小的路线。关于Routing模块我们已经在另外一篇文章中详细讲解过（[《解析百度Apollo之Routing模块》](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fapollo-routing%2F)），这里不再赘述。

2. 车辆的当前状态包含了很多因素，例如：车辆自身的状态（包括姿态，速度，角速度等等），当前所处的位置，周边物理世界的静态环境以及交通状态等等。

3. Planning模块的响应速度必须是稳定可靠的（当然，其他模块也是一样）。正常人类的反应速度是300ms，而自动驾驶车辆想要做到安全可靠，其反应时间必须短于100ms。所以，Planning模块通常以10Hz的频率运行着。如果其中某一个算法的时间耗费了太长时间，就可能造成其他模块的处理延迟，最终可能造成严重的后果。例如：没有即时刹车，或者转弯。

4. ”合适的轨迹“有多个层次的含义。首先，”轨迹“不同于“路径”，“轨迹”不仅仅包含了行驶路线，还要包含每个时刻的车辆的速度，加速度，方向转向等信息。其次，这条轨迹必须是底层控制可以执行的。因为车辆在运动过程中，具有一定的惯性，车辆的转弯角度也是有限的。在计算行驶轨迹时，这些因素都要考虑。最后，从人类的体验上来说，猛加速，急刹车或者急转弯都会造成非常不好的乘坐体验，因此这些也需要考虑。这就是为什么决定规划模块需要花很多的精力来优化轨迹，Apollo系统中的实现自然也不例外。

# 模块架构

Apollo的之前版本，包括3.0都是用了相同的配置和参数规划不同的场景，这种方法虽然线性且实现简单，但不够灵活或用于特定场景。随着Apollo的成熟并承担不同的道路条件和驾驶用例，Apollo项目组采用了更加模块化、适用于特定场景和整体的方法来规划其轨迹。

在这个方法中，每个驾驶用例都被当作不同的驾驶场景。这样做非常有用，因为与先前的版本相比，现在在特定场景中报告的问题可以在不影响其他场景的工作的情况下得到修复，其中问题修复影响其他驾驶用例，因为它们都被当作单个驾驶场景处理。

Apollo 3.5中Planning模块的架构如下图所示：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_3.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_3.png)

这其中主要的组件包括：

- Apollo FSM：一个有限状态机，与高清地图确定车辆状态给定其位置和路线。

- Planning Dispatcher：根据车辆的状态和其他相关信息，调用合适的Planner。

- Planner：获取所需的上下文数据和其他信息，确定相应的车辆意图，执行该意图所需的规划任务并生成规划轨迹。它还将更新未来作业的上下文。

- Deciders和Optimizers：一组实现决策任务和各种优化的无状态库。优化器特别优化车辆的轨迹和速度。决策者是基于规则的分类决策者，他们建议何时换车道、何时停车、何时爬行（慢速行进）或爬行何时完成。

- 黄色框：这些框被包含在未来的场景和/或开发人员中，以便基于现实世界的驱动用例贡献他们自己的场景。

# 整体Pipeline

有相关专业知识的人都会知道，决策规划模块的主体实现通常都是较为复杂的。

Apollo系统中的实现自然也不例外，这里先通过一幅图说明其整体的Pipeline。然后在下文中我们再逐步熟悉每个细节。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_4.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_4.png)

这里有三个主要部分需要说明：

- `PncMap`：全称是Planning and Control Map。这个部分的实现并不在Planning内部，而是位于`/modules/map/pnc_map/`目录下。但是由于该实现与Planning模块紧密相关，因此这里放在一起讨论。该模块的主要作用是：根据Routing提供的数据，生成Planning模块需要的路径信息。

- `Frame`：Frame中包含了Planning[一次计算循环](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fapollo-planning%2F%23id-planningcycle)中需要的所有数据。例如：地图，车辆状态，参考线，障碍物信息等等。`ReferenceLine`是车辆行驶的参考线，`TrafficDecider`与交通规则相关，这两个都是Planning中比较重要的子模块，因此会在下文中专门讲解。

- `EM Planner`：下文中我们会看到，Apollo系统中内置了好几个Planner，但目前默认使用的是EM Planner，这也是专门为开放道路设计的。该模块的实现可以说是整个Planning模块的灵魂所在。因此其算法值得专门用另外一篇文章来讲解。读者也可以阅读其官方论文来了解：[Baidu Apollo EM Motion Planner](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Farxiv.org%2Fabs%2F1807.08048)。

# 基础数据结构

Planning模块是一个比较大的模块，因此这其中有很多的数据结构需要在内部实现中流转。

这些数据结构集中定义在两个地方：

- `proto`目录：该目录下都是通过[Protocol Buffers](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fdevelopers.google.com%2Fprotocol-buffers%2F)格式定义的结构。这些结构会在编译时生成C++需要的文件。这些结构没有业务逻辑，就是专门用来存储数据的。（实际上不只是Planning，几乎每个大的模块都会有自己的proto文件夹。）

- `common`目录：这里是C++定义的数据结构。很显然，通过C++定义数据结构的好处是这些类的实现中可以包含一定的处理逻辑。

## proto

proto目录下的文件如下所示：

```
apollo/modules/planning/proto/
├── auto_tuning_model_input.proto
├── auto_tuning_raw_feature.proto
├── decider_config.proto
├── decision.proto
├── dp_poly_path_config.proto
├── dp_st_speed_config.proto
├── lattice_sampling_config.proto
├── lattice_structure.proto
├── navi_obstacle_decider_config.proto
├── navi_path_decider_config.proto
├── navi_speed_decider_config.proto
├── pad_msg.proto
├── planner_open_space_config.proto
├── planning.proto
├── planning_config.proto
├── planning_internal.proto
├── planning_stats.proto
├── planning_status.proto
├── poly_st_speed_config.proto
├── poly_vt_speed_config.proto
├── proceed_with_caution_speed_config.proto
├── qp_piecewise_jerk_path_config.proto
├── qp_problem.proto
├── qp_spline_path_config.proto
├── qp_st_speed_config.proto
├── reference_line_smoother_config.proto
├── side_pass_path_decider_config.proto
├── sl_boundary.proto
├── spiral_curve_config.proto
├── st_boundary_config.proto
├── traffic_rule_config.proto
└── waypoint_sampler_config.proto
```

我们在Routing模块讲解的文章中已经提到，通过proto格式定义数据结构好处有两个：

- 自动生成C++需要的数据结构。

- 可以方便的从文本文件导入和导出。下文将看到，Planning模块中有很多配置文件就是和这里的proto结构相对应的。

## common

common目录下的头文件如下：

```
apollo/modules/planning/common/
├── change_lane_decider.h
├── decision_data.h
├── distance_estimator.h
├── ego_info.h
├── frame.h
├── frame_manager.h
├── indexed_list.h
├── indexed_queue.h
├── lag_prediction.h
├── local_view.h
├── obstacle.h
├── obstacle_blocking_analyzer.h
├── path
│   ├── discretized_path.h
│   ├── frenet_frame_path.h
│   └── path_data.h
├── path_decision.h
├── planning_context.h
├── planning_gflags.h
├── reference_line_info.h
├── speed
│   ├── speed_data.h
│   ├── st_boundary.h
│   └── st_point.h
├── speed_limit.h
├── speed_profile_generator.h
├── threshold.h
├── trajectory
│   ├── discretized_trajectory.h
│   ├── publishable_trajectory.h
│   └── trajectory_stitcher.h
└── trajectory_info.h
```

这里有如下一些结构值得我们注意：

名称
说明
`EgoInfo`类
包含了自车信息，例如：当前位置点，车辆状态，外围Box等。
`Frame`类
包含了一次Planning计算循环中的所有信息。
`FrameManager`类
Frame的管理器，每个Frame会有一个整数型id。
`LocalView`类
Planning计算需要的输入，下文将看到其定义。
`Obstacle`类
描述一个特定的障碍物。障碍物会有一个唯一的id来区分。
`PlanningContext`类

Planning全局相关的信息，例如：是否正在变道。这是一个[单例](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSingleton_pattern)。

`ReferenceLineInfo`类
车辆行驶的参考线，下文会专门讲解。
`path`文件夹
描述车辆路线信息。包含：PathData，DiscretizedPath，FrenetFramePath三个类。
`speed`文件夹
描述车辆速度信息。包含SpeedData，STPoint，StBoundary三个类。
`trajectory`文件夹
描述车辆轨迹信息。包含DiscretizedTrajectory，PublishableTrajectory，TrajectoryStitcher三个类。
`planning_gflags.h`
定义了模块需要的许多常量，例如各个配置文件的路径。

>
> 你暂时不用记住所有这些类，在后面的文章中，我们会逐渐知道它们的作用。
>

# 模块配置

在下文中大家会发现，Planning模块中有很多处的逻辑是通过配置文件控制的。通过将这部分内容从代码中剥离，可以方便的直接对配置文件进行调整，而不用编译源代码。这对于系统调试和测试来说，是非常方便的。

Apollo系统中，很多模块都是类似的设计。因此每个模块都会将配置文件集中放在一起，也就是每个模块下的`conf`目录。

Planning模块的配置文件如下所示：

```
apollo/modules/planning/conf/
├── adapter.conf
├── cosTheta_smoother_config.pb.txt
├── navi_traffic_rule_config.pb.txt
├── planner_open_space_config.pb.txt
├── planning.conf
├── planning_config.pb.txt
├── planning_config_navi.pb.txt
├── planning_navi.conf
├── qp_spline_smoother_config.pb.txt
├── scenario
│   ├── lane_follow_config.pb.txt
│   ├── side_pass_config.pb.txt
│   ├── stop_sign_unprotected_config.pb.txt
│   ├── traffic_light_protected_config.pb.txt
│   └── traffic_light_unprotected_right_turn_config.pb.txt
├── spiral_smoother_config.pb.txt
└── traffic_rule_config.pb.txt
```

这里的绝大部分文件都是`.pb.txt`后缀的。因为这些文件是和上面提到的proto结构相对应的。因此可以直接被proto文件生成的数据结构读取。对于不熟悉的读者可以阅读Protocal Buffer的文档：[google.protobuf.text_format](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fdevelopers.google.com%2Fprotocol-buffers%2Fdocs%2Freference%2Fcpp%2Fgoogle.protobuf.text_format)。

读者暂时不用太在意这些文件的内容。随着对于Planning模块实现的熟悉，再回过来看这些配置文件，就很容易理解每个配置文件的作用了。下文中，对一些关键内容我们会专门提及。

# Planner

## Planning与Planner

Apollo 3.5废弃了原先的ROS，引入了新的运行环境：[Cyber RT](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Ftree%2Fmaster%2Fcyber)。

Cyber RT[以组件的方式来管理各个模块](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Fblob%2Fmaster%2Fdocs%2Fcyber%2FCyberRT_Quick_Start.md)，组件的实现会基于该框架提供的基类：`apollo::cyber::Component`。

Planning模块自然也不例外。其实现类是下面这个：

```
class PlanningComponent final

    : public cyber::Component<prediction::PredictionObstacles, canbus::Chassis, localization::LocalizationEstimate>

```

在`PlanningComponent`的实现中，会根据具体的配置选择Planning的入口。Planning的入口通过`PlanningBase`类来描述的。

PlanningBase只是一个抽象类，该类有三个子类：

- OpenSpacePlanning

- NaviPlanning

- StdPlanning

`PlanningComponent::Init()`方法中会根据配置选择具体的Planning入口：

`bool PlanningComponent::Init() { if (FLAGS_open_space_planner_switchable) { planning_base_ = std::make_unique<OpenSpacePlanning>(); } else { if (FLAGS_use_navigation_mode) { planning_base_ = std::make_unique<NaviPlanning>(); } else { planning_base_ = std::make_unique<StdPlanning>(); } } `

目前，`FLAGS_open_space_planner_switchable`和`FLAGS_use_navigation_mode`的配置都是false，因此最终的Planning入口类是：`StdPlanning`。

下面这幅图描述了上面说到的这些逻辑：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_5.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_5.png)

所以接下来，我们只要关注StdPlanning的实现即可。在这个类中，下面这个方法是及其重要的：

```
/**
* @brief main logic of the planning module,
* runs periodically triggered by timer.
*/

void RunOnce(const LocalView& local_view, ADCTrajectory* const trajectory_pb) override;

```

方法的注释已经说明得很清楚了：这是Planning模块的主体逻辑，会被timer以固定的间隔调用。每次调用就是一个规划周期。

## PlanningCycle

很显然，接下来我们重点要关注的就是`StdPlanning::RunOnce`方法的逻辑。该方法的实现较长，这里就不贴出代码了，而是通过一幅图描述其中的逻辑：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_6.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_6.png)

请读者尽可能仔细的关注一下这幅图，因为它涵盖了下文我们要讨论的所有内容。

## Planner概述

在最新的Apollo源码中，一共包含了5个Planner的实现。它们的结构如下图所示：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_7.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_7.png)

每个Planner都会有一个字符串描述的唯一类型，在配置文件中（见下文）通过这个类型来选择相应的Planner。

这5个Planner的说明如下表所示：

| 名称  | 加入版本 | 类型  | 说明  |
| --- | --- | --- | --- |
| RTKReplayPlanner | 1.0 | RTK | 根据录制的轨迹来规划行车路线。 |
| PublicRoadPlanner | 1.5 | PUBLIC_ROAD | 实现了[EM算法](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Farxiv.org%2Fabs%2F1807.08048)的规划器，这是目前的默认Planner。 |
| LatticePlanner | 2.5 | LATTICE | 基于网格算法的轨迹规划器。 |
| NaviPlanner | 3.0 | NAVI | 基于实时相对地图的规划器。 |
| OpenSpacePlanner | 3.5 | OPEN_SPACE | 算法源于论文：[《Optimization-Based Collision Avoidance》](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Farxiv.org%2Fpdf%2F1711.03449.pdf)。 |

RTKReplayPlanner基于录制的轨迹，是比较原始的规划器，所以不用多做说明。最新加入的两个规划器（NaviPlanner和OpenSpacePlanner）目前看来还需要更多时间的验证，我们暂时也不会过多讲解。

Apollo公开课里对两个较为成熟的Planner：EM Planner和Lattice Planner做了对比，我们可以一起来看一下：

| EM Planner | Lattice Planner |
| --- | --- |
| 横向纵向分开求解 | 横向纵向同时求解 |
| 参数较多（DP/QP, Path/Speed） | 参数较少且统一 |
| 流程复杂 | 流程简单 |
| 单周期解空间受限 | 简单场景解空间较大 |
| 能适应复杂场景 | 适合简单场景 |
| 适合城市道路 | 适合高速场景 |

后面的内容中，我们会尽可能集中在EM Planner算法上。对于Lattice Planner感兴趣的读者可以继续阅读这篇文章：[《Lattice Planner规划算法》](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FYDIoVf20kybu8JEUY3GZWg)。

## Planner配置

Planner的配置文件路径是在`planning_gflags.cc`中指定的，相关内容如下：

```
// /modules/planning/common/planning_gflags.cc

DEFINE_string(planning_config_file, "/apollo/modules/planning/conf/planning_config.pb.txt", "planning config file");

```

接下来我们可以看一下`planning_config.pb.txt`中的内容：

```
// modules/planning/conf/planning_config.pb.txt

standard_planning_config {

  planner_type: PUBLIC_ROAD planner_type: OPEN_SPACE planner_public_road_config { scenario_type: LANE_FOLLOW scenario_type: SIDE_PASS scenario_type: STOP_SIGN_UNPROTECTED } }

```

这里设置了两个Planner，最终选择哪一个由下面这个函数决定：

`std::unique_ptr<Planner> StdPlannerDispatcher::DispatchPlanner() { PlanningConfig planning_config; apollo::common::util::GetProtoFromFile(FLAGS_planning_config_file, &planning_config); if (FLAGS_open_space_planner_switchable) { return planner_factory_.CreateObject( planning_config.standard_planning_config().planner_type(1)); } return planner_factory_.CreateObject( planning_config.standard_planning_config().planner_type(0)); } `

`open_space_planner_switchable`决定了是否能够切换到OpenSpacePlanner上。但目前这个配置是`false`：

```
// /modules/planning/common/planning_gflags.cc

DEFINE_bool(open_space_planner_switchable, false, "true for std planning being able to switch to open space planner " "when close enough to target parking spot");

```

## PublicRoadPlanner

`PublicRoadPlanner`是目前默认的Planner，它实现了EM（Expectation Maximization）算法。

Planner的算法实现依赖于两个输入：

- 车辆自身状态：通过`TrajectoryPoint`描述。该结构中包含了车辆的位置，速度，加速度，方向等信息。

- 当前环境信息：通过`Frame`描述。前面我们已经提到，`Frame`中包含了一次Planning计算循环中的所有信息。

在`Frame`中有一个数据结构值得我们重点关于一下，那就是`LocalView`。这个类在前面我们也已经提到过。它的定义如下：

```
struct LocalView {

  std::shared_ptr<prediction::PredictionObstacles> prediction_obstacles; std::shared_ptr<canbus::Chassis> chassis; std::shared_ptr<localization::LocalizationEstimate> localization_estimate; std::shared_ptr<perception::TrafficLightDetection> traffic_light; std::shared_ptr<routing::RoutingResponse> routing; bool is_new_routing = false; std::shared_ptr<relative_map::MapMsg> relative_map; };

```

从这个定义中可以看到，这个结构中包含了这些信息：

- 障碍物的预测信息

- 车辆底盘信息

- 大致定位信息

- 交通灯信息

- 导航路由信息

- 相对地图信息

对于每个Planner来说，其主要的逻辑都实现在`Plan`方法中。`PublicRoadPlanner::Plan`方法的实现逻辑如下：

`Status PublicRoadPlanner::Plan(const TrajectoryPoint& planning_start_point, Frame* frame) { DCHECK_NOTNULL(frame); scenario_manager_.Update(planning_start_point, *frame); ① scenario_ = scenario_manager_.mutable_scenario(); ② auto result = scenario_->Process(planning_start_point, frame); ③ ... if (result == scenario::Scenario::STATUS_DONE) { scenario_manager_.Update(planning_start_point, *frame); ④ } else if (result == scenario::Scenario::STATUS_UNKNOWN) { return Status(common::PLANNING_ERROR, "scenario returned unknown"); } return Status::OK(); } `

这段代码的几个关键步骤是：

1. 确定当前Scenario：因为Frame中包含了当前状态的所有信息，所以通过它就可以确定目前是处于哪一个场景下。

2. 获取当前Scenario。

3. 通过Scenario进行具体的处理。

4. 如果处理成功，则再次通过ScenarioManager更新。

Scenario是Apollo 3.5上新增的驾驶场景功能。前面在模块架构中我们已经提到过，接下来我们就详细看一下这方面内容。

# Scenario

## 场景分类

Apollo3.5聚焦在三个主要的驾驶场景，即：

### 车道保持

车道保持场景是默认的驾驶场景，它不仅仅包含单车道巡航。同时也包含了：

- 换道行驶

- 遵循基本的交通约定

- 基本转弯

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_8.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_8.png)

### Side Pass

在这种情况下，如果在自动驾驶车辆（ADC）的车道上有静态车辆或静态障碍物，并且车辆不能在不接触障碍物的情况下安全地通过车道，则执行以下策略：

- 检查邻近车道是否接近通行

- 如果无车辆，进行绕行，绕过当前车道进入邻道

- 一旦障碍物安全通过，回到原车道上

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_9.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_9.png)

### 停止标识

停止标识有两种分离的驾驶场景：

1、未保护：在这种情况下，汽车预计会通过具有双向停车位的十字路口。因此，我们的ADC必须爬过并测量十字路口的交通密度，然后才能继续走上它的道路。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_10.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_10.png)

2、受保护：在此场景中，汽车预期通过具有四向停车位的十字路口导航。我们的ADC将必须对在它之前停下来的汽车进行测量，并在移动之前了解它在队列中的位置。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_11.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_11.png)

## 场景实现

场景的实现主要包含三种类：

- `ScenarioManager`：场景管理器类。负责注册，选择和创建`Scenario`。

- `Scenario`：描述一个特定的场景（例如：Side Pass）。该类中包含了`CreateStage`方法用来创建`Stage`。一个Scenario可能有多个Stage对象。在Scenario中会根据配置顺序依次调用`Stage::Process`方法。该方法的返回值决定了从一个Stage切换到另外一个Stage。

- `Stage`：如上面所说，一个Scenario可能有多个Stage对象。场景功能实现的主体逻辑通常是在`Stage::Process`方法中。

## 场景配置

所有场景都是通过配置文件来进行配置的。很显然，首先需要在proto文件夹中定义其结构。

其内容如下所示：

```
// proto/planning_config.proto
message ScenarioConfig {

  message StageConfig { optional StageType stage_type = 1; optional bool enabled = 2 [default = true]; repeated TaskConfig.TaskType task_type = 3; repeated TaskConfig task_config = 4; } optional ScenarioType scenario_type = 1; oneof scenario_config { ScenarioLaneFollowConfig lane_follow_config = 2; ScenarioSidePassConfig side_pass_config = 3; ScenarioStopSignUnprotectedConfig stop_sign_unprotected_config = 4; ScenarioTrafficLightProtectedConfig traffic_light_protected_config = 5; ScenarioTrafficLightUnprotectedRightTurnConfig traffic_light_unprotected_right_turn_config = 6; } repeated StageType stage_type = 7; repeated StageConfig stage_config = 8; }

```

这里定义了ScenarioConfig结构，一个ScenarioConfig中可以包含多个StageConfig。

另外，Stage和Scenario都有一个Type字段，它们的定义如下：

```
enum ScenarioType {

    LANE_FOLLOW = 0; // default scenario CHANGE_LANE = 1; SIDE_PASS = 2; // go around an object when it blocks the road APPROACH = 3; // approach to an intersection STOP_SIGN_PROTECTED = 4; STOP_SIGN_UNPROTECTED = 5; TRAFFIC_LIGHT_PROTECTED = 6; TRAFFIC_LIGHT_UNPROTECTED_LEFT_TURN = 7; TRAFFIC_LIGHT_UNPROTECTED_RIGHT_TURN = 8; } enum StageType { NO_STAGE = 0; LANE_FOLLOW_DEFAULT_STAGE = 1; STOP_SIGN_UNPROTECTED_PRE_STOP = 100; STOP_SIGN_UNPROTECTED_STOP = 101; STOP_SIGN_UNPROTECTED_CREEP = 102 ; STOP_SIGN_UNPROTECTED_INTERSECTION_CRUISE = 103; SIDE_PASS_APPROACH_OBSTACLE = 200; SIDE_PASS_GENERATE_PATH= 201; SIDE_PASS_STOP_ON_WAITPOINT = 202; SIDE_PASS_DETECT_SAFETY = 203; SIDE_PASS_PASS_OBSTACLE = 204; SIDE_PASS_BACKUP = 205; TRAFFIC_LIGHT_PROTECTED_STOP = 300; TRAFFIC_LIGHT_PROTECTED_INTERSECTION_CRUISE = 301; TRAFFIC_LIGHT_UNPROTECTED_RIGHT_TURN_STOP = 310; TRAFFIC_LIGHT_UNPROTECTED_RIGHT_TURN_CREEP = 311 ; TRAFFIC_LIGHT_UNPROTECTED_RIGHT_TURN_INTERSECTION_CRUISE = 312; };

```

## 场景注册

前面我们已经提到，`ScenarioManager`负责场景的注册。实际上，注册的方式就是读取配置文件：

`void ScenarioManager::RegisterScenarios() { CHECK(Scenario::LoadConfig(FLAGS_scenario_lane_follow_config_file, &config_map_[ScenarioConfig::LANE_FOLLOW])); CHECK(Scenario::LoadConfig(FLAGS_scenario_side_pass_config_file, &config_map_[ScenarioConfig::SIDE_PASS])); CHECK(Scenario::LoadConfig( FLAGS_scenario_stop_sign_unprotected_config_file, &config_map_[ScenarioConfig::STOP_SIGN_UNPROTECTED])); CHECK(Scenario::LoadConfig( FLAGS_scenario_traffic_light_protected_config_file, &config_map_[ScenarioConfig::TRAFFIC_LIGHT_PROTECTED])); CHECK(Scenario::LoadConfig( FLAGS_scenario_traffic_light_unprotected_right_turn_config_file, &config_map_[ScenarioConfig::TRAFFIC_LIGHT_UNPROTECTED_RIGHT_TURN])); } `

配置文件在上文中已经全部列出。很显然，这里读取的配置文件位于`/modules/planning/conf/scenario`目录下。

## 场景确定

下面这个函数用来确定当前所处的场景。前面我们已经说了，确定场景的依据是`Frame`数据。

`void ScenarioManager::Update(const common::TrajectoryPoint& ego_point, const Frame& frame) { `

这里面的逻辑就不过多说明了，读者可以自行阅读相关代码。

## 场景配置

场景的配置文件都位于`/modules/planning/conf/scenario`目录下。在配置场景的时候，还会同时为场景配置相应的Task对象。关于这部分内容，在下文讲解Task的时候再一起看。

# Frenet坐标系

大家最熟悉的坐标系应该是横向和纵向垂直的笛卡尔坐标系。但是在自动驾驶领域，最常用的却是Frenet坐标系。基于Frenet坐标系的动作规划方法由于是由BMW的[Moritz Werling提出的](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fwww.researchgate.net%2Fprofile%2FMoritz_Werling%2Fpublication%2F224156269_Optimal_Trajectory_Generation_for_Dynamic_Street_Scenarios_in_a_Frenet_Frame%2Flinks%2F54f749df0cf210398e9277af%2FOptimal-Trajectory-Generation-for-Dynamic-Street-Scenarios-in-a-Frenet-Frame.pdf)。

之所以这么做，最主要的原因是因为大部分的道路都不是笔直的，而是具有一定弯曲度的弧线。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_12.jpg](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_12.jpg)

在Frenet坐标系中，我们使用道路的中心线作为参考线，使用参考线的切线向量t和法线向量n建立一个坐标系，如上图的右图所示，它以车辆自身为原点，坐标轴相互垂直，分为s方向（即沿着参考线的方向，通常被称为纵向，Longitudinal）和d方向（即参考线当前的法向，被称为横向，Lateral），相比于笛卡尔坐标系（上图的左图），Frenet坐标系明显地简化了问题。因为在公路行驶中，我们总是能够简单的找到道路的参考线（即道路的中心线），那么基于参考线的位置的表示就可以简单的使用纵向距离（即沿着道路方向的距离）和横向距离（即偏离参考线的距离）来描述。

下面这幅图描述了同样一个路段，分别在笛卡尔坐标系和Frenet坐标系下的描述结果。很显然，Frenet坐标系要更容易理解和处理。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_13.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_13.png)

# ReferenceLine

下面这幅图是Apollo公开课上对于Planning模块架构的描述。

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_14.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_14.png)

从这个图上我们可以看出，参考线是整个决策规划算法的基础。从前面的内容我们也看到了，在Planning模块的每个计算循环中，会先生成ReferencePath，然后在这个基础上进行后面的处理。例如：把障碍物投影到参考线上。

在下面的内容，我们把详细代码贴出来看一下。

## ReferenceLineProvider

ReferenceLine由ReferenceLineProvider专门负责生成。这个类的结构如下：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_15.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_15.png)

### 创建ReferenceLine

ReferenceLine是在`StdPlanning::InitFrame`函数中生成的，相关代码如下：

`Status StdPlanning::InitFrame(const uint32_t sequence_num, const TrajectoryPoint& planning_start_point, const double start_time, const VehicleState& vehicle_state, ADCTrajectory* output_trajectory) { frame_.reset(new Frame(sequence_num, local_view_, planning_start_point, start_time, vehicle_state, reference_line_provider_.get(), output_trajectory)); ... std::list<ReferenceLine> reference_lines; std::list<hdmap::RouteSegments> segments; if (!reference_line_provider_->GetReferenceLines(&reference_lines, &segments)) { `

## ReferenceLineInfo

在ReferenceLine之外，在common目录下还有一个结构：ReferenceLineInfo，这个结构才是各个模块实际用到数据结构，它其中包含了ReferenceLine，但还有其他更详细的数据。

从ReferenceLine到ReferenceLineInfo是在`Frame::CreateReferenceLineInfo`中完成的。

`bool Frame::CreateReferenceLineInfo( const std::list<ReferenceLine> &reference_lines, const std::list<hdmap::RouteSegments> &segments) { reference_line_info_.clear(); auto ref_line_iter = reference_lines.begin(); auto segments_iter = segments.begin(); while (ref_line_iter != reference_lines.end()) { if (segments_iter->StopForDestination()) { is_near_destination_ = true; } reference_line_info_.emplace_back(vehicle_state_, planning_start_point_, *ref_line_iter, *segments_iter); ++ref_line_iter; ++segments_iter; } ... `

ReferenceLineInfo不仅仅包含了参考线信息，还包含了车辆状态，路径信息，速度信息，决策信息以及轨迹信息等。Planning模块的算法很多都是基于ReferenceLineInfo结构完成的。例如下面这个：

`bool Stage::ExecuteTaskOnReferenceLine( const common::TrajectoryPoint& planning_start_point, Frame* frame) { for (auto& reference_line_info : *frame->mutable_reference_line_info()) { ... if (reference_line_info.speed_data().empty()) { *reference_line_info.mutable_speed_data() = SpeedProfileGenerator::GenerateFallbackSpeedProfile(); reference_line_info.AddCost(kSpeedOptimizationFallbackCost); reference_line_info.set_trajectory_type(ADCTrajectory::SPEED_FALLBACK); } else { reference_line_info.set_trajectory_type(ADCTrajectory::NORMAL); } DiscretizedTrajectory trajectory; if (!reference_line_info.CombinePathAndSpeedProfile( planning_start_point.relative_time(), planning_start_point.path_point().s(), &trajectory)) { AERROR << "Fail to aggregate planning trajectory."; return false; } reference_line_info.SetTrajectory(trajectory); reference_line_info.SetDrivable(true); return true; } return true; } `

## Smoother

为了保证车辆轨迹的平顺，参考线必须是经过平滑的，目前Apollo中包含了这么几个Smoother用来做参考线的平滑：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_16.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_16.png)

在实现中，Smoother用到了下面两个开源库：

- [Ipopt Project](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fprojects.coin-or.org%2FIpopt)

- [Eigen](https://www.oschina.net/action/GoToLink?url=http%3A%2F%2Feigen.tuxfamily.org%2Findex.php%3Ftitle%3DMain_Page)

# TrafficRule

行驶在城市道路上的自动驾驶车辆必定受到各种交通规则的限制。在正常情况下，车辆不应当违反交通规则。

另外，交通规则通常是多种条例，不同城市和国家地区的交通规则可能是不一样的。

如果处理好这些交通规则就是模块实现需要考虑的了。目前Planning模块的实现中，有如下这些交通规则的实现：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_17.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_17.png)

## TrafficRule配置

交通条例的生效并非是一成不变的，因此自然就需要有一个配置文件来进行配置。交通规则的配置文件是：`modules/planning/conf/traffic_rule_config.pb.txt`。

下面是其中的一个代码片段：

```
// modules/planning/conf/traffic_rule_config.pb.txt
...

config: { rule_id: SIGNAL_LIGHT enabled: true signal_light { stop_distance: 1.0 max_stop_deceleration: 6.0 min_pass_s_distance: 4.0 max_stop_deacceleration_yellow_light: 3.0 signal_expire_time_sec: 5.0 max_monitor_forward_distance: 135.0 righ_turn_creep { enabled: false min_boundary_t: 6.0 stop_distance: 0.5 speed_limit: 1.0 } } }

```

## TrafficDecider

TrafficDecider是交通规则处理的入口，它负责读取上面这个配置文件，并执行交通规则的检查。在上文中我们已经看到，交通规则的执行是在`StdPlanning::RunOnce`中完成的。具体执行的逻辑如下：

`Status TrafficDecider::Execute(Frame *frame, ReferenceLineInfo *reference_line_info) { for (const auto &rule_config : rule_configs_.config()) { if (!rule_config.enabled()) { ① continue; } auto rule = s_rule_factory.CreateObject(rule_config.rule_id(), rule_config); ② if (!rule) { continue; } rule->ApplyRule(frame, reference_line_info); ③ } BuildPlanningTarget(reference_line_info); ④ return Status::OK(); } `

这段代码说明如下：

1. 遍历配置文件中的每一条交通规则，判断是否enable。

2. 创建具体的交通规则对象。

3. 执行该条交通规则逻辑。

4. 在ReferenceLineInfo上合并处理所有交通规则最后的结果。

# Task

一直到目前最新的Apollo 3.5版本为止，Planning模块最核心的算法就是其EM Planner（实现类是`PublicRoadPlanner`），而EM Planner最核心的就是其决策器和优化器。

但由于篇幅所限，这部分内容本文不再继续深入。预计后面会再通过一篇文章来讲解。这里我们仅仅粗略的了解一下其实现结构。

Planning中这部分逻辑实现位于`tasks`目录下，无论是决策器还是优化器都是从`apollo::planning::Task`继承的。该类具有下面这些子类：

![解析百度Apollo之决策规划模块 - osc_rwo4fnf2的个人空间 - OSCHINA - _image_18.png](images/解析百度Apollo之决策规划模块%20-%20osc_rwo4fnf2的个人空间%20-%20OSCHINA%20-%20_image_18.png)

`Task`类提供了`Execute`方法供子类实现，实现依赖的数据结构就是`Frame`和`ReferenceLineInfo`。

`Status Task::Execute(Frame* frame, ReferenceLineInfo* reference_line_info) { frame_ = frame; reference_line_info_ = reference_line_info; return Status::OK(); } `

有兴趣的读者可以通过阅读子类的`Execute`方法来了解算法实现。

## Task配置

上文中我们已经提到，场景和Task配置是在一起的。这些配置在下面这些文件中：

```
// /modules/planning/conf/scenario
.

├── lane_follow_config.pb.txt ├── side_pass_config.pb.txt ├── stop_sign_unprotected_config.pb.txt ├── traffic_light_protected_config.pb.txt └── traffic_light_unprotected_right_turn_config.pb.txt

```

一个Scenario可能有多个Stage，每个Stage可以指定相应的Task，下面是一个配置示例：

```
// /modules/planning/conf/scenario/lane_follow_config.pb.txt

scenario_type: LANE_FOLLOW

stage_type: LANE_FOLLOW_DEFAULT_STAGE stage_config: { stage_type: LANE_FOLLOW_DEFAULT_STAGE enabled: true task_type: DECIDER_RULE_BASED_STOP task_type: DP_POLY_PATH_OPTIMIZER ... task_config: { task_type: DECIDER_RULE_BASED_STOP } task_config: { task_type: QP_PIECEWISE_JERK_PATH_OPTIMIZER } task_config: { task_type: DP_POLY_PATH_OPTIMIZER } ... }

```

这里的`task_type`与Task实现类是一一对应的。

## Task读取

在构造Stage对象的时候，会读取这里的配置文件，然后创建相应的Task：

`Stage::Stage(const ScenarioConfig::StageConfig& config) : config_(config) { name_ = ScenarioConfig::StageType_Name(config_.stage_type()); next_stage_ = config_.stage_type(); std::unordered_map<TaskConfig::TaskType, const TaskConfig*, std::hash<int>> config_map; for (const auto& task_config : config_.task_config()) { config_map[task_config.task_type()] = &task_config; } for (int i = 0; i < config_.task_type_size(); ++i) { auto task_type = config_.task_type(i); CHECK(config_map.find(task_type) != config_map.end()) << "Task: " << TaskConfig::TaskType_Name(task_type) << " used but not configured"; auto ptr = TaskFactory::CreateTask(*config_map[task_type]); task_list_.push_back(ptr.get()); tasks_[task_type] = std::move(ptr); } } `

## Task执行

Task的执行是在`Stage::Process`中，通过`ExecuteTaskOnReferenceLine`完成的。

>

> 如果你已经搞不清楚这个逻辑关系，请回到> [> 上文](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fapollo-planning%2F%23id-planningcycle)> 看一下

>

`bool Stage::ExecuteTaskOnReferenceLine( const common::TrajectoryPoint& planning_start_point, Frame* frame) { for (auto& reference_line_info : *frame->mutable_reference_line_info()) { ... auto ret = common::Status::OK(); for (auto* task : task_list_) { ret = task->Execute(frame, &reference_line_info); if (!ret.ok()) { AERROR << "Failed to run tasks[" << task->Name() << "], Error message: " << ret.error_message(); break; } } ... } return true; } `

# 参考资料与推荐读物

- [ApolloAuto/apollo](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo)

- [apollo/modules/planning README](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Ftree%2Fmaster%2Fmodules%2Fplanning)

- [Class Architecture and Overview – Planning Module](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Fblob%2Fmaster%2Fdocs%2Fspecs%2FClass_Architecture_Planning.md)

- [A Survey of Motion Planning and Control Techniques for Self-driving Urban Vehicles](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Farxiv.org%2Fabs%2F1604.07446)

- [Optimal Trajectory Generation for Dynamic Street Scenarios in a Frene ́t Frame](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fwww.researchgate.net%2Fprofile%2FMoritz_Werling%2Fpublication%2F224156269_Optimal_Trajectory_Generation_for_Dynamic_Street_Scenarios_in_a_Frenet_Frame%2Flinks%2F54f749df0cf210398e9277af%2FOptimal-Trajectory-Generation-for-Dynamic-Street-Scenarios-in-a-Frenet-Frame.pdf)

- [无人驾驶汽车系统入门——基于Frenet优化轨迹的无人车动作规划方法](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fcloud.tencent.com%2Fdeveloper%2Farticle%2F1162338)

- [Apollo开发者社区 - Lattice Planner规划算法](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FYDIoVf20kybu8JEUY3GZWg)

- [Apollo 2.5版导航模式的使用方法](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Fblob%2Fmaster%2Fdocs%2Fhowto%2Fhow_to_use_apollo_2.5_navigation_mode_cn.md)

- [Apollo 规划技术详解](https://www.oschina.net/action/GoToLink?url=http%3A%2F%2Fbit.baidu.com%2FCourse%2Fdetail%2Fid%2F294.html)

- [Apollo 无人驾驶免费入门课程](https://www.oschina.net/action/GoToLink?url=http%3A%2F%2Fapollo.auto%2Fdevcenter%2Fdevcenter_cn.html)

- [Apollo 3.0 Software Architecture](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%2FApolloAuto%2Fapollo%2Fblob%2Fmaster%2Fdocs%2Fspecs%2FApollo_3.0_Software_Architecture.md)

原文地址：[《解析百度Apollo之决策规划模块》](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub%2Fapollo-planning%2F) by [保罗的酒吧](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fpaul.pub)