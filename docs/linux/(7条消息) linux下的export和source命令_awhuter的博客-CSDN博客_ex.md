# linux下的export和source命令

 ![original.png](../_resources/original.png)

 [awhuter](https://blog.csdn.net/qq_41253960)  ![newUpTime2.png](../_resources/newUpTime2.png)  已于 2022-03-21 21:48:19 修改  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  5034  [![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏   43]()

 分类专栏：  [linux](https://blog.csdn.net/qq_41253960/category_11362250.html)  文章标签：  [linux](https://so.csdn.net/so/search/s.do?q=linux&t=blog&o=vip&s=&l=&f=&viparticle=)  [shell](https://so.csdn.net/so/search/s.do?q=shell&t=blog&o=vip&s=&l=&f=&viparticle=)

 [版权<div style="display: none;"></div>

]()

 [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-20)      linux  专栏收录该内容](https://blog.csdn.net/qq_41253960/category_11362250.html)

 8 篇文章  0 订阅

 [订阅专栏]()

 <div style="display: none;"></div>

## 1、`export`命令

[参考](https://www.cnblogs.com/guojun-junguo/p/9855356.html)
语法：`export [-fnp] [变量名称]=[变量设置值]`

1. 在shell中执行程序时，export用于新增、修改或删除**环境变量**。

2. 一般在shell中运行脚本程序时，系统会创建一个shell（子shell），source除外（下面有解释），在子shell中定义的变量只在子shell中有效。（类比局部变量只在子程序(函数)中有效，在主程序中不可调用）,子shell可以用父shell中的环境变量。

3. 设置环境变量。`echo $变量名`（如`echo $PATH`）可以查看环境变量。若执行的程序在环境变量PATH中，直接输入程序名就可以，如果不在，则找不到该命令，需要完整路径表示命令位置。此时，可以添加该目录到环境变量。`PATH=$PATH:路径`，若想永久有效，添加到`~/.bashrc`中：(形如)`export PATH="$PATH:/opt/.../.../bin`

4. **只有环境变量(export的)才会传递到子shell中，对本shell和子shell有效（父shell无效）；一般变量对子shell也是无效的，只在本shell有效。**

## 2、`source`命令

***1、`source`命令***

`source ~/.../../file`

- 1

source命令作用：现在立刻马上在**当前shell**中按顺序**执行file中的脚本**。通常用于重新执行刚修改完的初始化文档。
如在linux中修改了`./bashrc`启动初始化文件，这时可以用`source`命令重新执行，使得修改生效而不用注销再次登录。

与./直接执行脚本不同，./是在子shell运行的，结果并没有反映到父shell中。source是直接在本shell中运行的，不会启动一个新的shell，所以脚本中设置的变量直接成为当前shell的一部分。

***2、`source file` 和 `sh file` 和 `./file` 的区别***

`sh file`和`./file`一样，重新建立新的shell执行脚本，子shell继承父shell的环境变量(export了才是），但是子shell新建的改变的变量不会影响父shell

 `source file`，**读取file中的脚本依次在当前shell中执行，没有建立新的子shell**

## 3、实验加深理解

三个bash脚本文件`noexport.sh`, `export.sh`, `test.sh`

```
*#noexport.sh*
var="test export and source"
```

- 1
- 2

```
*#export.sh*
var="test export and source"
export var

复制
```

- 1
- 2
- 3

```
*#test.sh*
echo $var
```

- 1
- 2

***实验1***

```
wfq@wfq:~$ source noexport.sh   *#本shell中执行noexport.sh*
wfq@wfq:~$ echo $var
test export and source
wfq@wfq:~$ source test.sh
test export and source
wfq@wfq:~$ sh test.sh  *#子shell中执行，无结果*

wfq@wfq:~$

*#解释：在本shell执行noexport.sh,没有export为环境变量，所以仅在本shell中变量有效，子shell中无效*
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10

***实验2***

```
wfq@wfq:~$ sh export.sh   *#在子shell执行export.sh*
wfq@wfq:~$ source test.sh   *#在本shell执行test.sh*

wfq@wfq:~$

*#结论解释:在子shell中的变量，尽管是export为环境变量了，在父shell仍然无效*
```

- 1
- 2
- 3
- 4
- 5
- 6

***实验3***

```
wfq@wfq:~$ source export.sh    *#在本shell执行，且export为环境变量了*
wfq@wfq:~$ source test.sh      *#在本shell中，环境变量肯定有效，一般变量也会有效*
test export and source
wfq@wfq:~$ sh test.sh        *#在子shell中，环境变量仍然有效*
test export and source
wfq@wfq:~$

*#结论解释：export的环境变量，在本shell和子shell均有效*
```

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8

文章知识点与官方知识档案匹配，可进一步学习相关知识

[CS入门技能树](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)**[Linux进阶](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)**[新增用户](https://edu.csdn.net/skill/gml/gml-107df6e5ed9b4bef8b035e0649b2449e)23855 人正在系统学习中

[【*export*】*Linux*中*export**命令*介绍，三种方法设置环境变量](https://huaweicloud.csdn.net/6356054ed3efff3090b58cbd.html)

[Xminyang的博客](https://blog.csdn.net/Xminyang)
![readCountWhite.png](../_resources/readCountWhite.png)1万+

[*Linux*中*export**命令*介绍，三种方法设置环境变量](https://huaweicloud.csdn.net/6356054ed3efff3090b58cbd.html)

[*linux**命令*：*export*  最新发布](https://blog.csdn.net/qq_16268979/article/details/127895390)

[qq_16268979的博客](https://blog.csdn.net/qq_16268979)
![readCountWhite.png](../_resources/readCountWhite.png)83

[*linux**命令*：*export*](https://blog.csdn.net/qq_16268979/article/details/127895390)

 [ *Linux**命令*之 -- *export* 设置/显示系统环境变量_liaowenxiong的博客-CSDN...](https://blog.csdn.net/liaowenxiong/article/details/117325103)

 12-9

 [ *export* 的基本作用就是将父 *shell* 中的局部变量设置为环境变量,使得该变量可以在子 *shell* 中使用。 *export*  *命令*用于将 *shell* 变量输出为环境变量,或者将 *shell* 函数输出为环境变量。 一个变量创建时,在它之后创建的 *shell* 进程是不知道...](https://blog.csdn.net/liaowenxiong/article/details/117325103)

 [ *export**命令*详解_weixin_34066347的博客](https://blog.csdn.net/weixin_34066347/article/details/93253852)

 11-20

 [ 首先说明,父进程可以将自己的环境变量写入到子进程的空间中,但是子进程无法将自己空间的数据写入到父进程中(至少*export**命令*做不到)。那么想要让子*shell*中的变量在父*shell*可见,最好的办法就是不要成为子*shell*,即只将该*shell*的内容导入到”...](https://blog.csdn.net/weixin_34066347/article/details/93253852)

[*linux*环境变量和*linux**命令**export*](https://huaweicloud.csdn.net/635665e8d3efff3090b5d34e.html)

[qq_41854911的博客](https://blog.csdn.net/qq_41854911)
![readCountWhite.png](../_resources/readCountWhite.png)5076

[什么是环境变量，*Linux*环境变量及作用 变量是计算机系统用于保存可变值的数据类型，我们可以直接通过变量名称来提取到对应的变量值。在 *Linux* 系统中，环境变量是用来定义系统运行环境的一些参数，比如每个用户不同的家目录（HOME）、邮件存放位置（MAIL）等。

值得一提的是，*Linux* 系统中环境变量的名称一般都是大写的，这是一种约定俗成的规范。

我们可以使用 env *命令*来查看到 *Linux* 系统中所有的环境变量，执行*命令*如下： [root@localhost ~]# env ORBIT_SOCKE](https://huaweicloud.csdn.net/635665e8d3efff3090b5d34e.html)

[*source**命令*](https://blog.csdn.net/qq_47100953/article/details/126832801)
[森林之王的博客](https://blog.csdn.net/qq_47100953)
![readCountWhite.png](../_resources/readCountWhite.png)584

[*source**命令**linux*](https://blog.csdn.net/qq_47100953/article/details/126832801)

 [ 【*export*】*Linux*中*export**命令*介绍,三种方法设置环境变量](https://blog.csdn.net/Xminyang/article/details/125117111)

 12-1

 [ $*export*PATH=$PATH:/home/dabai/test/bin 1 ▚ 02 拓展:三种方法设置环境变量 2.1 方法一:使用*export**命令* 普通用户即可修改;仅对当前登录的终端有效。 实例:将路径/home/dabai/test/bin添加到环境变量$PATH中 $*export*PATH=$PA...](https://blog.csdn.net/Xminyang/article/details/125117111)

 [ *export*:*命令*手册_开源*Linux*的博客](https://blog.csdn.net/weixin_38889300/article/details/120775835)

 11-22

 [ *export**命令*用于将*shell*变量输出为环境变量,或者将*shell*函数输出为环境变量。 一个变量创建时,它不会自动地为在它之后创建的*shell*进程所知。而*命令**export*可以向后面的*shell*传递变量的值。当一个*shell*脚本调用并执 行时,它不会自动得到原为...](https://blog.csdn.net/weixin_38889300/article/details/120775835)

[*Linux*中的*source**命令*](https://huaweicloud.csdn.net/63560531d3efff3090b58c71.html)

[weixin_48321825的博客](https://blog.csdn.net/weixin_48321825)
![readCountWhite.png](../_resources/readCountWhite.png)2万+

[*Linux*中的*source**命令*1、*source**命令*是什么？*source**命令*也称为“点*命令*”，也就是一个点符号（.），是bash的内部*命令*。 注意：该*命令*通常用*命令*“.”来替代 2、*source**命令* 功能（能干什么）？*source**命令*通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。因为*linux*所有的操作都会变成文件的格式存在。 示例： 当我修改了/etc/profile文件，我想让它立刻生效，而不用重新登录；这时就想到用*source**命令*，如:*source* /etc/profil](https://huaweicloud.csdn.net/63560531d3efff3090b58c71.html)

[*export**命令*](https://blog.csdn.net/wejfoasdbsdg/article/details/53930718)
[Crett](https://blog.csdn.net/wejfoasdbsdg)
![readCountWhite.png](../_resources/readCountWhite.png)1万+

[*export**命令*用于将*shell*变量输出为环境变量，或者将*shell*函数输出为环境变量。 一个变量创建时，它不会自动地为在它之后创建的*shell*进程所知。而*命令**export*可以向后面的*shell*传递变量的值。当一个*shell*脚本调用并执 行时，它不会自动得到原为脚本（调用者）里定义的变量的访问权，除非这些变量已经被显式地设置为可用。*export**命令*可以用于传递一个或多个变量的值到任何后继脚本。](https://blog.csdn.net/wejfoasdbsdg/article/details/53930718)

 [ *shell*中 的 *export**命令*_Dust_Evc的博客](https://blog.csdn.net/Dust_Evc/article/details/125277589)

 11-18

 [ 要使某个变量的值可以在其他*shell*中被改变,可以使用*export**命令*对已定义的变量进行输出。 *export**命令*将使系统在创建每一个新的*shell*时,定义这个变量的一个拷贝。 这个过程称之为变量输出。](https://blog.csdn.net/Dust_Evc/article/details/125277589)

 [ *linux*  *export**命令*_鲁班班班七号的博客](https://blog.csdn.net/liuxiaodong400/article/details/88185169)

 11-14

 [ *export**命令*  *Linux*  *export**命令*用于设置或显示环境变量。 在*shell*中执行程序时,*shell*会提供一组环境变量。*export*可新增,修改或删除环境变量,供后续执行的程序使用。同时,重要的一点是,*export*的效力仅及于该次登陆操作。注销或者重新开一个窗口...](https://blog.csdn.net/liuxiaodong400/article/details/88185169)

[*shell*-*export*和环境变量设置](https://blog.csdn.net/chengmei4012/article/details/100783100)

[(L)](https://blog.csdn.net/chengmei4012)
![readCountWhite.png](../_resources/readCountWhite.png)534

[语　　法：*export* [-fnp][变量名称]=[变量设置值] 补充说明： 在*shell*中执行程序时，*shell*会提供一组环境变量。 *export*可新增，修改或删除环境变量，供后续执行的程序使用。*export*的效力仅及于该此登陆操作。 参　　数： -f 　代表[变...](https://blog.csdn.net/chengmei4012/article/details/100783100)

[*Linux* 学习笔记 (6) —— *source**命令*](https://blog.csdn.net/Candyerer/article/details/113875813)

[Candyerer的博客](https://blog.csdn.net/Candyerer)
![readCountWhite.png](../_resources/readCountWhite.png)720

[1、*source*  *命令*的功能*source* filename	# filename必须是可执行的脚本文件 或者 . filename	# 注意“.”号后面还有一个空格

通知 当前*shell* 读入路径为filename的文件，并依次执行文件中的所有语句。 通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。 例如，当我们修改了/etc/profile文件，并想让它立刻生效，而不用重新登录，就可以使用*source**命令*，如“*source* /etc/profile”。](https://blog.csdn.net/Candyerer/article/details/113875813)

 [ *Linux*  *export*  *命令*用法_寒泉Hq的博客_*linux*中的*export*](https://blog.csdn.net/sinat_42483341/article/details/113712527)

 11-23

 [ *Linux*  *export*  *命令*用法 *Linux*  *export*  *命令*用于设置或显示环境变量。 在*shell* 中执行程序时,*shell* 会提供一组环境变量。*export* 可新增,修改或删除环境变量,供后续执行的程序使用。*export* 的效力仅限于该次登陆操作。 *export* [-fnp][变量...](https://blog.csdn.net/sinat_42483341/article/details/113712527)

[mysql*source* 数据库_MySQL 数据库 *source*  *命令*详解及实例](https://huaweicloud.csdn.net/63356e7ed3efff3090b56ab0.html)

[weixin_29129919的博客](https://blog.csdn.net/weixin_29129919)
![readCountWhite.png](../_resources/readCountWhite.png)7713

[MySQL 数据库*source*  *命令*详解及实例MySQL 数据库 *source*  *命令*，该*命令*是数据库导入*命令*。*source*  *命令*的用法非常简单，首先你需要进入 MySQL 数据库的*命令*行管理界面，然后选择需要导入的数据库，执行 *source*  *命令*。如下图所示。MySql 数据库 *source*  *命令*mysql> use testDatabase changedmysql> set na...](https://huaweicloud.csdn.net/63356e7ed3efff3090b56ab0.html)

[*Linux*  *export*  *命令*用法](https://blog.csdn.net/m0_54849806/article/details/126747183)

[m0_54849806的博客](https://blog.csdn.net/m0_54849806)
![readCountWhite.png](../_resources/readCountWhite.png)347

[在*shell* 中执行程序时，*shell* 会提供一组环境变量。*export* 可新增，修改或删除环境变量，供后续执行的程序使用。*export* 的效力仅限于该次登陆操作。*Linux*  *export*  *命令*用于设置或显示环境变量。](https://blog.csdn.net/m0_54849806/article/details/126747183)

[*linux* 下*export*的作用,*linux*  *export* 的作用](https://blog.csdn.net/weixin_33520952/article/details/116883033)

[weixin_33520952的博客](https://blog.csdn.net/weixin_33520952)
![readCountWhite.png](../_resources/readCountWhite.png)543

[功能说明：设置或显示环境变量。语　　法：*export*[-fnp][变量名称]=[变量设置值]补充说明：在*shell*中执行程序时，*shell*会提供一组环境变量。*export*可新增，修改或删除环境变量，供后续执行的程序使用。*export*的效力仅及于该此登陆操作。参　　数：　-f　代表[变量名称]中为函数名称。　-n　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。　-...](https://blog.csdn.net/weixin_33520952/article/details/116883033)

[*Linux*常用*命令*系列--*export*](https://huaweicloud.csdn.net/62a2b4da78f1bc4bebc12d80.html)

[华为云官方博客](https://blog.csdn.net/devcloud)
![readCountWhite.png](../_resources/readCountWhite.png)2322
[今天给大家介绍*Linux*的常用*命令*-*export*，大家感兴趣的可以留言交流哟~
1、*export*的输出

[root@ossserver01]$ echo $*SHELL*/bin/bash [root@ossserver01]$ *export*declare -x COLORTERM="1" declare -x CPU="x86_64" declare -x CSHEDIT="emacs" d...](https://huaweicloud.csdn.net/62a2b4da78f1bc4bebc12d80.html)

[*Linux*下*source**命令*详解  热门推荐](https://blog.csdn.net/violet_echo_0908/article/details/52056071)

[在努力！](https://blog.csdn.net/violet_echo_0908)
![readCountWhite.png](../_resources/readCountWhite.png)14万+

[*source**命令*用法*source* FileName*source**命令*作用在当前bash环境下读取并执行FileName中的*命令*。*注：该*命令*通常用*命令*“.”来替代。使用范例：*source* filename . filename（中间有空格）*source**命令*（从 C *Shell* 而来）是bash *shell*的内置*命令*。点*命令*，就是个点符号，（从Bourne *Shell*而来）是*source*的另一名称。同样](https://blog.csdn.net/violet_echo_0908/article/details/52056071)

[*Linux*  *export**命令*设置环境变量](https://blog.csdn.net/zd845101500/article/details/91817050)

[zd845101500的博客](https://blog.csdn.net/zd845101500)
![readCountWhite.png](../_resources/readCountWhite.png)6332

[*Linux*  *export**命令*用于设置或显示环境变量。*export*可新增，修改或删除环境变量，供后续执行的程序使用。*export*的效力仅及于该次登陆操作。

语法*export* [-fnp][变量名称]=[变量设置值]
参数说明：

-f 　代表[变量名称]中为函数名称。 -n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。 -p 　列出所有的*shell*赋予...](https://blog.csdn.net/zd845101500/article/details/91817050)

[在*Linux*里设置环境变量的方法（*export* PATH）](https://blog.csdn.net/qlexcel/article/details/121763069)

[qlexcel的专栏](https://blog.csdn.net/qlexcel)
![readCountWhite.png](../_resources/readCountWhite.png)1986

[一般来说，配置交叉编译工具链的时候需要指定编译工具的路径，此时就需要设置环境变量。例如我的mips-*linux*-gcc编译器在“/opt/au1200_rm/build_tools/bin”目录下，build_tools就是我的编译工具，则有如下三种方法来设置环境变量： 1、直接用*export**命令*（推荐） #*export* PATH=$PATH:/opt/au1200_rm/build_tools/bin 查看是否已经设好，可用*命令**export*查看： [root@localhost bin]# *export*](https://blog.csdn.net/qlexcel/article/details/121763069)

[*Linux*  *命令*（49）—— *export*  *命令*（builtin）](https://dablelv.blog.csdn.net/article/details/84310975)

[Dablelv 的博客专栏。](https://blog.csdn.net/K346K346)
![readCountWhite.png](../_resources/readCountWhite.png)1645

[*export*  *命令*为 *Shell* 内建*命令*，用于设置或显示环境变量，环境变量包含变量与函数。在 *Shell* 中执行程序时，*Shell* 会提供一组环境变量。*export* 可新增、删除或修改环境变量，供后续被执行的程序使用。*export* 的作用效果仅限于当前登录。](https://dablelv.blog.csdn.net/article/details/84310975)

[*linux*中*export*与*source*的作用](https://blog.csdn.net/qq_39759656/article/details/83547582)

[瞌睡的洋葱的博客](https://blog.csdn.net/qq_39759656)
![readCountWhite.png](../_resources/readCountWhite.png)1万+

[以前一直觉得*export*可有可无，虽然知道*export*是干嘛的，不就是把本地变量变成全局变量么（实际中叫环境变量），但是感觉好像没有这货也没影响，今天看了这篇博文，终于恍然大悟。用自己的语言，思维方式重新整理一遍

首先说明两个概念： 父*shell*与子*shell*，从*shell*A中启动一个*shell*，称之为*shell*B。 *shell*A为父*shell*，*shell*B为子*shell*。 最容易理解的情况就是...](https://blog.csdn.net/qq_39759656/article/details/83547582)

[*source**命令*的用法](https://blog.csdn.net/Levet/article/details/65442294)
[谁曾见过风-的博客](https://blog.csdn.net/Levet)
![readCountWhite.png](../_resources/readCountWhite.png)3643

[*source**命令*用法：*source* FileName 作用:在当前bash环境下读取并执行FileName中的*命令*。 注：该*命令*通常用*命令*“.”来替代。 如：*source* .bash_rc 与 . .bash_rc 是等效的。 注意：## 标题 ## *source**命令*与*shell* scripts的区别是，*source*在当前bash环境下执行*命令*，而scripts是启动一个子*shell*](https://blog.csdn.net/Levet/article/details/65442294)

[*source**命令*的作用](https://blog.csdn.net/cs1428907579/article/details/100428163)
[cs1428907579的博客](https://blog.csdn.net/cs1428907579)
![readCountWhite.png](../_resources/readCountWhite.png)2045

[当我修改了/etc/profile文件，我想让它立刻生效，而不用重新登录；这时就想到用*source**命令*，如:*source* /etc/profile 对*source*进行了学习，并且用它与sh 执行脚本进行了对比，现在总结一下。 ...](https://blog.csdn.net/cs1428907579/article/details/100428163)

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

 ©️2022 CSDN  皮肤主题：数字20   设计师：CSDN官方博客    [返回首页](https://blog.csdn.net/)

- [关于我们](https://www.csdn.net/company/index.html#about)

- [招贤纳士](https://www.csdn.net/company/index.html#recruit)

- [商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)

- [寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)

- ![tel.png](../_resources/tel.png)  400-660-0108

- ![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/qq_41253960/article/details/120286745mailto:webmaster@csdn.net)

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

 [![3_qq_41253960](../_resources/3_qq_41253960)](https://blog.csdn.net/qq_41253960)

 [awhuter](https://blog.csdn.net/qq_41253960)

 码龄5年    [![nocErtification.png](../_resources/nocErtification.png) 暂无认证](https://i.csdn.net/#/uc/profile?utm_source=14998968)

14万+
访问

[![blog4.png](../_resources/blog4.png)](https://blog.csdn.net/blogdevteam/article/details/103478461)

等级

1129
积分

35
粉丝

175
获赞

99
评论

844
收藏

 ![qiandao1@240.png](../_resources/qiandao1@240.png)

 ![51_create.png](../_resources/51_create.png)

 ![chizhiyiheng@240.png](../_resources/chizhiyiheng@240.png)

 ![1024@240.png](../_resources/1024@240.png)

 ![02d34b42a3ee476fb50850304ab67017.png](../_resources/02d34b42a3ee476fb50850304ab67017.png)

 ![qixiebiaobing4@240.png](../_resources/qixiebiaobing4@240.png)

 ![yuedu90@240.png](../_resources/yuedu90@240.png)

 [私信](https://im.csdn.net/chat/qq_41253960)

 [关注]()

 [![csdn-sou.png](../_resources/csdn-sou.png)]()

### 热门文章

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效![readCountWhite.png](../_resources/readCountWhite.png)30501](https://blog.csdn.net/qq_41253960/article/details/111563342)
- [用OpenCV进行相机标定(张正友标定,有代码)![readCountWhite.png](../_resources/readCountWhite.png)6889](https://blog.csdn.net/qq_41253960/article/details/124928140)
- [linux的环境变量配置文件，变量的显示、设置、删除、替换、取消![readCountWhite.png](../_resources/readCountWhite.png)6259](https://blog.csdn.net/qq_41253960/article/details/120296265)
- [CMakeLists.txt使用cmake_module, list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)![readCountWhite.png](../_resources/readCountWhite.png)6222](https://blog.csdn.net/qq_41253960/article/details/121256498)
- [linux下的export和source命令![readCountWhite.png](../_resources/readCountWhite.png)5025](https://blog.csdn.net/qq_41253960/article/details/120286745)

### 分类专栏

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-20)   python](https://blog.csdn.net/qq_41253960/category_11919650.html)

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-7)   计算机视觉](https://blog.csdn.net/qq_41253960/category_11438540.html)  15篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   数据结构与算法](https://blog.csdn.net/qq_41253960/category_11758004.html)  10篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-13)   私人笔记](https://blog.csdn.net/qq_41253960/category_11362279.html)

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   笔记](https://blog.csdn.net/qq_41253960/category_10510204.html)  6篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   C++](https://blog.csdn.net/qq_41253960/category_11362271.html)  26篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-5)   ROS](https://blog.csdn.net/qq_41253960/category_11362266.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-20)   linux](https://blog.csdn.net/qq_41253960/category_11362250.html)  8篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   slam](https://blog.csdn.net/qq_41253960/category_11362251.html)  5篇

 [![arrowDownWhite.png](../_resources/arrowDownWhite.png)]()

### 最新评论

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效](https://blog.csdn.net/qq_41253960/article/details/111563342#comments_24445526)

...  [烟雨入江南z:](https://blog.csdn.net/m0_72016578)  为什么我把空白分区移到C盘的后边，右击C盘那个调整移动分区选项是灰的

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效](https://blog.csdn.net/qq_41253960/article/details/111563342#comments_24291913)

...  [还我一份清凉:](https://blog.csdn.net/qq_61707806)  用了之后没用，现在电脑还变卡了![010.png](../_resources/010.png)

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效](https://blog.csdn.net/qq_41253960/article/details/111563342#comments_24240373)

...  [骑猪上高速22333:](https://blog.csdn.net/weixin_45769146)  感谢，成功扩充

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效](https://blog.csdn.net/qq_41253960/article/details/111563342#comments_24236881)

...  [kalilover:](https://blog.csdn.net/kalilover)  win10系统c盘成功扩容15g，数据有没有损坏不清楚，暂时没异常![015.png](../_resources/015.png)

- [小白操作Win10扩充C盘（把D盘内存分给C盘）亲测多次有效](https://blog.csdn.net/qq_41253960/article/details/111563342#comments_24204270)

...  [qq_19247785:](https://blog.csdn.net/qq_19247785)  D盘的东西会丢失吗？

### 您愿意向朋友推荐“博客详情页”吗？

- ![npsFeel1.png](../_resources/npsFeel1.png)

强烈不推荐

- ![npsFeel2.png](../_resources/npsFeel2.png)

不推荐

- ![npsFeel3.png](../_resources/npsFeel3.png)

一般般

- ![npsFeel4.png](../_resources/npsFeel4.png)

推荐

- ![npsFeel5.png](../_resources/npsFeel5.png)

强烈推荐

### 最新文章

- [C++11产生随机数，random库产生随机数](https://blog.csdn.net/qq_41253960/article/details/126475315)

- [C++智能指针unique_ptr, shared_ptr](https://blog.csdn.net/qq_41253960/article/details/125821569)

- [用OpenCV进行相机标定(张正友标定,有代码)](https://blog.csdn.net/qq_41253960/article/details/124928140)

[2022年32篇](https://blog.csdn.net/qq_41253960?type=blog&year=2022&month=08)

[2021年36篇](https://blog.csdn.net/qq_41253960?type=blog&year=2021&month=12)

[2020年3篇](https://blog.csdn.net/qq_41253960?type=blog&year=2020&month=12)

![1.png](../_resources/1.png)

### 目录

1. [1、export命令](https://blog.csdn.net/qq_41253960/article/details/120286745#t0)
2. [2、source命令](https://blog.csdn.net/qq_41253960/article/details/120286745#t1)
3. [3、实验加深理解](https://blog.csdn.net/qq_41253960/article/details/120286745#t2)

 ![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='2049'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='2050'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='2051'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2052'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='2053'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='2054'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2055'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='2056'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='2057'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='2058'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='2059'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='2060'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2061'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='2062'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='2063'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2064'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='2065'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='2066'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='2067'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='2068'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)