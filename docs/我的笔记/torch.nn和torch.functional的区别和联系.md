
### 1. 以relu为例

relu 函数在 pytorch 中总共有 3 次出现：

1. `torch.nn.ReLU()`
2. `torch.nn.functional.relu_()` `torch.nn.functional.relu_()`
3. `torch.relu()` `torch.relu_()`

而这3种不同的实现其实是有固定的包装关系，**由上至下是由表及里的过程**。其中最后一个实际上并不被 pytorch 的官方文档包含，同时也找不到对应的 python 代码，只是在 `__init__.pyi` 中存在，因为他们来自于通过C++编写的THNN库。

#### 1.1 torch.nn.ReLU()
torch.nn 中的类代表的是神经网络层，这里我们看到作为类出现的 **`ReLU()` 实际上只是调用了 `torch.nn.functional` 中的 `relu 和relu_` 实现。**

#### 1.2 `torch.nn.functional.relu()`和 **`torch.nn.functional.relu_()`**

其实这两个函数也是调用了 `torch.relu() and torch.relu_()`

#### 1.3 

结合上述对 `relu` 的分析，我们能够更清晰的认识到两个库之间的联系。：`torch.nn.relu`调用`torch.nn.functional.relu`，`torch.nn.functional.relu`d调用torch.relu

通常来说 `torch.nn.functional` 调用了 THNN库，实现核心计算，但是不对 `learnable_parameters` 例如 `weight` `bias` ，进行管理，为模型的使用带来不便。而 `torch.nn` 中实现的模型则对 `torch.nn.functional`，本质上是官方给出的对 `torch.nn.functional`的使用范例，我们通过直接调用这些范例能够快速方便的使用 `pytorch` ，但是范例可能不能够照顾到所有人的使用需求，**因此保留 `torch.nn.functional` 来为这些用户提供灵活性**，他们可以自己组装需要的模型。因此 `pytorch` 能够在灵活性与易用性上取得平衡。

特别注意的是，**`torch.nn`不全都是对`torch.nn.functional`的范例，有一些调用了来自其他库的函数，例如常用的`RNN`型神经网络族即没有在`torch.nn.functional`中出现**。