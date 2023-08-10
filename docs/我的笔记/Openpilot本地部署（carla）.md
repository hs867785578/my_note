参考[官方文档](https://github.com/commaai/openpilot/tree/master/tools/sim#openpilot-in-simulator)

# 1 测试系统

## 软件
- ubuntu 20.04
- docker 24.0
- carla 0.9.13
## 硬件
- i7-10700
- nvidia rtx2060
- 500G固态

# 2步骤

1. openpilot clone到本地
```bash
git clone https://github.com/commaai/openpilot.git --depth=1
git submodule update --init --recursive
```

2. 安装并启动carla0.9.13镜像（可选，可以用本地的0.9.13）
```bash
进入到openpilot目录下的your_openpilot_path/tools/sim
cd your_openpilot_path/tools/sim
./start_carla.sh（用该脚本启动的carla是不显示界面的，可以用nvidia-smi查看显存判断是否启动了calra）
```

3. 安装并启动openpilot镜像，进入到openpilot目录下的your_openpilot_path/tools/sim（也可以用本地的openpilot，启动./launch_openpilot.sh和./bridge.py）
```bash
cd your_openpilot_path/tools/sim
./start_openpilot_docker.sh（相当于在docker中启动了./launch_openpilot.sh和./bridge.py）
```

4. 此时会显示openpilot的界面，按2是开启openpilot，按1是增加巡航速度，具体可以看官方文档。

