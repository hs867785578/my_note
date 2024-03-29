 Carla使用过程中的一些注意点

## 1.ros_manual_control一片黑的原因可是是ros的cv bradge的问题

https://blog.csdn.net/qq_38606680/article/details/129118491
![](images/carla笔记_image_1.png)

## 2 carla安装后在zshrc或者bashrc中设置如下变量
```bash
#miniconda
PATH=$PATH:/home/thicv/miniconda3/bin
export CARLA_ROOT=/home/thicv/carla/CARLA_0.9.13 
export SCENARIO_RUNNER_ROOT=/home/thicv/scenario_runner-0.9.13
PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla
PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla/agents
```

## 3 carla的pygame的manual_control如果还是黑屏，可能是因为objects.json中没有配置好传感器

最少需要下列三个传感器，pygame才会有显示画面
![](images/carla笔记_image_2.png)

## 4 carla_ros_bridge的同步模式有两个

synchronous mode是carla中client和server的同步。
synchronous mode wait for control command是需要有一个控制指令传过来，carla才会更新一步。这个更精确

![](images/carla笔记_image_3.png)

## 5 carla源码编译

环境：ubuntu20.04、UE4（carla版本）、carla0.9.14

注意：UE4一定要用carla特供版本，不然make launch时会报错（在0.9.12的carla都使用carla特供版的UE4）

如果clang找不到头文件，需要装一下libstdc++-13-dev。
其他的按照官网一步一步走就行。


## 强化学习carla环境

### 1.carla-gym遇到pygame卡死的情况，在carla_env.py的531行

```python
    pygame.display.flip()
    for enent in pygame.event.get():
      if enent.type == pygame.QUIT:
        pygame.quit()
```

### 不用carla-gym环境

https://github.com/hs867785578/CARLA-SB3-RL-Training-Environment

参数设置
activate_spectator=True

```
        if start_carla:
            # if "CARLA_ROOT" not in os.environ:
            #     raise Exception("${CARLA_ROOT} has not been set!")
            carla_path = r"D:\\carla\\CARLA_0.9.13\\WindowsNoEditor\\CarlaUE4.exe"
            launch_command = [carla_path]
            launch_command += ['-quality_level=Low']
            launch_command += ['-benchmark']
            launch_command += ["-fps=%i" % fps]
            launch_command += ['-RenderOffScreen']
            launch_command += ['-prefernvidia']
            launch_command += [f'-carla-world-port={port}']
            print("Running command:")
            print(" ".join(launch_command))
            self.carla_process = subprocess.Popen(launch_command, stdout=subprocess.DEVNULL, stderr=subprocess.STDOUT)
            print("Waiting for CARLA to initialize\n")

            # ./CarlaUE4.sh -quality_level=Low -benchmark -fps=15 -RenderOffScreen
            time.sleep(5)
```