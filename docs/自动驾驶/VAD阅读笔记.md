
## 1 模型构建

首先在train.py的main函数中构建
- model：class VAD
- dataset：class VADCustomNuScenesDataset
![](images/VAD阅读笔记_image_1.png)

进入custom_train_model，然后构建detector
![](images/VAD阅读笔记_image_2.png)

在构建train_detector函数中，会构造runner，并在把一些必要的hook注册到runner里去，比如optimizer，lr等。

![](images/VAD阅读笔记_image_3.png)

然后会执行runner.run函数，开始训练，在run函数中会调用train函数，输入为dataloader（注意，dataloader可能有多个，比如train的dataloader和test的dataloader）

![](images/VAD阅读笔记_image_4.png)

在run函数中，调用train函数，从而进入run_iter函数（注意，在run_iter函数执行完后会进入到调用所有的hook，如果hook中有after_train_iter这个函数，就会执行，比如optimizer的hook中有after_train_iter这个函数，会调用loss.backward()）

![](images/VAD阅读笔记_image_5.png)

在run_iter函数中会调用model的train_step函数（注意，这个model不是config文件中的model，而是包装了一层MMDataParallel，他是在custom_train_detector中构建的，所以首先会执行MMDataParallel的train_step函数，会把data中的container包装给去掉，只剩下原始的data，然后再调用真正模型的train_step函数）

![](images/VAD阅读笔记_image_6.png)
![](images/VAD阅读笔记_image_7.png)

![](images/VAD阅读笔记_image_8.png)

![](images/VAD阅读笔记_image_9.png)

在模型的train_step函数中，会把data解包，并调用实例的forward函数，即self(\*\*data)。
![](images/VAD阅读笔记_image_10.png)

data中imag的维度是，B，sequence_length,cam_num,通道数，高，宽
![](images/VAD阅读笔记_image_11.png)



## 2 VAD前向传播（backbone和neck）

在模型的forward中会通过backbone和neck提取特征，如果_num_levels_=4，则会提取四个尺度的特征。这四个特征中，有三个是取的resnet50的2，3，4层（conv3_x,conv4_x,conv5_x）的输出，有一层是在fpn中通过maxpooling增加的。

![](images/VAD阅读笔记_image_12.png)

![](images/VAD阅读笔记_image_13.png)

**restnet的输出：**

下采样8倍，第一个维度是B和sequence_length相乘得到的
![](images/VAD阅读笔记_image_14.png)

下采样16倍
![](images/VAD阅读笔记_image_15.png)

下采样32倍
![](images/VAD阅读笔记_image_16.png)


**fpn的输出：**
首先fpn会将resnet的所有特征映射到256个通道，然后下一层的特征上采样，并加到下一层，也就是说 下采样32倍的特征会上采样2倍 然后加到 下采样16倍 的特征上。继续上采样2倍，加到下采样8倍的特征图上。

此外，fpn中根据下采样32倍的特征，继续下采样2倍，得到第四个特征图。

![](images/VAD阅读笔记_image_17.png)

其他三个特征图的维度为：
![](images/VAD阅读笔记_image_18.png)

![](images/VAD阅读笔记_image_19.png)

![](images/VAD阅读笔记_image_20.png)

所有特征图均存在一个list中

![](images/VAD阅读笔记_image_21.png)


提取完特征后会进入forward_pts_train函数中

![](images/VAD阅读笔记_image_22.png)
![](images/VAD阅读笔记_image_23.png)


## 3 VADHead 前向传播

### 3.1 transformer

#### 3.1.1 encoder得到bev特征
接着会调用vad_head，首先进入transformer，得到bev特征，检测头隐藏状态，检测头初始位置，检测头最终位置，map隐藏状态，map初始位置，map最终位置：

这些会当作下边交互的输入。
![](images/VAD阅读笔记_image_24.png)

在transformer的输入中，有三个参数比较重要，mlvl_feats，bev_querier，pre_bev
![](images/VAD阅读笔记_image_25.png)

