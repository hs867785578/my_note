
# 源码安装(目前源码安装不支持场景同步)

更新时间：2022-11-30

本手册旨在帮助用户重新安装 **Apollo 8.0** 系统环境及 **Apollo 8.0** 源码 运行软件。

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%E6%AD%A5%E9%AA%A4%E4%B8%80%EF%BC%9A%E5%AE%89%E8%A3%85-linux-%E7%B3%BB%E7%BB%9F)步骤一：安装 Linux 系统

Apollo 软件系统依赖于 Linux 操作系统运行，而 Linux 操作系统种类繁多，且又分为服务器版本和桌面版本，这里我们选择当下比较流行的 Ubuntu 桌面操作系统的 64 位版本。安装 Ubuntu 18.04+ 的步骤，参见 [官方安装指南](https://ubuntu.com/tutorials/install-ubuntu-desktop)。

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89%E6%AD%A5%E9%AA%A4%E4%BA%8C%EF%BC%9A%E5%AE%89%E8%A3%85-nvidia-gpu-%E9%A9%B1%E5%8A%A8)（可选）步骤二：安装 NVIDIA GPU 驱动

**Apollo 8.0** 的一些模块的编译和运行需要依赖 NVIDIA GPU 环境（例如感知模块），如果您有编译和运行这类模块的需求，则需要安装 NVIDIA GPU 驱动。

您可以通过以下两种方式在 Ubuntu 上进行安装：

