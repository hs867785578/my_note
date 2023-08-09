[Linux](https://www.zhihu.com/topic/19554300)
[Linux 系统管理](https://www.zhihu.com/topic/19554305)
[Ubuntu](https://www.zhihu.com/topic/19557067)

# Linux文件系统中/bin、/sbin、/usr/bin、/usr/sbin、/usr/local/bin、/usr/local/sbin文件夹的区别是什么？

被浏览
**91,563**

[查看全部 14 个回答](https://www.zhihu.com/question/21265424)

[![v2-87e54074326cc73f412fde8dc953eccd_l.jpg](../_resources/v2-87e54074326cc73f412fde8dc953eccd_l.jpg)](https://www.zhihu.com/people/luozhiwen)

[罗罗的游戏](https://www.zhihu.com/people/luozhiwen)

最近在进行Linux[应用软件![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='627'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=%E5%BA%94%E7%94%A8%E8%BD%AF%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)的一些编译和安装工作。因为对Linux源码编译软件，包安装，依赖解决等流程一直都不是很熟悉，所以想借此机会梳理一下有关知识点。在以往的工作中遇到问题，要么就是通过yum命令直接安装软件包，要么就是按网上的教程无脑执行指令，大概率也能解决问题。

首先需要了解清楚Linux的目录结构，目录设计都是有一定规范的，编译，安装，更新，解决依赖，读取动态库等都会遵循这些目录，因此熟悉掌握Linux目录对工作中编译安装软件，环境配置，或者解决各种找不到文件，找不到依赖等问题，会非常有帮助。

## **【 / 】**

**> 安装tree命令，执行tree -L 1 /**
以下是根目录[ / ]下的结构 + 每个目录的解释

![v2-7588c00e425fbbf83f63794dd12b8181_r.jpg](../_resources/v2-7588c00e425fbbf83f63794dd12b8181_r.jpg)

根目录[ / ]下的不常用的目录功能只需要大概了解一下就行，不需要熟记
**重要目录一定要记得功能和了解**
重要的目录有：**etc lib lib64 root bin sbin usr var**

## **【 /usr 】**

**> 【第一个是/usr】：执行tree -L 1 /usr**
以下是目录[ /usr ]下的结构

![v2-bffbc2b62a38f27820f7f8628ff71718_r.jpg](../_resources/v2-bffbc2b62a38f27820f7f8628ff71718_r.jpg)

解释如下：
***[usr/bin]***
所有一般用户能够使用的指令都放在这里！

而使用[链接文件![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='628'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=%E9%93%BE%E6%8E%A5%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)的方式将 /bin 也链接至此！

也就是说，/usr/bin 与 /bin 是一模一样了！
另外，FHS要求在此目录下不应该有子目录！
所以此目录下不会存在目录，都是能够使用的命令！

***[/usr/etc]***
用来存放用户所需要的配置文件和子目录

***[/usr/games]***
与游戏比较相关的数据放置处

***[/usr/include]***
c/c++等程序语言的头文件（header）等
当我们以tarball方式 （*.tar.gz 的方式安装软件）安装某些数据时
会使用到里头的许多包含档

***[/usr/lib/]***
与 /lib 功能相同，/lib 也是链接到此目录的！

***[/usr/lib64/]***

与 /[lib![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='629'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=lib&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D) 功能相同，不过存放的是64位的

***[/usr/[libexec![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='630'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=libexec&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)/]***

某些不被一般使用者惯用的可执行文件或脚本（script）等等，会放置在此目录中
例如大部分的 X 窗口下面的操作指令， 很多都是放在此目录下的

***[/usr/sbin/]***
系统正常运行所需要的系统指令
基本功能与 /sbin 也差不多， 因此 /sbin 也是链接到此目录中！

***[/usr/share/]***

主要放置只读架构的数据文件，当然也包括[共享文件![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='631'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=%E5%85%B1%E4%BA%AB%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)。在这个目录下放

置的数据几乎是不分硬件架构均可读取的数据， 因为几乎都是文字文
件嘛！在此目录下常见的还有这些次目录：
/usr/share/man：线上说明文档
/usr/share/doc：软件杂项的文件说明
/usr/share/zoneinfo：与时区有关的时区文件

***[/usr/src/]***
一般源代码建议放置到这里，src有source的意思
至于核心源代码则建议放置到/usr/src/linux/目录下

***[/usr/local/]***
系统管理员在本机自行安装自己下载的软件建议安装到此目录，这样会比较便于管理
举例来说，你的distribution提供的软件较旧，你想安装较新的软件但又不想移除旧版
此时你可以将新版软件安装于/usr/local/目录下，就与原先的旧版软件有分别啦！
你可以自行到/usr/local去看看，该目录下也是具有bin, etc, include, lib的次目录！

## **【 /usr/local 】**

> 【第二个是/usr/local/】：执行tree -L 1 /usr/local
以下是目录[ /usr/local ]下的结构

![v2-083e9ece125f3af12402740e730b6384_r.jpg](../_resources/v2-083e9ece125f3af12402740e730b6384_r.jpg)

因为[ /usr/local ]目录和前面的目录有相似之处，这里就作个对比：
***[/bin]*** = 直接连接到/usr/bin
***[/usr/bin]*** = 存放着系统的命令，可执行程序等
***[/usr/local/bin] ***= 存放着用户安装的可执行程序

***[/etc] ***= 存放所有的系统管理所需要的配置文件和子目录
***[/usr/etc] ***= 存放系统软件的配置文件
***[/usr/local/etc]*** = 存放用户安装软件的配置

***[/games] ***= 存放系统游戏相关数据
***[/usr/games] ***= 存放系统游戏相关数据
***[/usr/local/games]*** = 存放用户游戏相关数据

***[无] ***=
***[/usr/include]*** = c/c++用到的头文件，很多程序编译都会调用
***[/usr/local/include]*** = 自己写的c/c++头文件，自己使用的

***[/lib]*** = 直接连接到/usr/lib
***[/usr/lib]*** = 存放着系统默认的动态库，静态库，常用并重要的系统库都放在这里
***[/usr/local/lib]*** = 存放着用户安装软件时编译的库（32位库）

***[/lib64]*** = 直接连接到/usr/lib64
***[/usr/lib64]*** = 存放着系统默认的动态库，静态库，常用并重要的系统库都放在这里
***[/usr/local/lib64]*** = 存放着用户安装软件时编译的库（64位库）

***[/无]*** =
***[/usr/libexec]*** = 存放着系统某些不被一般使用者惯用的可执行文件或脚本
***[/usr/local/libexec]*** = 存放着用户某些不被一般使用者惯用的可执行文件或脚本

***[/无] ***=

***[/usr/share/man]*** = 存放系统软件的配置文件，一些共享文件，类似于[指导帮助手册![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='632'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=%E6%8C%87%E5%AF%BC%E5%B8%AE%E5%8A%A9%E6%89%8B%E5%86%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)

***[/usr/local/man]*** = 用户安装软件时，放置类似指导帮助手册

***[/sbin]*** = 直接连接到/usr/sbin
***[/usr/sbin]*** = 存放的是系统管理员使用的，系统才能执行的程序
***[/usr/local/sbin]*** = 存放的是用户安装的软件，系统才能执行的程序

***[无]*** =
***[/usr/share]*** = 放置系统的只读架构的数据文件，也包括共享文件
***[/usr/local/share]*** = 放置用户的只读架构的数据文件，也包括共享文件

***[无]*** =
***[/usr/src]*** = 存放系统软件的的源代码
***[/usr/local/src]*** = 存放用户下载的软件的源代码

***特别说明***
> 在根外面的
> /bin ---> /usr/bin
> /sbin ---> /usr/sbin

> /lib ---> /usr/lib
> /lib64 ---> /usr/lib64

> 都需要被链接到/usr里面

> 这里说一下链接的原因：
> 早期 Linux 在设计的时候，若发生问题时，救援模式通常仅挂载根目录而已
> 因此有五个重要的目录被要求一定要与根目录放置在一起， 那就是： /etc, /bin, /dev, /lib, /sbin

> 现在许多的 > [> Linux distributions![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='633'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=Linux%20distributions&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)>  由于已经将许多非必要的文件移出 /> [> usr![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20width='10px'%20height='10px'%20viewBox='0%200%2015%2015'%20class='css-1dvsrp%20js-evernote-checked'%20data-evernote-id='634'%3e%3cpath%20d='M10.89%209.477l3.06%203.059a1%201%200%200%201-1.414%201.414l-3.06-3.06a6%206%200%201%201%201.414-1.414zM6%2010a4%204%200%201%200%200-8%204%204%200%200%200%200%208z'%20fill='currentColor'%3e%3c/path%3e%3c/svg%3e)](https://www.zhihu.com/search?q=usr&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2735929857%7D)>  之外了， 所以 /usr 也是越来越精简

> 同时因为 /usr 被建议为“即使挂载成为只读，系统还是可以正常运行”的模样，所以救援模式也能同时挂载 /usr 喔
> 因此那个五大目录的限制已经被打破了呦！就将/sbin, /bin, /lib 通通移动到 /usr 下面了

## **【 /usr/local/app-name】**

> 【第三个是/usr/local/app-name】：执行tree -L 1 /usr/local/app-name
以下是目录[ /usr/local/app-name]下的结构

![v2-6e0056b399854a605bb1cd343248e1c6_r.jpg](../_resources/v2-6e0056b399854a605bb1cd343248e1c6_r.jpg)

这就是通过源码安装软件后，安装目录下的结构，都是有一定规范的
有些可能还带有src，doc等，不一定都是完全这样，但都差不多
根据上面的目录分析，很容易理解软件安装下的目录的含义了
**注意：**
**用户自己安装软件路径一般是可选择的，不一定是在/usr/lcoal/目录下。**
**但一般都以/usr/lcoal/app-name为默认安装路径**

## **总结**

> 比较常用和重要目录
**用户可执行文件：**/bin、 /usr/bin、 /usr/local/bin
**系统可执行文件：**/sbin、 /usr/sbin、 /usr/local/sbin
**共享库：**/lib、 /usr/lib、 /usr/local/lib 或者 /lib64、 /usr/lib64、 /usr/local/lib64
**源文件：**/usr/src、 /usr/local/src
**配置：**/etc、 /usr/etc、 /usr/local/etc
**头文件：**/usr/include、 /usr/local/include

> 一般目录
**主目录：**/root、 /home/username
**其他挂载点：**/media、 /mnt
**分享目录：**/usr/share、 /usr/local/share
**内核和Bootloader：**/boot
**服务器数据：**/var、 /srv
**系统信息：**/proc、 /sys

下一篇：继续学习Linux环境配置 + 动态库/静态库配置，点个关注不错过！
[(L)](https://zhuanlan.zhihu.com/p/578816049)
**欢迎评论交流，点赞，收藏，转发加关注！**

*公众号：**游戏服务器开发***
*来自一个想开水果店的游戏程序员*

[编辑于 2022-10-30 22:46](https://www.zhihu.com/question/21265424/answer/2735929857)・IP 属地广东

#### 更多回答

[![v2-f11667e070a50e2a59abf4f740fe35bc_l.jpg](../_resources/v2-f11667e070a50e2a59abf4f740fe35bc_l.jpg)](https://www.zhihu.com/people/xi-yang-86-73)

[Xi Yang](https://www.zhihu.com/people/xi-yang-86-73)
头像是色图

传统上的常规做法是：

- • 系统级的组件放在/bin、/lib；
- • 根用户才能访问的放在/sbin；
- • 系统repository提供的应用程序放在/usr/bin、/usr/lib；
- • 用户自己编译的放在/usr/local/XXX。

现在有一些变化，在大约两年前，大量Linux系统都将/bin、/lib弄成/usr/bin、/usr/lib的符号链接。

此外，不同系统还会有很多的细微区别，比如Redhat系喜欢把32位的库放在/lib、/usr/lib，64位的库放在/lib64、/usr/lib64，而Debian系喜欢把平台相关的那层名字放在/lib、/usr/lib的子目录里，比如/usr/lib/x86_64-linux-gnu/。然后，各种配置文件的文件名、路径也会有区别，比如ssh服务器的配置文件可能叫/etc/ssh/sshd.conf，也可能叫/etc/ssh/sshd_config。。。

分成三块的最早的渊源，据说是这样的：
1. 1. Unix开发者的机器的硬盘不够了，新加了一块，挂在/usr上；
2. 2. 又TM不够了，再加一块，挂在/usr/local上；
3. 3. 不知怎么，就变成规范了。。。。

[![v2-962666715adf773d2cad2d2ca8b1cbc0_l.jpg](../_resources/v2-962666715adf773d2cad2d2ca8b1cbc0_l.jpg)](https://www.zhihu.com/people/snowinmay)

[俞坚奇](https://www.zhihu.com/people/snowinmay)
前端工程师

//别人写的，觉得写的不错
所有用户皆可用的系统程序放在/bin
超级用户才能使用的系统程序放在/sbin
所有用户都可用的应用程序放在/usr/bin
超级用户才能使用的应用程序放在/usr/sbin
所有用户都可用的与本地机器无关的程序存放在/usr/local/bin
超级用户才能使用的与本地机器无关的程序存放在/usr/local/sbin

[查看全部 14 个回答](https://www.zhihu.com/question/21265424)
关于作者

[![v2-87e54074326cc73f412fde8dc953eccd_xl.jpg](../_resources/v2-87e54074326cc73f412fde8dc953eccd_xl.jpg)](https://www.zhihu.com/people/luozhiwen)

[罗罗的游戏](https://www.zhihu.com/people/luozhiwen)
游戏程序员 公众号：游戏服务器开发

[ 回答 **24**](https://www.zhihu.com/people/luozhiwen/answers)[ 文章 **22**](https://www.zhihu.com/people/luozhiwen/posts)[ 关注者 **41**](https://www.zhihu.com/people/luozhiwen/followers)

被收藏 次
[编程](https://www.zhihu.com/collection/270415801)
[林夕文](https://www.zhihu.com/people/lin-xi-wen-18) 创建
0 人关注
[程序](https://www.zhihu.com/collection/131453278)
[夜光Arion](https://www.zhihu.com/people/wang-zi-mo-40-60) 创建
0 人关注
[IT技术](https://www.zhihu.com/collection/390144224)
[yuxl3](https://www.zhihu.com/people/yuxl3) 创建
0 人关注
相关问题
[linux如何禁止覆盖服务器上已经同名存在的文件？](https://www.zhihu.com/question/40029355) 0 个回答
[为什么 Linux 可以删除正在运行的程序文件？](https://www.zhihu.com/question/24837573) 3 个回答
[Linux命令/bin/sh怎么用?](https://www.zhihu.com/question/432824880) 5 个回答
[linux里root下为什么不能修改文件？](https://www.zhihu.com/question/353084515) 4 个回答
[如何评价linux与windows的文件名与路径的限制？](https://www.zhihu.com/question/29472905) 9 个回答

[刘看山](https://liukanshan.zhihu.com/)·[知乎指南](https://www.zhihu.com/question/19581624)·[知乎协议](https://www.zhihu.com/term/zhihu-terms)·[知乎隐私保护指引](https://www.zhihu.com/term/privacy)

[应用](https://www.zhihu.com/app)·[工作](https://app.mokahr.com/apply/zhihu)·

[侵权举报](https://zhuanlan.zhihu.com/p/28852607)·[网上有害信息举报专区](http://www.12377.cn/)

[京 ICP 证 110745 号](https://tsm.miit.gov.cn/dxxzsp/)
[京 ICP 备 13052560 号 - 1](https://beian.miit.gov.cn/)

[![v2-d0289dc0a46fc5b15b3363ffa78cf6c7.png](../_resources/v2-d0289dc0a46fc5b15b3363ffa78cf6c7.png)京公网安备 11010802020088 号](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010802020088)

[京网文[2022]2674-081 号](https://www.zhihu.com/certificates)

[药品医疗器械网络信息服务备案 （京）网药械信息备字（2022）第00334号](https://pic3.zhimg.com/v2-c280f8bce57f9b045b83185384d86027.png)服务热线：400-919-0001违法和不良信息举报：010-82716601[举报邮箱：jubao@zhihu.com](https://www.zhihu.com/question/21265424/answer/2735929857mailto:jubao@zhihu.com)

[儿童色情信息举报专区](https://www.zhihu.com/term/child-jubao)
[互联网算法推荐举报专区](https://www.zhihu.com/term/algorithm-recommend-report)
[养老诈骗举报专区](https://www.zhihu.com/term/pension-swindle-report)
[MCN 举报专区](https://www.zhihu.com/term/mcn/report)
[信息安全漏洞反馈专区](https://www.zhihu.com/term/info-sec)
[内容从业人员违法违规行为举报](https://www.zhihu.com/term/report-content-idustry)
[网络谣言信息举报入口](https://www.zhihu.com/term/net-report)

[证照中心](https://www.zhihu.com/certificates)·[Investor Relations](https://ir.zhihu.com/)

[联系我们](https://www.zhihu.com/contact) © 2022 知乎
北京智者天下科技有限公司版权所有

![v2-ccdb7828c12afff31a27e51593d23260_720w.png](../_resources/v2-ccdb7828c12afff31a27e51593d23260_720w.png)