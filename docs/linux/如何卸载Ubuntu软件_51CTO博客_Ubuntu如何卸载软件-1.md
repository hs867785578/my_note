[ignore-error,1](../_resources/ignore-error,1-2)

# 如何卸载Ubuntu软件

 转载

 [mb5fe191195f1f1](https://blog.51cto.com/u_15064656)  2019-05-15 17:05:00

 **  *文章标签*  [包管理器](https://blog.51cto.com/search/result?q=%E5%8C%85%E7%AE%A1%E7%90%86%E5%99%A8)  [ubuntu](https://blog.51cto.com/topic/ubuntu-2.html)  [搜索](https://blog.51cto.com/topic/sousuo.html)  [配置文件](https://blog.51cto.com/search/result?q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)  [应用程序](https://blog.51cto.com/search/result?q=%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)  **  **  *文章分类*  [IT业界](https://blog.51cto.com/nav/industry)  [其它](https://blog.51cto.com/nav/other)  **  ***阅读数***171****

### 使用Synaptic软件包管理器进行卸载

1. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-1)
1

**打开软件包管理器。**Ubuntu自带了一个GUI（Graphical User Interface，图形化用户界面）软件包管理器，它可以让你在一个可视化窗口中卸载程序。如果你不习惯使用命令行，这一工具将非常有用。

    - 点击系统，然后选择管理。在管理菜单中，选择Synaptic软件包管理器。
    - 某些较新版本的Ubuntu没有预装Synaptic。要安装它，打开终端并输入：

sudo apt-get install synaptic

    - 如果你使用Unity，可以打开dashboard并搜索“Synaptic”

2. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-9)
2
**找出你希望卸载的程序。**在左边的窗格中，你可以按照类别对程序进行排序列表。已安装的程序（软件包）将在列表显示在Synaptic上方的窗格内。

    - 程序经常以它们的缩写名称显示。例如，Media Player常被显示为“mplayer”。如果你不能通过程序的缩写名称确定它是否是你需要删除的程序，请在删除它前在线搜索有关信息确认清楚。

3. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-4)
3
**右击你需要协助的软件包。**在菜单中选择标记为移除。你可以选择为多个需要卸载的软件包重复该操作。

    - 你还可以选择标记为完全移除，以便可以删除配置文件和程序文件。

4. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184)
4
**点击应用按钮。**对所需卸载的所有软件包完成标记后，点击窗口上方的应用按钮。软件包管理器将提示你确认更改。再次点击应用接受更改并卸载程序。

方法2

### 使用软件中心进行卸载

1. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-6)
1

**打开软件中心。**软件中心是一个可以安装和卸载Linux软件的GUI软件包管理器。在较旧版本的Ubuntu上，软件中心位于应用程序菜单内。在较近期的版本中，你可以再Launcher内找到软件中心，或者在Dash搜索栏中搜索“software”。

2. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-7)
2
**打开已安装的软件。**在左边窗格内，点击已安装软件链接。这将打开所有已安装在你的系统上的软件列表。

3. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-5)
3
**卸载程序。**选中需要卸载的程序并点击工具栏上的移除按钮。你可能会被要求输入管理员密码。输入密码后，程序将被自动移除。

    - 你可以选择多个程序把它们添加到移除队列，然后点击移除按钮。当第一个程序完成卸载后，将开始对队列中下一个程序进行卸载。

方法3

### 使用终端进行卸载

1. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-2)
1
**打开终端。**你将使用“apt-get”命令，这是用于管理已安装程序的通用命令。在卸载程序时，你可能需要输入管理员密码。

    - 当你输入密码时，密码将不会被显示。完成输入后按回车即可。

2. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-3)
2
**浏览已安装的程序。**要查看已安装的软件包列表，请输入以下命令。请注意你希望卸载的软件包的名称。

dpkg --list

3. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-10)
3
**卸载程序和所有配置文件。**在终端中输入以下命令，把<programname>替换成你希望完全移除的程序：

sudo apt-get --purge remove <programname>

4. [resize,m_fixed,w_1184](../_resources/resize,m_fixed,w_1184-8)
4
**只卸载程序。**如果你移除程序但保留配置文件，请输入以下命令：

sudo apt-get remove <programname>

