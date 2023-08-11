


```python
import torch
from torch import nn
from torch.nn import functional as F
from torch.utils.data import DataLoader
from torch.utils.data import random_split
from torchvision.datasets import MNIST
from torchvision import transforms
import pytorch_lightning as pl

class LitAutoEncoder(pl.LightningModule):
	def __init__(self):
		super().__init__()
		self.encoder = nn.Sequential(
      nn.Linear(28 * 28, 64),
      nn.ReLU(),
      nn.Linear(64, 3))
		self.decoder = nn.Sequential(
      nn.Linear(3, 64),
      nn.ReLU(),
      nn.Linear(64, 28 * 28))

	def forward(self, x):
		embedding = self.encoder(x)
		return embedding

#配置优化器
	def configure_optimizers(self):
		optimizer = torch.optim.Adam(self.parameters(), lr=1e-3)
		return optimizer
#配置训练过程
	def training_step(self, train_batch, batch_idx):
		x, y = train_batch
		x = x.view(x.size(0), -1)
		z = self.encoder(x)    
		x_hat = self.decoder(z)
		loss = F.mse_loss(x_hat, x)
		self.log('train_loss', loss)
		return loss
#配置验证过程
	def validation_step(self, val_batch, batch_idx):
		x, y = val_batch
		x = x.view(x.size(0), -1)
		z = self.encoder(x)
		x_hat = self.decoder(z)
		loss = F.mse_loss(x_hat, x)
		self.log('val_loss', loss)

# data
dataset = MNIST('', train=True, download=True, transform=transforms.ToTensor())
mnist_train, mnist_val = random_split(dataset, [55000, 5000])

train_loader = DataLoader(mnist_train, batch_size=32)
val_loader = DataLoader(mnist_val, batch_size=32)

# model
model = LitAutoEncoder()

# training
trainer = pl.Trainer(gpus=4, num_nodes=8, precision=16, limit_train_batches=0.5)
trainer.fit(model, train_loader, val_loader)
```

pytorch lighting的优点：

- 简化了部分代码，之前如果要转到GPU上，需要用to(device)方法判断，然后转过去。有了PyTorch lightning的帮助，可以自动帮你处理，通过设置trainer中的gpus参数即可。
- 提供了一些有用的工具，比如混合精度训练、分布式训练、Horovod
- 代码移植更加容易
- 更容易实现分布式训练，只需要通过修改 `pl.Trainer()`中的参数即可将单机单卡变成多机多卡的训练方式。具体的通过修改参数`gpus`、`num_nodes`设置训练需要多少张GPU和所使用机器的数量，同时通过参数`strategy`指定分布式训练的模式。

**单机多卡**. 单机多卡时无需指定参数`num_nodes`:

```python
trainer = pl.Trainer(gpus=4, strategy="dp") # 使用4块GPU
trainer = pl.Trainer(gpus=[0, 1, 2], strategy="dp") # 使用0，1，2号3块GPu
```

**多机多卡**

```python
trainer = pl.Trainer(gpus=4, num_nodes=3, strategy="ddp") # 使用3台机器，每个机器4块GPU，总共12张GPU
```

另外PL不仅支持常见的`dp`、`ddp`、`deepspeed`等，甚至还可以通过`DDPStrategy()`自定义`strategy`，有更高级的需求可以查阅官方文档：
https://link.zhihu.com/?target=https%3A//pytorch-lightning.readthedocs.io/en/latest/accelerators/gpu_expert.html
![](images/学会使用pytorch%20lighting_image_1.png)
 
<img src="images/学会使用pytorch%20lighting_image_1.png" width = "80%" height = "" align="center" />

![](images/学会使用pytorch%20lighting_image_2.png)