主要使用的工具为netplan
tips：
重启网络：
sudo service network-manager restart 或者 sudo systemctl restart NetworkManager.service

最简单的方法：
在root用户下使用
nmtui命令（不在root用户下会配置失败）

如果没有要sudo apt install network-manager（ubuntu是预装的）

如果实在不行（没有nmtui可用）：那么需要按以下步骤

#### 步骤 1：确定你的无线网络接口名称

ifconfg -a
一般wifi会议wlan0的名称出现

#### 步骤 2：编辑 Netplan 配置文件中的 wifi 接口详细信息

Netplan 配置文件在 `/etc/netplan` 目录下。如果你查看这个目录的内容，你应该看到类似 `01-network-manager-all.yml` 或 `50-cloud-init.yaml` 等文件。
![[Pasted image 20230518152733.png]]
如果是 Ubuntu 服务器，你应该有 `50-cloud-init.yaml` 文件。如果是桌面计算机，应该是 `01-network-manager-all.yml` 文件。

Linux 桌面计算机的 Network Manager 允许你选择一个无线网络。你可以在它的配置中硬编码写入 WiFi 接入点。这可以在自动掉线的情况下（比如挂起）时帮助到你。

不管是哪个文件，都可以打开编辑。我希望你对 Nano 编辑器有一点[熟悉](https://itsfoss.com/nano-editor-guide/)，因为 Ubuntu 预装了它。

编辑这个文件
![[Pasted image 20230518153020.png]]
1.  `wifis:`
2.  `wlan0:`
3.  `dhcp4: true`
4.  `optional: true`
5.  `access-points:`
6.  `"SSID_name":`
7.  `password: "WiFi_password"`

![[Pasted image 20230518153039.png]]


#### 步骤 3：使配置生效

1.  `sudo netplan generate`
2.  `sudo netplan apply`

如果报错：
![[Pasted image 20230518153140.png]]

否则重启