- (推荐) apt-get 命令，参见 [How to Install NVIDIA Driver](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#how-do-i-install-the-nvidia-driver)。
- 使用官方 [runfile](https://www.nvidia.com/en-us/drivers/unix/)。

对于 Ubuntu 18.04+，只需执行以下命令即可：

```bash
sudo apt-get update
sudo apt-add-repository multiverse
sudo apt-get update
sudo apt-get install nvidia-driver-455
```

安装完毕后，可以输入 `nvidia-smi`来校验 NVIDIA GPU 驱动是否在正常运行（可能需要在安装后重启系统以使驱动生效）。如果成功，则会出现以下信息：

```text
Prompt> nvidia-smi
Mon Jan 25 15:51:08 2021
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.27.04    Driver Version: 460.27.04    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|=++|
|   0  GeForce RTX 3090    On   | 00000000:65:00.0  On |                  N/A |
| 32%   29C    P8    18W / 350W |    682MiB / 24234MiB |      7%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=|
|    0   N/A  N/A      1286      G   /usr/lib/xorg/Xorg                 40MiB |
|    0   N/A  N/A      1517      G   /usr/bin/gnome-shell              120MiB |
|    0   N/A  N/A      1899      G   /usr/lib/xorg/Xorg                342MiB |
|    0   N/A  N/A      2037      G   /usr/bin/gnome-shell               69MiB |
|    0   N/A  N/A      4148      G   ...gAAAAAAAAA --shared-files      105MiB |
+-----------------------------------------------------------------------------+
```

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%E6%AD%A5%E9%AA%A4%E4%B8%89%EF%BC%9A%E5%AE%89%E8%A3%85-docker)步骤三：安装 docker

Apollo 8.0 依赖于 Docker 19.03+。要安装 Docker，参见 [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)。

Ubuntu 上的 Docker-CE 也可以通过 Docker 提供的官方脚本安装：

```bash
curl https://get.docker.com | sh
sudo systemctl start docker && sudo systemctl enable docker
```

您可以自由选择安装方式，安装之后，不要忘记执行 [Linux 上的后续操作说明](https://docs.docker.com/engine/install/linux-postinstall/)。更多内容，参见 [使用非 root 权限运行 docker](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) 和 [配置开机启动 docker](https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot)。

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89%E6%AD%A5%E9%AA%A4%E5%9B%9B%EF%BC%9A%E5%AE%89%E8%A3%85-nvidia-container-toolkit)（可选）步骤四：安装 NVIDIA Container Toolkit

为了在容器内获得 GPU 支持，在安装完 docker 后需要安装 NVIDIA Container Toolkit。 运行以下命令安装 NVIDIA Container Toolkit：

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get -y update
sudo apt-get install -y nvidia-docker2
```

安装完成后，重启 Docker 以使改动生效。

```bash
sudo systemctl restart docker
```

安装完毕后，可以在**APOLLO容器内**输入`nvidia-smi`来校验 NVIDIA GPU 在容器内是否能正常运行（详见步骤五）。

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%E6%AD%A5%E9%AA%A4%E4%BA%94%EF%BC%9A%E4%B8%8B%E8%BD%BD%E5%B9%B6%E7%BC%96%E8%AF%91-apollo-%E6%BA%90%E7%A0%81)步骤五：下载并编译 Apollo 源码

1. 安装 git 并将源码 clone 下来：
    
    ```bash
    cd ~/
    sudo apt update
    sudo apt install git -y
    git init
    git clone https://github.com/ApolloAuto/apollo.git
    ```
    
    代码下载的时间视网速的快慢而有所区别，请耐心等待。
    
2. 启动并进入 docker 容器，在终端输入以下命令：
    
    ```bash
     cd ~/apollo
     bash docker/scripts/dev_start.sh
    ```
    
    第一次进入 docker 时或者 image 镜像有更新时会自动下载 apollo 所需的 image 镜像文件，下载镜像文件的过程会很长，请耐心等待。
    
    如果一切正常，则会见到以下信息：
    
    ```bash
    [ OK ] Congratulations! You have successfully finished setting up Apollo Dev Environment.
    [ OK ] To login into the newly created apollo_neo_dev_root container, please run the following command:
    [ OK ]   bash scripts/edu_launcher.sh enter
    [ OK ] Enjoy!
    ```
    
    这个过程完成后，请输入以下命令以进入 docker 环境中：
    
    ```bash
    bash docker/scripts/dev_into.sh
    ```
    
    如果您在步骤二和步骤四分别安装了 NVIDIA GPU 驱动和 NVIDIA Container Toolkit，您可以输入`nvidia-smi`来校验 NVIDIA GPU 在容器内是否能正常运行，如果成功，则会出现以下信息:
    
    ```text
    root@in-dev-docker:/apollo_workspace# nvidia-smi 
    Wed Sep 14 11:43:13 2022       
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 460.32.03    Driver Version: 460.32.03    CUDA Version: 11.2     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |=++|
    |   0  Tesla V100-SXM2...  Off  | 00000000:03:00.0 Off |                    0 |
    | N/A   31C    P0    38W / 300W |    153MiB / 32510MiB |      0%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=|
    |    0   N/A  N/A      9962      C   nvidia-cuda-mps-server             29MiB |
    +-----------------------------------------------------------------------------+
    ```
    
3. 编译 Apollo 源码。
    
    编译 Apollo，在终端输入以下命令，等待编译完成，编译过程耗时视机器配置的不同而有所区别，请耐心等待：
    
    ```bash
    bash apollo.sh build
    ```
    

## [](https://apollo.baidu.com/Apollo-Homepage-Document/Apollo_Doc_CN_8_0/%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E/%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85#%E6%AD%A5%E9%AA%A4%E5%85%AD%EF%BC%9A%E8%BF%90%E8%A1%8C-dreamview-%E6%A3%80%E9%AA%8C%E7%BC%96%E8%AF%91%E6%98%AF%E5%90%A6%E6%88%90%E5%8A%9F)步骤六：运行 Dreamview 检验编译是否成功

1. 进入 Apollo 容器环境。
    
    ```bash
     cd ~/apollo
     bash docker/scripts/dev_start.sh
     bash docker/scripts/dev_into.sh
    ```
    
    > 注：如果您已在容器环境内，请忽略此步骤。
    
2. 启动 dreamview。
    
    在终端输入以下命令：
    
    ```bash
    bash scripts/bootstrap.sh
    ```
    
    如果启动成功，在终端会输出以下信息：
    
    ```bash
     nohup: appending output to 'nohup.out'
     Launched module monitor.
     nohup: appending output to 'nohup.out'
     Launched module dreamview.
     Dreamview is running at http://localhost:8888
    ```
    
    在浏览器中输入以下地址访问 Dreamview：
    
    ```text
    http://localhost:8888
    ```
    
3. 回放数据包。
    
    在终端输入以下命令下载数据包：
    
    ```bash
    wget https://apollo-system.cdn.bcebos.com/dataset/6.0_edu/demo_3.5.record
    ```
    
    输入以下命令可以回放数据包，在浏览器 DreamView 中应该可以看到回放画面：
    
    ```bash
    cyber_recorder play -f demo_3.5.record --loop
    ```