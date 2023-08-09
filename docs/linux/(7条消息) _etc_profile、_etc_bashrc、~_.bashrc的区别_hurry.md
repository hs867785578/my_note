# /etc/profile、/etc/bashrc、~/.bashrc的区别

 ![reprint.png](../_resources/reprint.png)

 [hurryddd](https://blog.csdn.net/m0_37845735)  ![newCurrentTime2.png](../_resources/newCurrentTime2.png)  于 2022-06-26 10:49:05 发布  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  1628  [![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏   15]()

 分类专栏：  [# 嵌入式基础](https://blog.csdn.net/m0_37845735/category_10649332.html)  文章标签：  [bash](https://so.csdn.net/so/search/s.do?q=bash&t=blog&o=vip&s=&l=&f=&viparticle=)  [linux](https://so.csdn.net/so/search/s.do?q=linux&t=blog&o=vip&s=&l=&f=&viparticle=)  [开发语言](https://so.csdn.net/so/search/s.do?q=%E5%BC%80%E5%8F%91%E8%AF%AD%E8%A8%80&t=blog&o=vip&s=&l=&f=&viparticle=)

 [版权<div style="display: none;"></div>

]()

 [![489fad64a62648818eaaebc28e5c8659.jpg](../_resources/489fad64a62648818eaaebc28e5c8659.jpg)      华为云开发者联盟  该内容已被华为云开发者联盟社区收录，社区免费抽大奖，赢华为平板、Switch等好礼！](#)

 [加入社区]()

 [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-19)      嵌入式基础  专栏收录该内容](https://blog.csdn.net/m0_37845735/category_10649332.html)

 4 篇文章  0 订阅

 [订阅专栏]()

1> etc目录下存放系统管理和配置文件 (系统配置)

etc/profile:  profile为所有的用户设置系统范围的环境变量和启动顺序，当用户登录时读取该文件，这个文件对每个shell都有效。

 /etc/bashrc：为每一个运行bash shell的用户执行此文件，当bash shell被打开时,该文件被读取。也就是说，当用户shell执行了bash时，运行这个文件。

 总结如下：

/etc/profile 作用域大于/etc/bashrc。
 /etc/profile 是所有用户登录时加载配置环境，/etc/bashrc 是所有用户在执行bash shell 时，加载配置环境。

2> ~/.bashrc（用户配置）

该文件存储的是专属于个人bash shell的信息，当登录时以及每次打开一个新的shell时,执行这个文件，在这个文件里可以自定义用户专属的个人信息。

修改/etc路径下的配置文件将会应用到整个系统，属于系统级的配置，而修改用户目录下的.bashrc则只是限制在用户应用上，属于用户级设置。两者在应用范围上有所区别，建议如需修改的话，修改用户目录下的.bashrc，即无需root权限，也不会影响其他用户。

PATH环境变量

PATH变量决定了shell 将到哪些目录中寻找命令或程序。如果要执行的命令的目录在 $PATH 中，您就不必输入这个命令的完整路径，直接输入命令就可以了。一些第三方软件没有将可执行文件放到 Linux 的标准目录中。因此，将这些非标准的安装目录添加到 $PATH 是一种解决的办法。此外，您也将看到如何处理一般的环境变量。

首先，作为惯例，所有环境变量名都是大写。由于 Linux 区分大小写，这点您要留意。当然，您可以自己定义一些变量，如'$path'、'$pAtH'，但 shell 不会理睬这些变量。

第二点是变量名有时候以'$'开头，但有时又不是。当设置一个变量时，直接用名称，而不需要加“$”，如

“PATH=/usr/bin:/usr/local/bin:/bin”

假如要获取变量值的话，就要在变量名前加'$'：
       “echo $PATH”
       则会显示当前设置的PATH变量“/usr/bin:/usr/local/bin:/bin”

否则的话，变量名就会被当作普通文本了：
       “echo PATH”
       显示“PATH”

       处理 $PATH 变量要注意的第三点是：您不能只替换变量，而是要将新的字符串添加到原来的值中。在大多数情况下，您不能用“PATH=/some /directory”，因为这将删除 $PATH 中其他的所有目录，这样您在该终端运行程序时，就不得不给出完整路径。所以，只能作添加：“PATH=$PATH:/some/directory”，假如你要添加/usr/local/arm/3.4.1/bin交叉编译命令，则操作为“PATH=$PATH:/usr/local/arm/3.4.1/bin”

这样，PATH 被设成当前的值（以 $PATH 来表示）＋新添的目录。

到目前为止，你只为当前终端设置了新的 $PATH 变量。如果您打开一个新的终端，运行 echo $PATH ，将显示旧的 $PATH 值，而看不到你刚才添加的新目录。因为你先前定义的是一个局部环境变量（仅限于当前的终端）。

要定义一个全局变量，使在以后打开的终端中生效，您需要将局部变量输出(export)，可以用"export"命令：

       export PATH=$PATH:/some/directory

现在如果打开一个新的终端，输入 echo $PATH ，也能看到新设置的$PATH 了。请注意，命令'export'只能改变当前终端及以后运行的终端里的变量。对于已经运行的终端没有作用。

       为了将目录永久添加到 $PATH ，只要将"export"的那行添加到.bashrc或/etc/bashrc文件中。

　　使用命令:

　　sudo gedit ~/.bashrc

文章知识点与官方知识档案匹配，可进一步学习相关知识

[CS入门技能树](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c)**[Linux入门](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c)**[初识Linux](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c)23855 人正在系统学习中

[![3_m0_37845735](../_resources/3_wq1129)hurryddd](https://blog.csdn.net/m0_37845735)

 [关注](#)

- [![newHeart2021Black.png](../_resources/newHeart2021Black.png)   1]()

- [![newUnHeart2021Black.png](../_resources/newUnHeart2021Black.png)]()

- [![newCollectActive.png](../_resources/newCollectActive.png)   15](#)

- [![newComment2021Black.png](../_resources/newComment2021Black.png)  0]()

-

- [![newShareBlack.png](../_resources/newShareBlack.png)](#)

 [专栏目录]()

[/etc/*profile*文件简单介绍](https://blog.csdn.net/weixin_42031299/article/details/119089926)

[weixin_42031299的博客](https://blog.csdn.net/weixin_42031299)
![readCountWhite.png](../_resources/readCountWhite.png)1万+
[什么是/etc/*profile*文件

/etc/*profile*文件为系统的每个用户设置环境信息,此文件的修改会影响到所有用户。想了解更多细节内容可以用：vi /etc/*profile* 命令进行查看。

/etc/*profile*文件和.*bash**rc*文件的*区别*/etc/*profile*影响所有用户，.*bash**rc*影响当前用户。
/etc/*profile*文件的妙用

当需要某些操作在系统运行起来就自动执行时，可以考虑将该部分代码写到/etc/*profile*文件中。 例如： 我在部署海思的SDK包到海思芯片上时](https://blog.csdn.net/weixin_42031299/article/details/119089926)

[/etc/*bash**rc* 和 用户目录下.*bash**rc*的*区别*](https://blog.csdn.net/hooyying/article/details/83662180)

[hooyying的博客](https://blog.csdn.net/hooyying)
![readCountWhite.png](../_resources/readCountWhite.png)4278

[/etc/*bash**rc*,用户目录下.*bash**rc*有什么*区别*？ 一个是针对整个系统所有用户的,一个是针对特定用户的./etc/*bash**rc*修改了以后要重启系统才生效,而用户目录下.*bash**rc*修改了以后重新登录就生效 2。/etc/*profile*与/etc/*bash**rc*的*区别*？ 前一个主要用来设置一些系统变量,比如JAVA_HOME等等,后面一个主要用来保存一些*bash*的设置. /etc/profi...](https://blog.csdn.net/hooyying/article/details/83662180)

 [ *Linux* /etc/*profile*文件详解及修改后如何立即生效(使用sou*rc*e命令...](https://blog.csdn.net/weixin_38233274/article/details/80092837)

 12-9

 [ 登入系统读取步骤: 当登入系统时候获得一个shell进程时,其读取环境设定档有三步 : 1.首先读入的是全局环境变量设定档/etc/*profile*,然后根据其内容读取额外的设定的文档,如 /etc/*profile*.d和/etc/input*rc* 2.然后根据不同使用者帐号,去...](https://blog.csdn.net/weixin_38233274/article/details/80092837)

 [ sou*rc*e /etc/*profile*作用_llzhang_fly的博客_/etc/...](https://blog.csdn.net/llzhang_fly/article/details/104980029)

 12-3

 [ 1、/etc/*profile*:是操作系统定制用户环境使用的第一个文件,此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。 2、/etc/environment:在登录时操作系统使用的第二个文件,系统在读取你自己的*profile*前,设置环境的变量。](https://blog.csdn.net/llzhang_fly/article/details/104980029)

[浅析*linux* 下的/etc/*profile*、/etc/*bash**rc*、~/.*bash*_*profile*、~/.*bash**rc*](https://download.csdn.net/download/z5653821/4004615)

01-06

[浅析*linux* 下的/etc/*profile*、/etc/*bash**rc*、~/.*bash*_*profile*、~/.*bash**rc*](https://download.csdn.net/download/z5653821/4004615)

[/etc/*profile*文件详解](https://blog.csdn.net/thebigdipperbdx/article/details/80862224)

[thebigdipperbdx的博客](https://blog.csdn.net/thebigdipperbdx)
![readCountWhite.png](../_resources/readCountWhite.png)4194

[介绍*linux* /etc/*profile*文件的改变会涉及到系统的环境，也就是有关*Linux*环境变量的东西，学习*Linux*要了解*Linux*  *profile*文件的相关原理，这里对这一文件进行具体分析。这里修改会对所有用户起作用。

具体分析*Linux*是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环...](https://blog.csdn.net/thebigdipperbdx/article/details/80862224)

 [ *Linux*——》/etc/*profile*_小仙。的博客](https://blog.csdn.net/weixin_43453386/article/details/83587277)

 11-15

 [ *Linux*——》/etc/*profile* 一、/etc/*profile* 二、常见的环境变量 三、设置JDK 四、/etc/*profile*生效 一、/etc/*profile* /etc/*profile*文件里存放的是系统的环境变量,对所有用户都有效果,要对其更改的话,必须要在root用户权限下才能进行...](https://blog.csdn.net/weixin_43453386/article/details/83587277)

 [ /etc/*profile*配置文件的详解_暗影岛-寒冰射手的博客_etc/profi...](https://blog.csdn.net/m0_37477061/article/details/89436539)

 11-15

 [ 1./etc/*profile* , /etc/*profile*.d ,~/.*bash**rc*, ~/.*bash*_file,这几个文件的*区别*是啥,可能很多新人,很疑惑。即使很多配置一些软件环境变量的人也是很疑惑 ~/.*bash**rc*, ~/.*bash*_file这两个看到～这个符合,应该明白,这是宿主目录下...](https://blog.csdn.net/m0_37477061/article/details/89436539)

[【笔记】etc/*profile*和~/.*bash**rc*的*区别*](https://blog.csdn.net/ZoeYen_/article/details/78560905)

[ZoeYen_](https://blog.csdn.net/ZoeYen_)
![readCountWhite.png](../_resources/readCountWhite.png)6205

[在搭建单节点的hadoop集群时，jdk的环境变量是在~/.*bash**rc* 文件中配置的而搭建三节点的hadoop集群时，是在root用户下的 etc/*profile*目录下配置的环境变量两者有什么*区别*呢？/etc/*profile*： 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。是系统全局针对终端环境的设置，它是login时最先被系统加载的，是它调用了/etc/*bash**rc*，以及](https://blog.csdn.net/ZoeYen_/article/details/78560905)

[/etc/*profile*和.*bash**rc*两种环境变量](https://raychiu.blog.csdn.net/article/details/119348565)

[RayChiu757374816的博客](https://blog.csdn.net/RayChiu757374816)
![readCountWhite.png](../_resources/readCountWhite.png)612

[修改.*bash**rc*文件，它可以把使用这些环境变量的权限控制到用户级别，只是针对某一个特定的用户。而修改 /etc/*profile* 文件，它是针对于所有的用户，使所有用户都有权使用这些环境变量。 　　相比较起来，第一种方法更加安全，因为如果采用第二种方法，它可能会给系统带来安全性的问题。 　　建议：如果你的计算机仅仅作为*开发*使用，则推荐第二种方法，否则最好使用 第一种方法。

两种方式编辑后都需要sou*rc*e使其生效：

sudo vi/etc/*profile*变量名=变量值 ...=......](https://raychiu.blog.csdn.net/article/details/119348565)

 [ /etc/*profile* 解析_dingxiu8587的博客](https://blog.csdn.net/dingxiu8587/article/details/102160941)

 12-5

 [ 与环境变量相关的文件可能还会有/etc/*bash**rc*等,不过这是shell变量,是局部的,对于特定的shell器作用。/etc/*profile*是全局的,适用于所有的shell。 *profile*文件会告诉shell使用什么*语言*,什么shell,命令的搜索路径等等。](https://blog.csdn.net/dingxiu8587/article/details/102160941)

 [ *Linux*的etc下的*profile*详解,*linux*下 /etc/*profile*、~/.*bash*_*profile*...](https://blog.csdn.net/weixin_31504783/article/details/117009181)

 11-14

 [ 在刚登陆*Linux*时,首先启动 /etc/*profile* 文件,而后再启动用户目录下的 ~/.*bash*_*profile*、 ~/.*bash*_login或 ~/.*profile*文件中的其中一个,执行的顺序为:~/.*bash*_*profile*、 ~/.*bash*_login、 ~/.*profile*。若是 ~/.*bash*_*profile*文...](https://blog.csdn.net/weixin_31504783/article/details/117009181)

[*Linux*下/etc/*profile* 文件和/etc/*profile*.d*区别*以及相关说明](https://huaweicloud.csdn.net/63564504d3efff3090b5d012.html)

[xu710263124的博客](https://blog.csdn.net/xu710263124)
![readCountWhite.png](../_resources/readCountWhite.png)4456

[在*Linux*系统中，当我们在安装新的系统时，经常需要做的就是去修改环境变量，用到的最常见的就是/etc/*profile*文件，但是你会发现，有个与之相似的文件 /etc/*profile*.d文件。 那么就来介绍下/etc/*profile*文件和/etc/*profile*.d/的*区别*1、首先，/etc/*profile*/ ，此文件涉及系统的环境，即环境变量相关。这里的修改会对所有用户起作用。 当一个用户登录*Linux*系统或使用su -命令切换到另一个用户时，也就是Login shell启动时，首先要确保执行的启动脚本](https://huaweicloud.csdn.net/63564504d3efff3090b5d012.html)

[/etc/*bash**rc*和/etc/*profile*文件的*区别*](https://blog.csdn.net/supahero/article/details/107546428)

[supahero的博客](https://blog.csdn.net/supahero)
![readCountWhite.png](../_resources/readCountWhite.png)677

[  要明白两个文件*区别*，我们首先先要了解下面几个概念： • 登录式shell：需要输入用户名和密码才能进入Shell的方式。 • 非登录式shell：不需要输入用户和密码就能进入Shell，比如运行*bash*会开启一个新的会话窗口。

//至于如何判断,可以使用echo $0 [root@cp ~]# echo $0 -*bash* //登录式shell返回的结果 [root@cp ~]# *bash*[root@cp ~]# echo $0*bash*](https://blog.csdn.net/supahero/article/details/107546428)

 [ *linux* /etc/*profile*文件,*linux*系统中/etc/*profile*和.*profile*的介绍_weixi...](https://blog.csdn.net/weixin_39594080/article/details/116556076)

 12-9

 [ /etc/*profile*文件的主要功能包括:显示UNIX/Xenix版本信息或者系统专用应用程序的 提示信息,设置掩码(umask),对终端和邮箱(mail box)进行处理,对非root用户禁止使用new s命令等。 因为/etc/*profile*文件的作用范围是全体用户,所以非共性的设...](https://blog.csdn.net/weixin_39594080/article/details/116556076)

 [ etc/*profile* 文件和/etc/*profile*.d_u011277123的博客_/etc/p...](https://blog.csdn.net/u011277123/article/details/72864826/)

 11-16

 [ /etc/*profile* 文件 当一个用户登录*Linux*系统或使用su -命令切换到另一个用户时,也就是Login shell 启动时,首先要确保执行的启动脚本就是 /etc/*profile* 。 敲黑板:只有Login shell 启动时才会运行 /etc/*profile* 这个脚本,而Non-login...](https://blog.csdn.net/u011277123/article/details/72864826/)

[/etc/*profile*、/etc/*bash**rc*、~/.*bash*_*profile*、~/.*bash**rc*的*区别*](https://blog.csdn.net/u014093837/article/details/107508253)

[u014093837的博客](https://blog.csdn.net/u014093837)
![readCountWhite.png](../_resources/readCountWhite.png)305

[/etc/*profile*:此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行. 并从/etc/*profile*.d目录的配置文件中搜集shell的设置. /etc/*bash**rc*:为每一个运行*bash* shell的用户执行此文件.当*bash* shell被打开时,该文件被读取. ~/.*bash*_*profile*:每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该 文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.*bash**rc*文件. ~/.*bash**rc*:该文件包含](https://blog.csdn.net/u014093837/article/details/107508253)

[Ubuntu /etc/*profile*权限不够](https://blog.csdn.net/weixin_42033981/article/details/102755610)

[weixin_42033981的博客](https://blog.csdn.net/weixin_42033981)
![readCountWhite.png](../_resources/readCountWhite.png)9040

[Ubuntu /etc/*profile*权限不够 出现问题的时间比较长了，一直不知道怎么解决（网上也没搜到类似问题），也不知道什么原因。今天试着找了一下解决方法，成功解决了问题。 问题描述 /usr/sbin/lightdm-session:行 29: /etc/*profile*：权限不够 读取/etc/*profile*时发现错误 作为结果，会话不会被正确配置

问题如下图

解决方法 用文本的方式打开...](https://blog.csdn.net/weixin_42033981/article/details/102755610)

 [ *linux* /etc/*profile*文件,*Linux* 配置文件 /etc/*profile*_春草池塘梦...](https://blog.csdn.net/weixin_32394015/article/details/116556079)

 12-10

 [ *Linux* 配置文件 /etc/*profile* 1. 显示环境变量HOME $ echo $HOME /home/redbooks 2. 设置一个新的环境变量hello $ export HELLO="Hello!" $ echo $HELLO Hello! 3. 使用env命令显示所有的环境变量 ...](https://blog.csdn.net/weixin_32394015/article/details/116556079)

[*Linux*下 /etc/*profile*文件修改及生效  热门推荐](https://huaweicloud.csdn.net/63560e5ad3efff3090b5917a.html)

[wangmx1985的博客](https://blog.csdn.net/wangmx1985)
![readCountWhite.png](../_resources/readCountWhite.png)1万+
[遇到jmeter环境变量需要修改

/etc/*profile*中 export JMETER_HOME=/root/jmeter/apache-jmeter-5.1.1为/export JMETER_HOME=/data/jmeter/apache-jmeter-5.1.1

步骤：
1、当使用vim /etc/*profile*只有只读权限时，使用命令：
sudo vim /etc/*profile*2、修改完成后，需要立即生效
让/etc/*profile*文件修改后立即生效 ,可以使用如下命令:
sou](https://huaweicloud.csdn.net/63560e5ad3efff3090b5917a.html)

[/etc/*profile*与/etc/*bash**rc*的几点记录](https://blog.csdn.net/weixin_43390992/article/details/100034423)

[weixin_43390992的博客](https://blog.csdn.net/weixin_43390992)
![readCountWhite.png](../_resources/readCountWhite.png)88

[/etc/*profile*:此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从/etc/*profile*.d目录的配置文件中搜集shell的设置 ./etc/*bash**rc*:为每一个运行*bash* shell的用户执行此文件.当*bash* shell被打开时,该文件被读取. ~/.*bash*_*profile*:每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件...](https://blog.csdn.net/weixin_43390992/article/details/100034423)

[/etc/*bash**rc*和/etc/*profile*傻傻分不清楚？](https://blog.csdn.net/weixin_34346099/article/details/85822672)

[weixin_34346099的博客](https://blog.csdn.net/weixin_34346099)
![readCountWhite.png](../_resources/readCountWhite.png)1379

[导读 在一般的*linux* 或者 unix 系统中, 都可以通过编辑 *bash**rc* 和 *profile*来设置用户的工作环境, 很多文章对于 *profile* 和 *bash**rc* 也都有使用, 但究竟每个文件都有什么作用和该如何使用呢?

首先我们来看系统中的这些文件, 一般的系统可能会有 /etc/*profile*/etc/*bash**rc*~/.*bash**rc*~/....](https://blog.csdn.net/weixin_34346099/article/details/85822672)

[/etc/*profile*、/etc/*bash**rc*、~/.*bash*_*profile*、~/.*bash**rc* 文件的作用  最新发布](https://blog.csdn.net/qq_50685659/article/details/126011826)

[qq_50685659的博客](https://blog.csdn.net/qq_50685659)
![readCountWhite.png](../_resources/readCountWhite.png)44

[/etc/*profile*、/etc/*bash**rc*、~/.*bash*_*profile*、~/.*bash**rc* 文件的作用](https://blog.csdn.net/qq_50685659/article/details/126011826)

[*bash**rc*与*profile*的*区别*](https://blog.csdn.net/heybeaman/article/details/87289405)

[frank的专栏](https://blog.csdn.net/heybeaman)
![readCountWhite.png](../_resources/readCountWhite.png)7013

[*bash**rc*与*profile*的*区别*1， 要搞清*bash**rc*与*profile*的*区别*，首先要弄明白什么是交互式shell和非交互式shell，什么是login shell 和non-login shell。

交互式模式就是shell等待你的输入，并且执行你提交的命令。这种模式被称作交互式是因为shell与用户进行交互。这种模式也是大多数用户非常熟悉的：登录、执行一些命令、签退。当你签退后，she...](https://blog.csdn.net/heybeaman/article/details/87289405)

[*Linux*启动初始化配置文件浅析](https://blog.csdn.net/Kevinlou2008/article/details/17411441)

[kevin@hbstu.net](https://blog.csdn.net/Kevinlou2008)
![readCountWhite.png](../_resources/readCountWhite.png)597

[（1）/etc/*profile*   登录时，会执行。 全局（公有）配置，不管是哪个用户，登录时都会读取该文件。 （2）/ect/*bash**rc*   Ubuntu没有此文件，与之对应的是/ect/*bash*.*bash**rc*  *bash*.*bash**rc* 是交互式shell的初始化文件。

（3）~/.*profile*  某个用户读取的配置。 若*bash*是以login方式执行时，读取~](https://blog.csdn.net/Kevinlou2008/article/details/17411441)

[*Linux*下 环境变量/etc/*profile*、/etc/*bash**rc*、~/.*bash**rc*的*区别*](https://blog.csdn.net/a932432866/article/details/90638802)

[a932432866的专栏](https://blog.csdn.net/a932432866)
![readCountWhite.png](../_resources/readCountWhite.png)2616

[①/etc/*profile*: 该文件登录操作系统时，为每个用户设置环境信息，当用户第一次登录时,该文件被执行。也就是说这个文件对每个shell都有效，用于获取系统的环境信息。可以通过命令sou*rc*e /etc/*profile*立即生效

$ vi /etc/*profile*在里面加入: export PATH="$PATH:/my_new_path"

# /etc/*profile*# ...](https://blog.csdn.net/a932432866/article/details/90638802)

[*Linux* /etc/*profile*文件详解](https://blog.csdn.net/eudemon_cn/article/details/5853942)

[积木网络](https://blog.csdn.net/eudemon_cn)
![readCountWhite.png](../_resources/readCountWhite.png)5576

[<br />     *Linux* /etc/*profile*文件的改变会涉及到系统的环境，也就是有关*Linux*环境变量的东西，学习*Linux*要了解*Linux*  *profile*文件的相关原理，这里对则以文件进行具体分析。这里修改会对所有用户起作用。<br />　　1、*Linux*是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环境变量的定义。用户可以对自己的运行环境进行定制，其方法就是修改相应的系统环境变量。<br />　　2、常在](https://blog.csdn.net/eudemon_cn/article/details/5853942)

[*linux*复制*profile*文件,全面解析*Linux* *profile*文件](https://blog.csdn.net/weixin_39621669/article/details/116576355)

[weixin_39621669的博客](https://blog.csdn.net/weixin_39621669)
![readCountWhite.png](../_resources/readCountWhite.png)444

[全面解析*Linux*  *profile*文件*Linux**profile*文件的改变会涉及到系统的环境，也就是有关*Linux*环境变量的东西，学习*Linux*要了解*Linux**profile*文件的相关原理，这里对则以文件进行具体分析。这里修改会对所有用户起作用。1、*Linux*是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环境变量的...](https://blog.csdn.net/weixin_39621669/article/details/116576355)

[~/.*bash**rc*和/etc/*profile*的異同](https://blog.csdn.net/SONGCHUNHONG/article/details/50354092)

[Chunhong Song的专栏](https://blog.csdn.net/SONGCHUNHONG)
![readCountWhite.png](../_resources/readCountWhite.png)315

[什麼叫做死讀書不如無數，什麼叫書到用時方恨少。 我今天算是有體驗了一把。哈哈～不過，也發現，問題只有是自己實實在在解決的，才能記得牢。 前幾天載倒騰ubuntu14.04不能聯網的問題上浪費了N多時間，然後一位是自己把系統的配置文件搞亂了，於是在折騰了N久之后，终于决定重装系统，然后我用了小半年的系统和各种配置就彻底拜拜了，最后一个同学，一语道破天机。原来是我们实验室的网络载应用前登记了MAC](https://blog.csdn.net/SONGCHUNHONG/article/details/50354092)

### “相关推荐”对你有帮助么？

- ![npsFeel1.png](../_resources/npsFeel1.png)

非常没帮助

- ![npsFeel2.png](../_resources/npsFeel2.png)

没帮助

- ![npsFeel3.png](../_resources/npsFeel3.png)

一般

- ![npsFeel4.png](../_resources/npsFeel4.png)

有帮助

- ![npsFeel5.png](../_resources/npsFeel5.png)

非常有帮助

 ©️2022 CSDN  皮肤主题：大白   设计师：CSDN官方博客    [返回首页](https://blog.csdn.net/)

- [关于我们](https://www.csdn.net/company/index.html#about)

- [招贤纳士](https://www.csdn.net/company/index.html#recruit)

- [商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)

- [寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)

- ![tel.png](../_resources/tel.png)  400-660-0108

- ![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/m0_37845735/article/details/125467763mailto:webmaster@csdn.net)

- ![cs.png](../_resources/cs.png)  [在线客服](https://csdn.s2.udesk.cn/im_client/?web_plugin_id=29181)

- 工作时间 8:30-22:00

- [公安备案号11010502030143](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010502030143)

- [京ICP备19004658号](http://beian.miit.gov.cn/publish/query/indexFirst.action)

- [京网文〔2020〕1039-165号](https://csdnimg.cn/release/live_fe/culture_license.png)

- [经营性网站备案信息](https://csdnimg.cn/cdn/content-toolbar/csdn-ICP.png)

- [北京互联网违法和不良信息举报中心](http://www.bjjubao.org/)

- [家长监护](https://download.csdn.net/tutelage/home)

- [网络110报警服务](http://www.cyberpolice.cn/)

- [中国互联网举报中心](http://www.12377.cn/)

- [Chrome商店下载](https://chrome.google.com/webstore/detail/csdn%E5%BC%80%E5%8F%91%E8%80%85%E5%8A%A9%E6%89%8B/kfkdboecolemdjodhmhmcibjocfopejo?hl=zh-CN)

- [账号管理规范](https://blog.csdn.net/blogdevteam/article/details/126135357)

- [版权与免责声明](https://www.csdn.net/company/index.html#statement)

- [版权申诉](https://blog.csdn.net/blogdevteam/article/details/90369522)

- [出版物许可证](https://img-home.csdnimg.cn/images/20220705052819.png)

- [营业执照](https://img-home.csdnimg.cn/images/20210414021142.jpg)

- ©1999-2022北京创新乐知网络技术有限公司

 ![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='2095'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='2096'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='2097'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2098'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='2099'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='2100'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2101'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='2102'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='2103'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='2104'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='2105'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='2106'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2107'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='2108'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='2109'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2110'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='2111'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='2112'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='2113'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='2114'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)