- **  [**](#)  ****赞  **

- **  [**](#)  ****收藏  **

- **  [**](#)  **2**评论  **

- **  [**](#)  分享  **

- **  [**](#)  举报  **

上一篇：[2018 python获取动态User-Agent](https://blog.51cto.com/u_15064656/4387855)

下一篇：[django实战总结2](https://blog.51cto.com/u_15064656/4387858)

 [![noavatar_middle.gif](../_resources/noavatar_middle.gif)](https://blog.51cto.com/)

 提问和评论都可以，用心的回复会被更多人看到  **评论**

 **全部评论**  (**2**)  **最热  **最新

 [[ignore-error,1](../_resources/ignore-error,1)](https://blog.51cto.com/u_15064656/4387857)

###   [ff01d48e8391](https://blog.51cto.com/u_15064656/4387857)  1 年前

写的很好，加油

 **回复  **点赞****

 [[ignore-error,1](../_resources/ignore-error,1)](https://blog.51cto.com/u_15224061)

###   [7df05f9904cf](https://blog.51cto.com/u_15224061)  1 年前

写的挺不错的，继续加油哦

 **回复  **点赞****

 **相关文章**

- [ qt卸载     进入qt安装目录下 ghy@ghy-virtual-machine:~/Qt5.9.9$ sudo ./MaintenanceTool ...](https://blog.51cto.com/u_15127504/4346994)

- [ 怎么卸载jdk 才能卸载干净(怎么卸载jdk)     1、如果之前安装过程序,第一时间进入控制面板卸载:开始→控制面板→添加或删除程序→当前安装的程](https://blog.51cto.com/yetaotao/5799050)

- [ 怎么卸载javajdk_怎么卸载java环境     可以下载腾讯电脑管家卸载打开腾讯电脑管家——工具箱——软件管理——卸载软件腾讯电脑](https://blog.51cto.com/yetaotao/5796596)

- [ UBuntu14.04下安装和卸载Qt5.3.1     安装： 1. Qt5.3.1下载地址为：http://qt-project.org/，选择”Qt 5.3.1 for Linux 32-bit”版本，文件名是”qt-opensource-linux-x86-5.3.1.run”； 2. 进入qt-opensource-linux-x86-5.3.1](https://blog.51cto.com/u_15060547/4641808)

- [ Ubuntu 卸载wine     删除文件 删除applications 查看wine](https://blog.51cto.com/jiqing9006/3284809)

- [ ubuntu卸载软件     1.打开一个终端，输入​​dpkg --list​​ ,按下Enter键，终端输出以下内容，显示的是你电脑上安装的所有软件。2.在终端中找到你需要卸载的软件的名称，列表是按照首字母排序的。3.在终端上输入命令sudo apt-get --purge remove 包名（--purge是可选项，写上这个属性是将软件及其配置文件一并删除，如不需要删除配置文件，可执行sudo apt-get remov](https://blog.51cto.com/u_15057829/4185110)

- [ Ubuntu卸载MySQL     查看已安装的MySQL dpkg --list|grep mysql 卸载MySQL sudo apt remove mysql-common sudo apt remove mysql-server sudo apt remove mysql-server-5.7 sudo apt remove ...](https://blog.51cto.com/u_15127603/4386612)

- [ Ubuntu卸载VMware     # ./vmware-uninstallYou have gotten this message because you are either downgrading VMwareWorkstation, Player, or VIX,](https://blog.51cto.com/u_15080022/4746889)

- [ Ubuntu卸载Django     cd /usr/lib/python2.7/dist-packagessudo rm -rf django sudo rm Django-1.8.7.egg-info](https://blog.51cto.com/u_12836588/5742285)

- [ Ubuntu 完整卸载Ubuntu One     Ubuntu One捆绑安装在Ubuntu上，但是从来不用它。Google了一下完整的卸载方法。](https://blog.51cto.com/u_15127547/4646431)

- [ 怎么卸载nodejs？     Node.js是一个JavaScript运行环境，可以使JavaScript这类脚本语言编写出来的代码运行速度获得极大提升，那么安装后该如何卸载呢？ Windows平台下卸载nodejs 对于Windows平台来说，所有的应用程序的卸载方法都是一样的。 1、在【卸载程序】中卸载程序和功能 在桌面左下](https://blog.51cto.com/u_15080034/3844342)

- [ oracle怎么卸载     Oracle Database，又名Oracle RDBMS，或简称Oracle。是甲骨文公司的一款关系数据库管理系统。到目前仍在数据库市场上占有主要份额。劳伦斯·埃里森和他的朋友，之前的同事Bob Miner和Ed Oates在1977年建立了软件开发实验室咨询公司（SDL，Software De](https://blog.51cto.com/u_9595448/3273651)

- [ ubuntu 安装／卸载桌面     安装GNOME方法：sudo apt-get install gnome或者sudo apt-get install gnome-desktop删除Gnome的方法：apt-get --purge remove liborbit2](https://blog.51cto.com/u_975220/486384)

- [ Windows下卸载ubuntu      今天朋友想要卸载UBUNTU系统，完成，记录之... 方法：用Disk Genius（DG）将ubuntu的boot分区以及交换分区格式化后，通过DG硬盘选项中的&ldquo;重建主引导记录&rdquo;恢复适用于XP的MBR表，重启，大功告成！ 附上软件:http://dl.dbank.com/c0r481oh7n 疑问：](https://blog.51cto.com/aloha/604641)

- [ ubuntu下卸载vmware     直接crl+alt+t打开一个terminal，然后输入sudo vmware-installer --uninstall-product vmware-workstation即可卸载！操作如下图： 然后会马上弹出图形界面，后面的步骤类似于windows下的操作，所以不再赘述！ 再说一句，virtu](https://blog.51cto.com/u_15127679/3943329)

- [ Ubuntu 无法卸载光驱     当卸载文件系统的某个文件时出现umount: /home: device is busy 这是某个进程打开了文件，用fuser命令来查找其打开文件的进程并将其杀死就可以正常卸载文件了fuser -ki /目录/文件名](https://blog.51cto.com/gaoming/1109719)

- [ ubuntu彻底卸载软件     找此软件名称,sudo apt-get purge ......(点点程序名称),purge参数彻底删除文件,sudo apt-get autoremove,sudo apt-get clean和dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P 两条命令来清除残余配置文件 ...](https://blog.51cto.com/zhouzhenxing/1371478)

- [ ubuntu dpkg 软件卸载     ubuntu dpkg 软件卸载在Debian中卸载和清除软件包是两个不同的概念. 不同之处在于软件包被删除(卸载)后,它的配置文件仍会留在系统中,只有清除时才会删除它们. 默认情况下, Debian 仅会做删除操作, 除非你明确指出, 才会将配置文件删除. 如果要清除软件包, 则在清除前将会隐含地执行删除操作.要删除一个软件包,dpkg需要使用--remove选项将软件包卸载.与安装不同,删除只](https://blog.51cto.com/devingeng/1629728)

- [ ubuntu 彻底删除卸载     1 sudo apt-get install zabbix-agent 1916 sudo apt-get autoremove zabbix_agent root@(none):~# apt-get purge zabbix-agent Reading package lists... Done Building dependency tree R...](https://blog.51cto.com/bass/5069284)

 [  [ignore-error,1](../_resources/ignore-error,1)](https://blog.51cto.com/u_15064656)

 [mb5fe191195f1f1](https://blog.51cto.com/u_15064656)

 ****

- [114](https://blog.51cto.com/u_15064656/original)

[原创](https://blog.51cto.com/u_15064656/original)

- 63.2万

人气

- [10](https://blog.51cto.com/u_15064656/followers)

[粉丝](https://blog.51cto.com/u_15064656/followers)

- 3677

评论

- [0](https://blog.51cto.com/u_15064656/translate)

[翻译](https://blog.51cto.com/u_15064656/translate)

- [2013](https://blog.51cto.com/u_15064656/reproduce)

[转载](https://blog.51cto.com/u_15064656/reproduce)

- [0](https://blog.51cto.com/u_15064656/following)

[关注](https://blog.51cto.com/u_15064656/following)

- 12

 收藏

[resize,h_96,w_107](../_resources/resize,h_96,w_107-3)
[resize,h_96,w_107](../_resources/resize,h_96,w_107)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-5)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-2)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-7)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-1)
[format,webp](../_resources/format,webp)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-4)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-8)
[resize,h_96,w_107](../_resources/resize,h_96,w_107-6)

 [关注]()

 **近期文章**

- [1.062 200 SMART](https://blog.51cto.com/u_15451880/5928127)

- [2.运用ogg实现oracle 10g到19c的单表迁移](https://blog.51cto.com/u_12991611/5928120)

- [3.普通人如何不被 OpenAI 取代？](https://blog.51cto.com/u_15671528/5928117)

- [4.ToDesk企业版使用测试：破解企业远程办公难题，更安全更高效](https://blog.51cto.com/u_14122613/5928086)

- [5.微服务同时接入多个Kafka](https://blog.51cto.com/u_15733182/5928107)

 **文章目录**

- 使用Synaptic软件包管理器进行卸载

- 使用软件中心进行卸载

- 使用终端进行卸载

 [[ignore-error,1](../_resources/ignore-error,1-1)](https://blog.51cto.com/)

Copyright © 2005-2022 [51CTO.COM](http://www.51cto.com/) 版权所有 京ICP证060544号

关于我们

|     |     |     |     |
| --- | --- | --- | --- |
| [官方博客](https://blog.51cto.com/51ctoblog) | [意见反馈](https://blog.51cto.com/51ctoblog/2643377) | [了解我们](http://www.51cto.com/about/aboutus.html) | [全部文章](https://blog.51cto.com/nav) |
| [在线客服](#) | [网站地图](http://www.51cto.com/about/map.html) | [热门标签](https://blog.51cto.com/topic/all) |

友情链接

|     |     |
| --- | --- |
| [开源基础软件社区](https://ost.51cto.com/?utm_source=blogsitemap) | [51CTO学堂](https://edu.51cto.com/) |
| [51CTO](http://www.51cto.com/) |  [汽车开发者社区](https://icv.51cto.com/) |