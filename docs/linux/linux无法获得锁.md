## 方案一

> 强制解锁（比较暴力）

```shell
sudo rm /var/lib/apt/lists/lock
```

## 方案二

> ps aux 列出当前进程列表 找到 apt-get 那个被lock住的进程记下PID，sudo kill PID 即可，因为linux只允许开一个apt-get，当然apt-get和新立得也是只能同时开一个，具体命令如下：

```
1.列出当前进程列表 
ps -aux | grep apt-get
 
2.找到最后一列以apt-get 开头的进程,然后sudo kill PID
sudo kill PID

```

