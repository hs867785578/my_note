[![20201124032511.png](../_resources/20201124032511.png)](https://www.csdn.net/)

[博客](https://blog.csdn.net/)

[下载](https://download.csdn.net/)
[学习](https://edu.csdn.net/)
[社区](https://bbs.csdn.net/)
[GitCode](https://gitcode.net/?utm_source=csdn_toolbar)
[云服务](https://dev-portal.csdn.net/welcome?utm_source=toolbar)
[猿如意](https://devbit.csdn.net/?source=csdn_toolbar)

[![2_qq_44640266](../_resources/2_qq_44640266)](https://blog.csdn.net/qq_44640266)

[会员中心 ![20210918025138.gif](../_resources/20210918025138.gif)](https://mall.csdn.net/vip)

[足迹](https://i.csdn.net/#/user-center/collection-list?type=1)
[动态](https://blink.csdn.net/)
[(L)](https://i.csdn.net/#/msg/index)[消息](https://i.csdn.net/#/msg/index)

[创作中心 ![20220627041202.png](../_resources/20220627041202.png)](https://mp.csdn.net/)

[发布](https://mp.csdn.net/edit)

# 如何调整Ubuntu的字体大小？

![original.png](../_resources/original.png)

[戎码关山](https://dghcs.blog.csdn.net/)  ![newCurrentTime2.png](../_resources/newCurrentTime2.png)  于 2020-02-20 23:04:35 发布  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  11105  ![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏  53

分类专栏：  [# linux](https://blog.csdn.net/dghcs18/category_9301471.html)
版权

[![resize,m_fixed,h_224,w_224](../_resources/resize,m_fixed,h_224,w_224-2)](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)[linux](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)[专栏收录该内容](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)

5 篇文章  1 订阅
订阅专栏
<div style="display: none;"></div>

我刚下载[Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020)的ISO文件装在vmware上，发现字体小的不行，我的眼睛看得非常吃力。

向别人询问来了方法：
1、打开终端，输入@ubuntu:~$ sudo install gnome-tweaks
![20200220225901979.png](../_resources/20200220225901979.png)
2、安装完成之后，输入gnome-tweaks
将打开设置终端

![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RnaGNzMTg=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RnaGNzMTg=,size_16,color_FFFFFF,t_70)

3、在font页面调整字体即scaling factor
调节缩放比例，就可以调整字体大小

![watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RnaGNzMTg=,size_16,color_FFFFFF,t_70](../_resources/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RnaGNzMTg=,size_16,color_FFFFFF,t_70-1)

大功告成

[(L)](https://blog.csdn.net/linxi8693/article/details/97389393)

[(L)](https://blog.csdn.net/linxi8693/article/details/97389393)[ubuntu](https://blog.csdn.net/linxi8693/article/details/97389393)[设置终端](https://blog.csdn.net/linxi8693/article/details/97389393)[字体大小](https://blog.csdn.net/linxi8693/article/details/97389393)

[(L)](https://blog.csdn.net/linxi8693)[linxi8693的博客](https://blog.csdn.net/linxi8693)

![readCountWhite.png](../_resources/readCountWhite.png) 1万+
[(L)](https://blog.csdn.net/linxi8693/article/details/97389393)

[(L)](https://blog.csdn.net/linxi8693/article/details/97389393)[操作步骤 1、打开终端，在终端界面单击鼠标右键，选择“Preferences” 2、上一步操作会进入，下面的界面，勾选上"Custom font" 3、单击右边的“Monospace Bold” ...](https://blog.csdn.net/linxi8693/article/details/97389393)

[(L)](https://blog.csdn.net/sudaroot/article/details/119083436)

[(L)](https://blog.csdn.net/sudaroot/article/details/119083436)[Ubuntu](https://blog.csdn.net/sudaroot/article/details/119083436)[18.04](https://blog.csdn.net/sudaroot/article/details/119083436)[调整](https://blog.csdn.net/sudaroot/article/details/119083436)[字体大小](https://blog.csdn.net/sudaroot/article/details/119083436)

[(L)](https://blog.csdn.net/sudaroot)[sudaroot的博客](https://blog.csdn.net/sudaroot)

![readCountWhite.png](../_resources/readCountWhite.png) 7490
[(L)](https://blog.csdn.net/sudaroot/article/details/119083436)

[(L)](https://blog.csdn.net/sudaroot/article/details/119083436)[屏幕分辨率太高了，导致整个系统字体看起来过小。 更新一下软件源 sudo apt-get update 安装gnome-tweaks桌面配置工具。 sudo apt install gnome-tweaks 安装完成后，在终端输入下面命令，弹出优化窗口 gnome-tweaks 设置缩放比例，调节所有字体缩放的倍数，如我设置1.5倍大小，自己根据自己的电脑分辨率定。 输入数字完成后，按下enter回车键即可。 全篇完。 本人是一个嵌入式未入门小白，博客仅仅代表我个人...](https://blog.csdn.net/sudaroot/article/details/119083436)

评论7条![commentArrowRightWhite.png](../_resources/commentArrowRightWhite.png)写评论

[![3_tongk](../_resources/3_wq1129)tongk的迷路](https://blog.csdn.net/tongk)

热评
第一条语句明明是显示的安装错误，楼主不看的吗
[(L)](https://blog.csdn.net/luobeihai/article/details/123949843)

[(L)](https://blog.csdn.net/luobeihai/article/details/123949843)[解决](https://blog.csdn.net/luobeihai/article/details/123949843)[Ubuntu](https://blog.csdn.net/luobeihai/article/details/123949843)[18.04版本高分辨率下导致字体过小问题_luobeihai的博客-CSDN...](https://blog.csdn.net/luobeihai/article/details/123949843)

12-11
[(L)](https://blog.csdn.net/luobeihai/article/details/123949843)

[(L)](https://blog.csdn.net/luobeihai/article/details/123949843)[我所使用的是小米笔记本,显示屏是3.2K的分辨率。由于分辨率太高了,然后在](https://blog.csdn.net/luobeihai/article/details/123949843)[ubuntu](https://blog.csdn.net/luobeihai/article/details/123949843)[18.04的版本下显示的字体很小,小到都看不清了那种。于是查找了](https://blog.csdn.net/luobeihai/article/details/123949843)[调整](https://blog.csdn.net/luobeihai/article/details/123949843)[18.04版本](https://blog.csdn.net/luobeihai/article/details/123949843)[字体大小](https://blog.csdn.net/luobeihai/article/details/123949843)[的方法如下: 安装gnome-tweaks工具 ...](https://blog.csdn.net/luobeihai/article/details/123949843)

[(L)](https://blog.csdn.net/weixin_39900608/article/details/110665257)

[(L)](https://blog.csdn.net/weixin_39900608/article/details/110665257)[centos7调节虚拟机字体_](https://blog.csdn.net/weixin_39900608/article/details/110665257)[ubuntu](https://blog.csdn.net/weixin_39900608/article/details/110665257)[虚拟机(中或英文版)](https://blog.csdn.net/weixin_39900608/article/details/110665257)[字体大小](https://blog.csdn.net/weixin_39900608/article/details/110665257)[调节傻瓜教程](https://blog.csdn.net/weixin_39900608/article/details/110665257)

[(L)](https://blog.csdn.net/weixin_39900608)[weixin_39900608的博客](https://blog.csdn.net/weixin_39900608)

![readCountWhite.png](../_resources/readCountWhite.png) 5905
[(L)](https://blog.csdn.net/weixin_39900608/article/details/110665257)

[(L)](https://blog.csdn.net/weixin_39900608/article/details/110665257)[最近学ics和数据库，用了](https://blog.csdn.net/weixin_39900608/article/details/110665257)[ubuntu](https://blog.csdn.net/weixin_39900608/article/details/110665257)[的虚拟机，感觉字体太小了，所以探索了很多方法调解了一下，总结如下。（个人强推第三种方法）1.方法一桌面右击，选择“打开终端”（或“open terminal)，输入 "sudo apt-get install unity-tweak-tool"(以上命令可以直接复制，但记住要去掉双引号），然后回车。安装过程中出现的一些的问你yes or no的全部输入yes...](https://blog.csdn.net/weixin_39900608/article/details/110665257)

[(L)](https://blog.csdn.net/weixin_42506905/article/details/89970994)

[(L)](https://blog.csdn.net/weixin_42506905/article/details/89970994)[ubuntu](https://blog.csdn.net/weixin_42506905/article/details/89970994)[更改系统](https://blog.csdn.net/weixin_42506905/article/details/89970994)[字体大小](https://blog.csdn.net/weixin_42506905/article/details/89970994)

[(L)](https://blog.csdn.net/weixin_42506905)[磨镜台的博客](https://blog.csdn.net/weixin_42506905)

![readCountWhite.png](../_resources/readCountWhite.png) 6349
[(L)](https://blog.csdn.net/weixin_42506905/article/details/89970994)

[(L)](https://blog.csdn.net/weixin_42506905/article/details/89970994)[gnome-tweaks工具 apt install gnome-tweaks 安装打开后，选择字体选项更改大小。](https://blog.csdn.net/weixin_42506905/article/details/89970994)

[(L)](https://blog.csdn.net/weixin_42735097/article/details/96919748)

[(L)](https://blog.csdn.net/weixin_42735097/article/details/96919748)[笔记本](https://blog.csdn.net/weixin_42735097/article/details/96919748)[调整](https://blog.csdn.net/weixin_42735097/article/details/96919748)[ubuntu](https://blog.csdn.net/weixin_42735097/article/details/96919748)[14.04界面、](https://blog.csdn.net/weixin_42735097/article/details/96919748)[字体大小](https://blog.csdn.net/weixin_42735097/article/details/96919748)

[(L)](https://blog.csdn.net/weixin_42735097)[weixin_42735097的博客](https://blog.csdn.net/weixin_42735097)

![readCountWhite.png](../_resources/readCountWhite.png) 1902
[(L)](https://blog.csdn.net/weixin_42735097/article/details/96919748)

[(L)](https://blog.csdn.net/weixin_42735097/article/details/96919748)[因为使用matebook 13安装了](https://blog.csdn.net/weixin_42735097/article/details/96919748)[ubuntu](https://blog.csdn.net/weixin_42735097/article/details/96919748)[14.04，结果](https://blog.csdn.net/weixin_42735097/article/details/96919748)[ubuntu](https://blog.csdn.net/weixin_42735097/article/details/96919748)[显示的界面、字体等都小到要贴近屏幕才能看清屏幕，实在太累、太伤眼睛了。通过网上搜索教程，一开始找到了改变终端字体的方法，嗯，用着还不错。但是，后来发现打开其它的软件和终端也还是一样小。捣鼓捣鼓着无意中找到了能](https://blog.csdn.net/weixin_42735097/article/details/96919748)[调整](https://blog.csdn.net/weixin_42735097/article/details/96919748)[界面及](https://blog.csdn.net/weixin_42735097/article/details/96919748)[字体大小](https://blog.csdn.net/weixin_42735097/article/details/96919748)[的设置的方法，分享给其它有需要的人。 1.进入系统后点击](https://blog.csdn.net/weixin_42735097/article/details/96919748)[ubuntu](https://blog.csdn.net/weixin_42735097/article/details/96919748)[右上角的设置图标（齿轮状）； ...](https://blog.csdn.net/weixin_42735097/article/details/96919748)

[(L)](https://blog.csdn.net/weixin_43479651/article/details/123674638)

[(L)](https://blog.csdn.net/weixin_43479651/article/details/123674638)[ubuntu](https://blog.csdn.net/weixin_43479651/article/details/123674638)[终端](https://blog.csdn.net/weixin_43479651/article/details/123674638)[字体大小](https://blog.csdn.net/weixin_43479651/article/details/123674638)[调整](https://blog.csdn.net/weixin_43479651/article/details/123674638)[方法](https://blog.csdn.net/weixin_43479651/article/details/123674638)

[(L)](https://blog.csdn.net/weixin_43479651)[weixin_43479651的博客](https://blog.csdn.net/weixin_43479651)

![readCountWhite.png](../_resources/readCountWhite.png) 8840
[(L)](https://blog.csdn.net/weixin_43479651/article/details/123674638)

[(L)](https://blog.csdn.net/weixin_43479651/article/details/123674638)[1.打开终端，鼠标在终端页面右击，选择preferences,进入文本设置 2.勾选custom font,就可以](https://blog.csdn.net/weixin_43479651/article/details/123674638)[调整](https://blog.csdn.net/weixin_43479651/article/details/123674638)[字体和](https://blog.csdn.net/weixin_43479651/article/details/123674638)[字体大小](https://blog.csdn.net/weixin_43479651/article/details/123674638)[了。](https://blog.csdn.net/weixin_43479651/article/details/123674638)

[(L)](https://blog.csdn.net/weixin_42393245/article/details/114008086)

[(L)](https://blog.csdn.net/weixin_42393245/article/details/114008086)[ubuntu](https://blog.csdn.net/weixin_42393245/article/details/114008086)[系统字号_在K](https://blog.csdn.net/weixin_42393245/article/details/114008086)[ubuntu](https://blog.csdn.net/weixin_42393245/article/details/114008086)[显示中，字体尺寸太小 如何放大系统字体/符号的大小？...](https://blog.csdn.net/weixin_42393245/article/details/114008086)

[(L)](https://blog.csdn.net/weixin_42393245)[weixin_42393245的博客](https://blog.csdn.net/weixin_42393245)

![readCountWhite.png](../_resources/readCountWhite.png) 329
[(L)](https://blog.csdn.net/weixin_42393245/article/details/114008086)

[(L)](https://blog.csdn.net/weixin_42393245/article/details/114008086)[问题我有一个4k分辨率的屏幕，显示分辨率：3840x2160，我不想降低分辨率，以下是问题：非常小的字体，需要鼻子触摸到屏幕上才能看到是什么。系统规格：K](https://blog.csdn.net/weixin_42393245/article/details/114008086)[ubuntu](https://blog.csdn.net/weixin_42393245/article/details/114008086)  [16.04，配备NVIDIA Quadro M1000M的Dell Precision 5510 4k触摸屏答案1确定你的屏幕DPI，我是282创建一个文件，告诉xdpi是什么？# sudo nano /etc/X11/Xsessi...](https://blog.csdn.net/weixin_42393245/article/details/114008086)

[(L)](https://blog.csdn.net/baidu_41560343/article/details/87796946)

[(L)](https://blog.csdn.net/baidu_41560343/article/details/87796946)[ubuntu](https://blog.csdn.net/baidu_41560343/article/details/87796946)[18.04字体设置](https://blog.csdn.net/baidu_41560343/article/details/87796946)

[(L)](https://blog.csdn.net/baidu_41560343)[yingzhengttt blog](https://blog.csdn.net/baidu_41560343)

![readCountWhite.png](../_resources/readCountWhite.png) 1万+
[(L)](https://blog.csdn.net/baidu_41560343/article/details/87796946)

[(L)](https://blog.csdn.net/baidu_41560343/article/details/87796946)[ubuntu](https://blog.csdn.net/baidu_41560343/article/details/87796946)[18.04字体设置 本文主要来自于百度经验，网站链接 https://jingyan.baidu.com/article/cbf0e50056a6f52eaa28931c.html 1. 打开终端 右键打开 或者 ctrl+alt+t 快捷键打开终端窗口 2. 安装gnome-tweaks桌面配置工具 执行命令 sudo apt install gnome-tweaks 3. ...](https://blog.csdn.net/baidu_41560343/article/details/87796946)

[(L)](https://blog.csdn.net/qq_44065334/article/details/113924322)

[(L)](https://blog.csdn.net/qq_44065334/article/details/113924322)[ubuntu](https://blog.csdn.net/qq_44065334/article/details/113924322)[终端如何放大字体？](https://blog.csdn.net/qq_44065334/article/details/113924322)

[(L)](https://blog.csdn.net/qq_44065334)[qq_44065334的博客](https://blog.csdn.net/qq_44065334)

![readCountWhite.png](../_resources/readCountWhite.png) 2655
[(L)](https://blog.csdn.net/qq_44065334/article/details/113924322)

[(L)](https://blog.csdn.net/qq_44065334/article/details/113924322)[右上有所有快捷键的目录设置； 其中，ctrl+shift+=(即ctrl+"+")可以实现放大， 缩小ctrl±.](https://blog.csdn.net/qq_44065334/article/details/113924322)  [ubuntu](https://blog.csdn.net/qq_44065334/article/details/113924322)[对+的要求还是比较较真的，还得按一个shift…](https://blog.csdn.net/qq_44065334/article/details/113924322)

[(L)](https://blog.csdn.net/w690333243/article/details/53137988)

[(L)](https://blog.csdn.net/w690333243/article/details/53137988)[ubuntu](https://blog.csdn.net/w690333243/article/details/53137988)[终端](https://blog.csdn.net/w690333243/article/details/53137988)[字体大小](https://blog.csdn.net/w690333243/article/details/53137988)[和窗口大小设置](https://blog.csdn.net/w690333243/article/details/53137988)

[(L)](https://blog.csdn.net/w690333243)[王人冉的博客](https://blog.csdn.net/w690333243)

![readCountWhite.png](../_resources/readCountWhite.png) 6万+
[(L)](https://blog.csdn.net/w690333243/article/details/53137988)

[(L)](https://blog.csdn.net/w690333243/article/details/53137988)[CSDN的这个编辑器真的很麻烦，上传图片还只能一个一个上传，这里只上传图片，圈出关键点，不再进行文字描述，只写出关键点 记住，如果设置之后不起作用，可以重启下虚拟机试下，终端输入$reboot重启 小编正在配置ctags看android源码的工具，网上的配置方法比较凌乱，有比较全面的希望可以告知，谢谢 选择edit preferences：Profile preferences是prefer](https://blog.csdn.net/w690333243/article/details/53137988)

[(L)](https://blog.csdn.net/xiaoli_nu/article/details/89873310)

[(L)](https://blog.csdn.net/xiaoli_nu/article/details/89873310)[Ubuntu](https://blog.csdn.net/xiaoli_nu/article/details/89873310)  [16.04修改显示](https://blog.csdn.net/xiaoli_nu/article/details/89873310)[字体大小](https://blog.csdn.net/xiaoli_nu/article/details/89873310)

[(L)](https://blog.csdn.net/xiaoli_nu)[xiaoli_nu的博客](https://blog.csdn.net/xiaoli_nu)

![readCountWhite.png](../_resources/readCountWhite.png) 2万+
[(L)](https://blog.csdn.net/xiaoli_nu/article/details/89873310)

[(L)](https://blog.csdn.net/xiaoli_nu/article/details/89873310)[博客园有一位博主给出了两种方法，在此备份一下。 方法一： 参考：https://www.cnblogs.com/EasonJim/p/7456028.html Unity： 安装Unity Tweak Tool sudo apt-get install unity-tweak-tool GNOME： 打开优化工具： 方法二： 参考：http://www.cnblogs...](https://blog.csdn.net/xiaoli_nu/article/details/89873310)

[(L)](https://blog.csdn.net/beguile/article/details/88799811)

[(L)](https://blog.csdn.net/beguile/article/details/88799811)[Ubuntu](https://blog.csdn.net/beguile/article/details/88799811)  [设置屏幕](https://blog.csdn.net/beguile/article/details/88799811)[字体大小](https://blog.csdn.net/beguile/article/details/88799811)

[(L)](https://blog.csdn.net/beguile)[tfish的专栏](https://blog.csdn.net/beguile)
![readCountWhite.png](../_resources/readCountWhite.png) 2592
[(L)](https://blog.csdn.net/beguile/article/details/88799811)

[(L)](https://blog.csdn.net/beguile/article/details/88799811)[在【设置】-->【显示】-->【菜单和标题栏缩放比例】这里 从 1 改为比如 1.38 修改之后，滚动条 和 关闭/最小化/最大化按钮 也会跟着变大一些。 ...](https://blog.csdn.net/beguile/article/details/88799811)

[(L)](https://blog.csdn.net/qq_15192373/article/details/81561519)

[(L)](https://blog.csdn.net/qq_15192373/article/details/81561519)[Ubuntu](https://blog.csdn.net/qq_15192373/article/details/81561519)  [怎么](https://blog.csdn.net/qq_15192373/article/details/81561519)[调整](https://blog.csdn.net/qq_15192373/article/details/81561519)[终端](https://blog.csdn.net/qq_15192373/article/details/81561519)[字体大小](https://blog.csdn.net/qq_15192373/article/details/81561519)

[(L)](https://blog.csdn.net/qq_15192373)[梦Dancing的博客](https://blog.csdn.net/qq_15192373)

![readCountWhite.png](../_resources/readCountWhite.png) 6108
[(L)](https://blog.csdn.net/qq_15192373/article/details/81561519)

[(L)](https://blog.csdn.net/qq_15192373/article/details/81561519)[放大： ’Ctrl’ + ’shift ’ + ‘ + ’ 缩小：’Ctrl’ + ‘ - ’](https://blog.csdn.net/qq_15192373/article/details/81561519)

[(L)](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)

[(L)](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[ubuntu](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)  [终端模式下：](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[字体大小](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[设置](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)

[(L)](https://blog.csdn.net/CHENGZI_Y)[CHENGZI_Y的博客](https://blog.csdn.net/CHENGZI_Y)

![readCountWhite.png](../_resources/readCountWhite.png) 2万+
[(L)](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)

[(L)](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[//](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)  [ubuntu](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)  [终端模式字体设置太小，怎么](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[调整](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)[？1、sudo dpkg-reconfigure console-setup。 2、弹出 Configuring console-setup 界面，选择适当的编码格式，一般选择默认的UTF-8，选择OK 3、在接下来的界面里选择字体，可以依次尝试，选择默认的latin1 and latin5 -western Europe and Turkic](https://blog.csdn.net/CHENGZI_Y/article/details/52514976)

[(L)](https://blog.csdn.net/ben3726/article/details/25343901)

[(L)](https://blog.csdn.net/ben3726/article/details/25343901)[Ubuntu](https://blog.csdn.net/ben3726/article/details/25343901)[终端Terminal常用快捷键](https://blog.csdn.net/ben3726/article/details/25343901)

[(L)](https://blog.csdn.net/ben3726)[碧 月 有 约](https://blog.csdn.net/ben3726)
![readCountWhite.png](../_resources/readCountWhite.png) 2072
[(L)](https://blog.csdn.net/ben3726/article/details/25343901)

[(L)](https://blog.csdn.net/ben3726/article/details/25343901)[Ubuntu](https://blog.csdn.net/ben3726/article/details/25343901)[中的许多操作在终端（Terminal）中十分的快捷，记住一些快捷键的操作更得心应手。 在](https://blog.csdn.net/ben3726/article/details/25343901)[Ubuntu](https://blog.csdn.net/ben3726/article/details/25343901)[中打开终端的快捷键是 Ctrl+Alt+T 。其他的一些常用的快捷键如下:](https://blog.csdn.net/ben3726/article/details/25343901)

[(L)](https://blog.csdn.net/Gyuau/article/details/103085535)

[(L)](https://blog.csdn.net/Gyuau/article/details/103085535)[关于](https://blog.csdn.net/Gyuau/article/details/103085535)[ubuntu](https://blog.csdn.net/Gyuau/article/details/103085535)[设置字体样式，](https://blog.csdn.net/Gyuau/article/details/103085535)[调整](https://blog.csdn.net/Gyuau/article/details/103085535)[字体大小](https://blog.csdn.net/Gyuau/article/details/103085535)[的操作](https://blog.csdn.net/Gyuau/article/details/103085535)

[(L)](https://blog.csdn.net/Gyuau)[Gyuau的博客](https://blog.csdn.net/Gyuau)
![readCountWhite.png](../_resources/readCountWhite.png) 3233
[(L)](https://blog.csdn.net/Gyuau/article/details/103085535)

[(L)](https://blog.csdn.net/Gyuau/article/details/103085535)[ctrl+alt+t 快捷键进入终端 输入命令sudo apt install gnome-tweaks先安装 gnome-tweaks桌面配置工具 输入密码 等待，安装完成！ 按alt+f2 在运行窗口输入 gnome-tweaks 命令，然后回车。 接着会打开优化窗口，选择字体，可以自行按照需要调节字体属性。 -END- ...](https://blog.csdn.net/Gyuau/article/details/103085535)

[(L)](https://elasticstack.blog.csdn.net/article/details/50995471)

[(L)](https://elasticstack.blog.csdn.net/article/details/50995471)[Ubuntu](https://elasticstack.blog.csdn.net/article/details/50995471)[屏幕尺寸及](https://elasticstack.blog.csdn.net/article/details/50995471)[字体大小](https://elasticstack.blog.csdn.net/article/details/50995471)

[(L)](https://blog.csdn.net/UbuntuTouch)[Elastic 中国社区官方博客](https://blog.csdn.net/UbuntuTouch)

![readCountWhite.png](../_resources/readCountWhite.png) 6197
[(L)](https://elasticstack.blog.csdn.net/article/details/50995471)

[(L)](https://elasticstack.blog.csdn.net/article/details/50995471)[在今天的这篇文章中，我们们来做一个应用来显示](https://elasticstack.blog.csdn.net/article/details/50995471)[Ubuntu](https://elasticstack.blog.csdn.net/article/details/50995471)[字体及屏幕大小．这篇文章的内容在以前的一些文章中也有提及．在这里，我把所有的内容综合到一起，做成一个应用．这样大家可以一目了然．](https://elasticstack.blog.csdn.net/article/details/50995471)

[(L)](https://blog.csdn.net/Amorny959/article/details/125805013)

[(L)](https://blog.csdn.net/Amorny959/article/details/125805013)[ubuntu](https://blog.csdn.net/Amorny959/article/details/125805013)[修改字体](https://blog.csdn.net/Amorny959/article/details/125805013)

[最新发布](https://blog.csdn.net/Amorny959/article/details/125805013)

[(L)](https://blog.csdn.net/Amorny959)[Amorny959的博客](https://blog.csdn.net/Amorny959)

![readCountWhite.png](../_resources/readCountWhite.png) 252
[(L)](https://blog.csdn.net/Amorny959/article/details/125805013)

[(L)](https://blog.csdn.net/Amorny959/article/details/125805013)[我的终端打不开gnome-tweaks，指令下载...搞定](https://blog.csdn.net/Amorny959/article/details/125805013)

[(L)](https://blog.csdn.net/qq_30115765/article/details/52623935)

[(L)](https://blog.csdn.net/qq_30115765/article/details/52623935)[调整](https://blog.csdn.net/qq_30115765/article/details/52623935)[ubuntu](https://blog.csdn.net/qq_30115765/article/details/52623935)[命令行终端字体颜色和大小](https://blog.csdn.net/qq_30115765/article/details/52623935)

[热门推荐](https://blog.csdn.net/qq_30115765/article/details/52623935)

[(L)](https://blog.csdn.net/qq_30115765)[朱安的博客](https://blog.csdn.net/qq_30115765)

![readCountWhite.png](../_resources/readCountWhite.png) 6万+
[(L)](https://blog.csdn.net/qq_30115765/article/details/52623935)

[(L)](https://blog.csdn.net/qq_30115765/article/details/52623935)[刚装好](https://blog.csdn.net/qq_30115765/article/details/52623935)[ubuntu](https://blog.csdn.net/qq_30115765/article/details/52623935)[16.04新系统，由于电脑比较新，而且是笔记本1080p高清屏幕，分辨率一高，字体就变小了，又不甘心把分辨率调低，就自己动手丰衣足食吧，首先使用](https://blog.csdn.net/qq_30115765/article/details/52623935)[ubuntu](https://blog.csdn.net/qq_30115765/article/details/52623935)  [最常用的就是命令行终端，以我工作经历来说，基本百分之90以上都是在跟终端打交道，所以把终端设置好基本问题就解决了。试了网上该xterm的方法，不知道为什么我的电脑是没有反应的，好像那个也只能改变终端的大小，](https://blog.csdn.net/qq_30115765/article/details/52623935)[字体大小](https://blog.csdn.net/qq_30115765/article/details/52623935)[并不能改变。](https://blog.csdn.net/qq_30115765/article/details/52623935)

[(L)](https://blog.csdn.net/tp7309/article/details/60594995)

[(L)](https://blog.csdn.net/tp7309/article/details/60594995)[Ubuntu](https://blog.csdn.net/tp7309/article/details/60594995)[12.04](https://blog.csdn.net/tp7309/article/details/60594995)[调整](https://blog.csdn.net/tp7309/article/details/60594995)[系统](https://blog.csdn.net/tp7309/article/details/60594995)[字体大小](https://blog.csdn.net/tp7309/article/details/60594995)

[(L)](https://blog.csdn.net/tp7309)[不若乘风来](https://blog.csdn.net/tp7309)
![readCountWhite.png](../_resources/readCountWhite.png) 5068
[(L)](https://blog.csdn.net/tp7309/article/details/60594995)

[(L)](https://blog.csdn.net/tp7309/article/details/60594995)[最近应该有特殊需求，所以装了下](https://blog.csdn.net/tp7309/article/details/60594995)[Ubuntu](https://blog.csdn.net/tp7309/article/details/60594995)[12.04版本，感觉字体真是太小的不舒服，设置中又没有字体选项，于是找了下调字体的方法： 在](https://blog.csdn.net/tp7309/article/details/60594995)[Ubuntu](https://blog.csdn.net/tp7309/article/details/60594995)[软件中心搜索##高级设置##，安装后打开就可以调字体了！](https://blog.csdn.net/tp7309/article/details/60594995)

### “相关推荐”对你有帮助么？

![npsFeel1.png](../_resources/npsFeel1.png)
非常没帮助
![npsFeel2.png](../_resources/npsFeel2.png)
没帮助
![npsFeel3.png](../_resources/npsFeel3.png)
一般
![npsFeel4.png](../_resources/npsFeel4.png)
有帮助
![npsFeel5.png](../_resources/npsFeel5.png)
非常有帮助
©️2022 CSDN  皮肤主题：自定义皮肤  设计师：戎码关山  [返回首页](https://blog.csdn.net/)
[关于我们](https://www.csdn.net/company/index.html#about)
[招贤纳士](https://www.csdn.net/company/index.html#recruit)
[商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)
[寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)
![tel.png](../_resources/tel.png)  400-660-0108

![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/dghcs18/article/details/104420127mailto:webmaster@csdn.net)

![cs.png](../_resources/cs.png)  [在线客服](https://csdn.s2.udesk.cn/im_client/?web_plugin_id=29181)

工作时间 8:30-22:00

[公安备案号11010502030143](http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010502030143)

[京ICP备19004658号](http://beian.miit.gov.cn/publish/query/indexFirst.action)
[京网文〔2020〕1039-165号](https://csdnimg.cn/release/live_fe/culture_license.png)
[经营性网站备案信息](https://csdnimg.cn/cdn/content-toolbar/csdn-ICP.png)
[北京互联网违法和不良信息举报中心](http://www.bjjubao.org/)
[家长监护](https://download.csdn.net/tutelage/home)
[网络110报警服务](http://www.cyberpolice.cn/)
[中国互联网举报中心](http://www.12377.cn/)

[Chrome商店下载](https://chrome.google.com/webstore/detail/csdn%E5%BC%80%E5%8F%91%E8%80%85%E5%8A%A9%E6%89%8B/kfkdboecolemdjodhmhmcibjocfopejo?hl=zh-CN)

[账号管理规范](https://blog.csdn.net/blogdevteam/article/details/126135357)
[版权与免责声明](https://www.csdn.net/company/index.html#statement)
[版权申诉](https://blog.csdn.net/blogdevteam/article/details/90369522)
[出版物许可证](https://img-home.csdnimg.cn/images/20220705052819.png)
[营业执照](https://img-home.csdnimg.cn/images/20210414021142.jpg)
©1999-2022北京创新乐知网络技术有限公司

[![3_dghcs18](../_resources/3_dghcs18)](https://dghcs.blog.csdn.net/)

[(L)](https://dghcs.blog.csdn.net/)
[(L)](https://dghcs.blog.csdn.net/)[戎码关山](https://dghcs.blog.csdn.net/)
![vipNew.png](../_resources/vipNew.png)
码龄4年

[![nocErtification.png](../_resources/nocErtification.png) 暂无认证](https://i.csdn.net/#/uc/profile?utm_source=14998968)

57万+
访问

[![blog7.png](../_resources/blog7.png)](https://blog.csdn.net/blogdevteam/article/details/103478461)

等级

9496
积分
132
粉丝
340
获赞
131
评论
711
收藏
![blinkrecomment@240.png](../_resources/blinkrecomment@240.png)
![blinkhot@240.png](../_resources/blinkhot@240.png)
![qiandao10@240.png](../_resources/qiandao10@240.png)
![linkedin@240.png](../_resources/linkedin@240.png)
![chizhiyiheng@240.png](../_resources/chizhiyiheng@240.png)
![github@240.png](../_resources/github@240.png)
![maimai@240.png](../_resources/maimai@240.png)
![02d34b42a3ee476fb50850304ab67017.png](../_resources/02d34b42a3ee476fb50850304ab67017.png)
![qixiebiaobing4@240.png](../_resources/qixiebiaobing4@240.png)
![blog_normal_medal@240.png](../_resources/blog_normal_medal@240.png)
![blinknewcomer@240.png](../_resources/blinknewcomer@240.png)
![yuedu3@240.png](../_resources/yuedu3@240.png)
[私信](https://im.csdn.net/chat/dghcs18)
关注
![csdn-sou.png](../_resources/csdn-sou.png)

### **热门文章**

[当windows+D键不起作用时该怎么办？ ![readCountWhite.png](../_resources/readCountWhite.png)](https://dghcs.blog.csdn.net/article/details/102578655)  [36991](https://dghcs.blog.csdn.net/article/details/102578655)

[如何由密度函数求分布函数 ![readCountWhite.png](../_resources/readCountWhite.png)](https://dghcs.blog.csdn.net/article/details/103757086)  [32276](https://dghcs.blog.csdn.net/article/details/103757086)

[鼠标指针在微信界面消失怎么办？ ![readCountWhite.png](../_resources/readCountWhite.png)](https://dghcs.blog.csdn.net/article/details/102774919)  [28573](https://dghcs.blog.csdn.net/article/details/102774919)

[visual studio中，已经安装完成后如何再安装其他组件（即在安装过程中未勾选的）怎么办? ![readCountWhite.png](../_resources/readCountWhite.png)](https://dghcs.blog.csdn.net/article/details/97047042)  [18898](https://dghcs.blog.csdn.net/article/details/97047042)

[Macbook苹果笔记本出现闪屏现象的解决方法 ![readCountWhite.png](../_resources/readCountWhite.png)](https://dghcs.blog.csdn.net/article/details/101906465)  [13568](https://dghcs.blog.csdn.net/article/details/101906465)

### **分类专栏**

[(L)](https://blog.csdn.net/dghcs18/category_9877871.html)

[(L)](https://blog.csdn.net/dghcs18/category_9877871.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)](https://blog.csdn.net/dghcs18/category_9877871.html)  [LeetCode](https://blog.csdn.net/dghcs18/category_9877871.html)  [(L)](https://blog.csdn.net/dghcs18/category_9877871.html)

13篇
[(L)](https://blog.csdn.net/dghcs18/category_10603315.html)

[(L)](https://blog.csdn.net/dghcs18/category_10603315.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-43)](https://blog.csdn.net/dghcs18/category_10603315.html)  [#　ｐｙｔｈｏｎ基础](https://blog.csdn.net/dghcs18/category_10603315.html)  [(L)](https://blog.csdn.net/dghcs18/category_10603315.html)

[(L)](https://blog.csdn.net/dghcs18/category_10350050.html)

[(L)](https://blog.csdn.net/dghcs18/category_10350050.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-20)](https://blog.csdn.net/dghcs18/category_10350050.html)  [笔记](https://blog.csdn.net/dghcs18/category_10350050.html)  [(L)](https://blog.csdn.net/dghcs18/category_10350050.html)

5篇
[(L)](https://blog.csdn.net/dghcs18/category_9902045.html)

[(L)](https://blog.csdn.net/dghcs18/category_9902045.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)](https://blog.csdn.net/dghcs18/category_9902045.html)  [给自己打工](https://blog.csdn.net/dghcs18/category_9902045.html)  [(L)](https://blog.csdn.net/dghcs18/category_9902045.html)

[(L)](https://blog.csdn.net/dghcs18/category_9866853.html)

[(L)](https://blog.csdn.net/dghcs18/category_9866853.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-39)](https://blog.csdn.net/dghcs18/category_9866853.html)  [Tsinghua courses](https://blog.csdn.net/dghcs18/category_9866853.html)  [(L)](https://blog.csdn.net/dghcs18/category_9866853.html)

35篇
[(L)](https://blog.csdn.net/dghcs18/category_9724924.html)

[(L)](https://blog.csdn.net/dghcs18/category_9724924.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)](https://blog.csdn.net/dghcs18/category_9724924.html)  [计算机图形学](https://blog.csdn.net/dghcs18/category_9724924.html)  [(L)](https://blog.csdn.net/dghcs18/category_9724924.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9658214.html)

[(L)](https://blog.csdn.net/dghcs18/category_9658214.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-2)](https://blog.csdn.net/dghcs18/category_9658214.html)  [网站搭建](https://blog.csdn.net/dghcs18/category_9658214.html)  [(L)](https://blog.csdn.net/dghcs18/category_9658214.html)

7篇
[(L)](https://blog.csdn.net/dghcs18/category_9654296.html)

[(L)](https://blog.csdn.net/dghcs18/category_9654296.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-41)](https://blog.csdn.net/dghcs18/category_9654296.html)  [matlab](https://blog.csdn.net/dghcs18/category_9654296.html)  [(L)](https://blog.csdn.net/dghcs18/category_9654296.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9651075.html)

[(L)](https://blog.csdn.net/dghcs18/category_9651075.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-43)](https://blog.csdn.net/dghcs18/category_9651075.html)  [processing语言](https://blog.csdn.net/dghcs18/category_9651075.html)  [(L)](https://blog.csdn.net/dghcs18/category_9651075.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9401365.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401365.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-48)](https://blog.csdn.net/dghcs18/category_9401365.html)  [算法](https://blog.csdn.net/dghcs18/category_9401365.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401365.html)

[(L)](https://blog.csdn.net/dghcs18/category_9184350.html)

[(L)](https://blog.csdn.net/dghcs18/category_9184350.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-50)](https://blog.csdn.net/dghcs18/category_9184350.html)  [数据结构](https://blog.csdn.net/dghcs18/category_9184350.html)  [(L)](https://blog.csdn.net/dghcs18/category_9184350.html)

6篇
[(L)](https://blog.csdn.net/dghcs18/category_9240551.html)

[(L)](https://blog.csdn.net/dghcs18/category_9240551.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-58)](https://blog.csdn.net/dghcs18/category_9240551.html)  [洛谷](https://blog.csdn.net/dghcs18/category_9240551.html)  [(L)](https://blog.csdn.net/dghcs18/category_9240551.html)

77篇
[(L)](https://blog.csdn.net/dghcs18/category_9672884.html)

[(L)](https://blog.csdn.net/dghcs18/category_9672884.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)](https://blog.csdn.net/dghcs18/category_9672884.html)  [codeforces](https://blog.csdn.net/dghcs18/category_9672884.html)  [(L)](https://blog.csdn.net/dghcs18/category_9672884.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9401354.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401354.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-45)](https://blog.csdn.net/dghcs18/category_9401354.html)  [体育](https://blog.csdn.net/dghcs18/category_9401354.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401354.html)

[(L)](https://blog.csdn.net/dghcs18/category_9163749.html)

[(L)](https://blog.csdn.net/dghcs18/category_9163749.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-32)](https://blog.csdn.net/dghcs18/category_9163749.html)  [足球](https://blog.csdn.net/dghcs18/category_9163749.html)  [(L)](https://blog.csdn.net/dghcs18/category_9163749.html)

29篇
[(L)](https://blog.csdn.net/dghcs18/category_9587440.html)

[(L)](https://blog.csdn.net/dghcs18/category_9587440.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-7)](https://blog.csdn.net/dghcs18/category_9587440.html)  [经济与管理](https://blog.csdn.net/dghcs18/category_9587440.html)  [(L)](https://blog.csdn.net/dghcs18/category_9587440.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9401321.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401321.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-42)](https://blog.csdn.net/dghcs18/category_9401321.html)  [文学与音乐](https://blog.csdn.net/dghcs18/category_9401321.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401321.html)

[(L)](https://blog.csdn.net/dghcs18/category_9315123.html)

[(L)](https://blog.csdn.net/dghcs18/category_9315123.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-35)](https://blog.csdn.net/dghcs18/category_9315123.html)  [吉他](https://blog.csdn.net/dghcs18/category_9315123.html)  [(L)](https://blog.csdn.net/dghcs18/category_9315123.html)

10篇
[(L)](https://blog.csdn.net/dghcs18/category_9398895.html)

[(L)](https://blog.csdn.net/dghcs18/category_9398895.html)  [![](../_resources/5eba9634898886c5650982b3d0ef22ab.png)](https://blog.csdn.net/dghcs18/category_9398895.html)  [每日英语](https://blog.csdn.net/dghcs18/category_9398895.html)  [(L)](https://blog.csdn.net/dghcs18/category_9398895.html)

24篇
[(L)](https://blog.csdn.net/dghcs18/category_9165936.html)

[(L)](https://blog.csdn.net/dghcs18/category_9165936.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-55)](https://blog.csdn.net/dghcs18/category_9165936.html)  [学习方法](https://blog.csdn.net/dghcs18/category_9165936.html)  [(L)](https://blog.csdn.net/dghcs18/category_9165936.html)

9篇
[(L)](https://blog.csdn.net/dghcs18/category_9401358.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401358.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-57)](https://blog.csdn.net/dghcs18/category_9401358.html)  [人工智能](https://blog.csdn.net/dghcs18/category_9401358.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401358.html)

4篇
[(L)](https://blog.csdn.net/dghcs18/category_9401363.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401363.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-30)](https://blog.csdn.net/dghcs18/category_9401363.html)  [计算机视觉](https://blog.csdn.net/dghcs18/category_9401363.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401363.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401922.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401922.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-47)](https://blog.csdn.net/dghcs18/category_9401922.html)  [计算机语言](https://blog.csdn.net/dghcs18/category_9401922.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401922.html)

[(L)](https://blog.csdn.net/dghcs18/category_9653946.html)

[(L)](https://blog.csdn.net/dghcs18/category_9653946.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-5)](https://blog.csdn.net/dghcs18/category_9653946.html)  [C语言](https://blog.csdn.net/dghcs18/category_9653946.html)  [(L)](https://blog.csdn.net/dghcs18/category_9653946.html)

10篇
[(L)](https://blog.csdn.net/dghcs18/category_9161819.html)

[(L)](https://blog.csdn.net/dghcs18/category_9161819.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-31)](https://blog.csdn.net/dghcs18/category_9161819.html)  [C++编程](https://blog.csdn.net/dghcs18/category_9161819.html)  [(L)](https://blog.csdn.net/dghcs18/category_9161819.html)

127篇
[(L)](https://blog.csdn.net/dghcs18/category_9306512.html)

[(L)](https://blog.csdn.net/dghcs18/category_9306512.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-49)](https://blog.csdn.net/dghcs18/category_9306512.html)  [python](https://blog.csdn.net/dghcs18/category_9306512.html)  [(L)](https://blog.csdn.net/dghcs18/category_9306512.html)

14篇
[(L)](https://blog.csdn.net/dghcs18/category_9630631.html)

[(L)](https://blog.csdn.net/dghcs18/category_9630631.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-40)](https://blog.csdn.net/dghcs18/category_9630631.html)  [数据库](https://blog.csdn.net/dghcs18/category_9630631.html)  [(L)](https://blog.csdn.net/dghcs18/category_9630631.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9191618.html)

[(L)](https://blog.csdn.net/dghcs18/category_9191618.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-51)](https://blog.csdn.net/dghcs18/category_9191618.html)  [java](https://blog.csdn.net/dghcs18/category_9191618.html)  [(L)](https://blog.csdn.net/dghcs18/category_9191618.html)

22篇
[(L)](https://blog.csdn.net/dghcs18/category_9161889.html)

[(L)](https://blog.csdn.net/dghcs18/category_9161889.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-53)](https://blog.csdn.net/dghcs18/category_9161889.html)  [命令行](https://blog.csdn.net/dghcs18/category_9161889.html)  [(L)](https://blog.csdn.net/dghcs18/category_9161889.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9190593.html)

[(L)](https://blog.csdn.net/dghcs18/category_9190593.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-52)](https://blog.csdn.net/dghcs18/category_9190593.html)  [汇编语言](https://blog.csdn.net/dghcs18/category_9190593.html)  [(L)](https://blog.csdn.net/dghcs18/category_9190593.html)

[(L)](https://blog.csdn.net/dghcs18/category_9620994.html)

[(L)](https://blog.csdn.net/dghcs18/category_9620994.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-29)](https://blog.csdn.net/dghcs18/category_9620994.html)  [HTML](https://blog.csdn.net/dghcs18/category_9620994.html)  [(L)](https://blog.csdn.net/dghcs18/category_9620994.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9405807.html)

[(L)](https://blog.csdn.net/dghcs18/category_9405807.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-27)](https://blog.csdn.net/dghcs18/category_9405807.html)  [C#](https://blog.csdn.net/dghcs18/category_9405807.html)  [(L)](https://blog.csdn.net/dghcs18/category_9405807.html)

3篇
[(L)](https://blog.csdn.net/dghcs18/category_9401327.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401327.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-33)](https://blog.csdn.net/dghcs18/category_9401327.html)  [计算机软件&技术](https://blog.csdn.net/dghcs18/category_9401327.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401327.html)

7篇
[(L)](https://blog.csdn.net/dghcs18/category_9405710.html)

[(L)](https://blog.csdn.net/dghcs18/category_9405710.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-59)](https://blog.csdn.net/dghcs18/category_9405710.html)  [Unity](https://blog.csdn.net/dghcs18/category_9405710.html)  [(L)](https://blog.csdn.net/dghcs18/category_9405710.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9403202.html)

[(L)](https://blog.csdn.net/dghcs18/category_9403202.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-46)](https://blog.csdn.net/dghcs18/category_9403202.html)  [windows编程](https://blog.csdn.net/dghcs18/category_9403202.html)  [(L)](https://blog.csdn.net/dghcs18/category_9403202.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9323900.html)

[(L)](https://blog.csdn.net/dghcs18/category_9323900.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-56)](https://blog.csdn.net/dghcs18/category_9323900.html)  [爬虫](https://blog.csdn.net/dghcs18/category_9323900.html)  [(L)](https://blog.csdn.net/dghcs18/category_9323900.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9301471.html)

[(L)](https://blog.csdn.net/dghcs18/category_9301471.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-44)](https://blog.csdn.net/dghcs18/category_9301471.html)  [linux](https://blog.csdn.net/dghcs18/category_9301471.html)  [(L)](https://blog.csdn.net/dghcs18/category_9301471.html)

5篇
[(L)](https://blog.csdn.net/dghcs18/category_9323487.html)

[(L)](https://blog.csdn.net/dghcs18/category_9323487.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-54)](https://blog.csdn.net/dghcs18/category_9323487.html)  [Django](https://blog.csdn.net/dghcs18/category_9323487.html)  [(L)](https://blog.csdn.net/dghcs18/category_9323487.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9339963.html)

[(L)](https://blog.csdn.net/dghcs18/category_9339963.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-38)](https://blog.csdn.net/dghcs18/category_9339963.html)  [mathematica](https://blog.csdn.net/dghcs18/category_9339963.html)  [(L)](https://blog.csdn.net/dghcs18/category_9339963.html)

3篇
[(L)](https://blog.csdn.net/dghcs18/category_9158077.html)

[(L)](https://blog.csdn.net/dghcs18/category_9158077.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-34)](https://blog.csdn.net/dghcs18/category_9158077.html)  [电脑使用技巧](https://blog.csdn.net/dghcs18/category_9158077.html)  [(L)](https://blog.csdn.net/dghcs18/category_9158077.html)

55篇
[(L)](https://blog.csdn.net/dghcs18/category_9217936.html)

[(L)](https://blog.csdn.net/dghcs18/category_9217936.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-41)](https://blog.csdn.net/dghcs18/category_9217936.html)  [ps](https://blog.csdn.net/dghcs18/category_9217936.html)  [(L)](https://blog.csdn.net/dghcs18/category_9217936.html)

1篇
[(L)](https://blog.csdn.net/dghcs18/category_9157993.html)

[(L)](https://blog.csdn.net/dghcs18/category_9157993.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-36)](https://blog.csdn.net/dghcs18/category_9157993.html)  [我用IDE](https://blog.csdn.net/dghcs18/category_9157993.html)  [(L)](https://blog.csdn.net/dghcs18/category_9157993.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9401314.html)

[(L)](https://blog.csdn.net/dghcs18/category_9401314.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-26)](https://blog.csdn.net/dghcs18/category_9401314.html)  [数学](https://blog.csdn.net/dghcs18/category_9401314.html)  [(L)](https://blog.csdn.net/dghcs18/category_9401314.html)

[(L)](https://blog.csdn.net/dghcs18/category_9244126.html)

[(L)](https://blog.csdn.net/dghcs18/category_9244126.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-37)](https://blog.csdn.net/dghcs18/category_9244126.html)  [微积分](https://blog.csdn.net/dghcs18/category_9244126.html)  [(L)](https://blog.csdn.net/dghcs18/category_9244126.html)

2篇
[(L)](https://blog.csdn.net/dghcs18/category_9176514.html)

[(L)](https://blog.csdn.net/dghcs18/category_9176514.html)  [![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-28)](https://blog.csdn.net/dghcs18/category_9176514.html)  [线性代数](https://blog.csdn.net/dghcs18/category_9176514.html)  [(L)](https://blog.csdn.net/dghcs18/category_9176514.html)

10篇
[(L)](https://blog.csdn.net/dghcs18/category_9402835.html)

[(L)](https://blog.csdn.net/dghcs18/category_9402835.html)  [![](../_resources/5eba9634898886c5650982b3d0ef22ab.png)](https://blog.csdn.net/dghcs18/category_9402835.html)  [我](https://blog.csdn.net/dghcs18/category_9402835.html)  [(L)](https://blog.csdn.net/dghcs18/category_9402835.html)

12篇

![arrowDownWhite.png](../_resources/arrowDownWhite.png)

### **最新评论**

[什么时候scanf（"%d",&a）中不用写&](https://dghcs.blog.csdn.net/article/details/103493686#comments_24353942)

...  [m0_74954213:](https://blog.csdn.net/m0_74954213)  数组内容得是字符串才行吧

[mac下Vscode如何解决检测到 #include 错误。请更新 includePath](https://dghcs.blog.csdn.net/article/details/108032159#comments_24307108)

...  [少年正梦见狮子:](https://blog.csdn.net/ys0538)  根本没有任何用

[如何调整Ubuntu的字体大小？](https://dghcs.blog.csdn.net/article/details/104420127#comments_24258462)

...  [小小小小白白白白白白:](https://blog.csdn.net/m0_46528885)  误人子弟

[不能在csdn博客中发表评论怎么办](https://dghcs.blog.csdn.net/article/details/102071149#comments_24087047)

...  [llcifer:](https://blog.csdn.net/llcifer)  有的可以评论，有的是不是禁止评论了

[用十六进制表示浮点型常量](https://dghcs.blog.csdn.net/article/details/103967222#comments_23940749)

...  [udodvjib:](https://blog.csdn.net/udodvjib)  0x1.f4p+14什么意思

### **您愿意向朋友推荐“博客详情页”吗？**

![npsFeel1.png](../_resources/npsFeel1.png)
强烈不推荐
![npsFeel2.png](../_resources/npsFeel2.png)
不推荐
![npsFeel3.png](../_resources/npsFeel3.png)
一般般
![npsFeel4.png](../_resources/npsFeel4.png)
推荐
![npsFeel5.png](../_resources/npsFeel5.png)
强烈推荐

### **最新文章**

[vscode显示 code is already running](https://dghcs.blog.csdn.net/article/details/120575627)

[Essential C++ [1] [ Getting started]](https://dghcs.blog.csdn.net/article/details/120134182)

[Essential C++在线版读书笔记总目录](https://dghcs.blog.csdn.net/article/details/120133862)

[(L)](https://dghcs.blog.csdn.net/?type=blog&year=2021&month=10)[2021年](https://dghcs.blog.csdn.net/?type=blog&year=2021&month=10)[7篇](https://dghcs.blog.csdn.net/?type=blog&year=2021&month=10)

[(L)](https://dghcs.blog.csdn.net/?type=blog&year=2020&month=12)[2020年](https://dghcs.blog.csdn.net/?type=blog&year=2020&month=12)[289篇](https://dghcs.blog.csdn.net/?type=blog&year=2020&month=12)

[(L)](https://dghcs.blog.csdn.net/?type=blog&year=2019&month=12)[2019年](https://dghcs.blog.csdn.net/?type=blog&year=2019&month=12)[211篇](https://dghcs.blog.csdn.net/?type=blog&year=2019&month=12)

![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='2172'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='2173'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='2174'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2175'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='2176'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='2177'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='2178'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='2179'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='2180'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='2181'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='2182'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='2183'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2184'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='2185'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='2186'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='2187'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='2188'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='2189'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='2190'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='2191'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)