![](images/VAD阅读笔记_image_26.png)

![](images/VAD阅读笔记_image_27.png)

在transformer的forward函数中，首先会get_bev_features，在这里mlvl_feats会被concat成feat_flatten，并且记录好之前的size

![](images/VAD阅读笔记_image_28.png)
![](images/VAD阅读笔记_image_29.png)

然后会被送入encoder(bevformer)：
![](images/VAD阅读笔记_image_30.png)

在bevformer的encoder中，首先会得到2d和3d的参考点，其中2d的参考点用于**TemporalSelfAttention中的可变形注意力**（bev_queries与pre_bev做attention），3d参考点用于**SpatialCrossAttention的可变形注意力**。

self.point_sampling用于将3d的参考点通过矩阵变换投影到6个相机中，并通过投影后归一化的uv值判断是否应该被mask（超过1说明超出图像，应该被mask）
![](images/VAD阅读笔记_image_31.png)


![](images/VAD阅读笔记_image_32.png)

注意在encoder中，需要做TemporalSelfAttention（q是bev_queries，kv是pre_bev）
![](images/VAD阅读笔记_image_33.png)

SpatialCrossAttention（q是bev_queries，kv是feat_flatten）
![](images/VAD阅读笔记_image_34.png)


经过encoder之后得到bev_embed

![](images/VAD阅读笔记_image_35.png)


#### 3.1.2 decoder 输出目标检测和map检测

然后会进入decoder阶段（query为object_query，kv为bev_embed），需要注意的是，在decoder中，self attention是传统的attention，而cross attention是可变形注意力。而在encoder中，self attention（TSA）和cross attention（SCA）都是可变形注意力，

![](images/VAD阅读笔记_image_36.png)

![](images/VAD阅读笔记_image_37.png)

decoder得到的是3d目标检测的结果和最后隐藏层的特征，inter_states, inter_references

![](images/VAD阅读笔记_image_38.png)
![](images/VAD阅读笔记_image_39.png)

同样，VAD还会进行在线建图，即对map进行检测（query为map_query，kv为bev_embed）：

![](images/VAD阅读笔记_image_40.png)

最后将3d目标检测和map检测的结果都返回
![](images/VAD阅读笔记_image_41.png)

![](images/VAD阅读笔记_image_42.png)

### 3.2 motion_decoder(CustomTransformerDecoder)

通过transformer 输出的目标检测隐藏层特征当作motion_query，做motion_decoder得到占该物的特征。
![](images/VAD阅读笔记_image_43.png)


### 3.3 motion_map_decoder（CustomTransformerDecoder）

通过transformer 输出的map 检测隐藏层特征当作map_query，作为kv，之前3.2得到的motion_hs当作q，做cross attention。

![](images/VAD阅读笔记_image_44.png)


### 3.4 ego_agent_decoder（CustomTransformerDecoder）

首先将3.3的ca_motion_query和3.2的motion_hs cat到一起，得到新的motion_hs

![](images/VAD阅读笔记_image_45.png)

对自车历史里轨迹编码，得到ego_query，作为q，motion_hs也就是agent_query作为key，做交互。
![](images/VAD阅读笔记_image_46.png)


### 3.5 ego_map_decoder

3.4得到的ego_agent作为q，3.1得到的map_hs作为ky做交互
![](images/VAD阅读笔记_image_47.png)


### 3.6 自车规划

将3.5得到的ego_mao_query与3.4得到的ego_agent_query拼起来，然后过一个ego_fut_decoder（Liner，relu，Liner），得到自车轨迹
![](images/VAD阅读笔记_image_48.png)

最后返回所有输出

![](images/VAD阅读笔记_image_49.png)

## 4 计算loss

得到3.6的输出，并返回后，结合gt，计算loss。
![](images/VAD阅读笔记_image_50.png)

最后VAD模型的forward会返回loss