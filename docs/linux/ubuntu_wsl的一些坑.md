## wsl中由于权限问题，之前修改了usr目录的权限，导致sudo不能用了，解决方案如下
在
windows power shell中
```bash
ubuntu1804.exe config --default-user root

chown root:root /usr/bin/sudo
chmod 4755 /usr/bin/sudo
chown root /usr/lib/sudo/sudoers.so

ubuntu1804.exe config --default-user hanshuo
```

## windows安装wsl2的教程
https://zhuanlan.zhihu.com/p/356397851

[如何在windows 11中安装WSLG(WSL2) - guojikun - 博客园 (cnblogs.com)](https://www.cnblogs.com/guojikun/p/15092696.html)

[wsl2可视化](https://zhuanlan.zhihu.com/p/137618871)

```bash
export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
```

## wsl2访问外网：
1.首先保证wsl能访问本机ip
https://blog.csdn.net/nick_young_qu/article/details/113709768
![](images/ubuntu%20wsl的一些坑_image_1.png)
2.调整socket代理
![](images/ubuntu%20wsl的一些坑_image_2.png)

```
host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
#export ALL_PROXY="http://$host_ip:10810"
export http_proxy=socks5://$host_ip:10810
export https_proxy=socks5://$host_ip:10810
```
如果遇到了socket报错，可以暂时先取消代理
```
unset http_proxy
unset https_proxy
```

## cmake升级：
https://blog.csdn.net/qingkong8978/article/details/106432949