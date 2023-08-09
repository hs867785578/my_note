[![20201124032511.png](../_resources/20201124032511.png)](https://www.csdn.net/)

- [博客](https://blog.csdn.net/)
- [下载](https://download.csdn.net/)
- [学习](https://edu.csdn.net/)
- [社区](https://bbs.csdn.net/)
- [GitCode](https://gitcode.net/?utm_source=csdn_toolbar)
- [云服务](https://dev-portal.csdn.net/welcome?utm_source=toolbar)
- [猿如意](https://devbit.csdn.net/?source=csdn_toolbar)

 [![2_qq_44640266](../_resources/2_qq_44640266)](https://blog.csdn.net/qq_44640266)

 [会员中心 ![20210918025138.gif](../_resources/20210918025138.gif)](https://mall.csdn.net/vip)

 [足迹](https://i.csdn.net/#/user-center/collection-list?type=1)

 [动态](https://blink.csdn.net/)

 [消息**](https://i.csdn.net/#/msg/index)

 [创作中心![20220627041202.png](../_resources/20220627041202.png)](https://mp.csdn.net/)

 [**发布**](https://mp.csdn.net/edit)

# vscode修改菜单栏字体、配置C++标准等

 ![original.png](../_resources/original.png)

 [Cc1924](https://blog.csdn.net/qq_42731705)  ![newCurrentTime2.png](../_resources/newCurrentTime2.png)  于 2022-07-15 20:14:49 发布  ![articleReadEyes2.png](../_resources/articleReadEyes2.png)  1561  [![tobarCollectionActive2.png](../_resources/tobarCollectionActive2.png)  已收藏   3]()

 分类专栏：  [Linux工具操作笔记](https://blog.csdn.net/qq_42731705/category_11047144.html)  文章标签：  [vscode](https://so.csdn.net/so/search/s.do?q=vscode&t=blog&o=vip&s=&l=&f=&viparticle=)  [ide](https://so.csdn.net/so/search/s.do?q=ide&t=blog&o=vip&s=&l=&f=&viparticle=)  [编辑器](https://so.csdn.net/so/search/s.do?q=%E7%BC%96%E8%BE%91%E5%99%A8&t=blog&o=vip&s=&l=&f=&viparticle=)

 [版权<div style="display: none;"></div>

]()

 [![resize,m_fixed,h_224,w_224](../_resources/resize,m_fixed,h_224,w_224-1)      Linux工具操作笔记  专栏收录该内容](https://blog.csdn.net/qq_42731705/category_11047144.html)

 58 篇文章  3 订阅

 [订阅专栏]()

 <div style="display: none;"></div>

### 文章目录

- [1.修改字体](https://blog.csdn.net/qq_42731705/article/details/125811610#1_2)
- [2.配置C++标准](https://blog.csdn.net/qq_42731705/article/details/125811610#2C_11)

# 1.修改字体

参考：[VSCODE修改菜单栏字体大小](https://blog.csdn.net/qq_41554005/article/details/119855889)

 `ctrl + shift + P`调出输入栏，输入`settings`，然后选择`JSON`，并且不要选`Default`，因为这个是默认的不能更改。

![1b02dc59b27248639a819e49e6ea880c.png](../_resources/1b02dc59b27248639a819e49e6ea880c.png)
在其中设置`window.zoomLevel`机会把所有字体都放大。然后字体就选择`monospace`，这个看着比较舒服。
 ![8635e33ff13d41cba4889db8058b1c21.png](../_resources/8635e33ff13d41cba4889db8058b1c21.png)

# 2.配置C++标准

默认是没有选择C++标准的，尽管在`CMakeLists.txt`中设置了C++标准，但是那个是给编译程序使用的，对于[Vscode](https://so.csdn.net/so/search?q=Vscode&spm=1001.2101.3001.7020)这个编辑器来说他是不知道的。所以如果使用了一些比较高版本的C++中的新特性，那么可能Vscode中就会有很多莫名奇妙的语法报错，比如`vscode`显示`应输入声明`、`未定义标识符`等错误，实际就是因为没有配置C++标准，Vscode进行语法检查的时候就认为是错误。

从上图可以看到，实际我已经配置完了，所以在我的配置JSON文件中已经有了。如果要重新配置，可以直接左下角打开`settings`的UI界面，输入`C++ standard`搜索，然后选择配置：

 ![3c5fc0d95a134f7d9270f2c0c498133e.png](../_resources/3c5fc0d95a134f7d9270f2c0c498133e.png)

[*VsCode* 设置窗口*菜单栏*显示*字体*大小](https://blog.csdn.net/u014627020/article/details/124785833)

[u014627020的博客](https://blog.csdn.net/u014627020)
![readCountWhite.png](../_resources/readCountWhite.png)3832

[ctrl+shift +p 在命令面板输入settings，选择首选项：打开设置 { "window.zoomLevel": 1, "editor.fontsize": 15, }

直接Ctrl+s就能看到效果 另外代码区的*字体*在下图设置，改完后看打开的代码文件*字体*大小直接能观察到效果
...](https://blog.csdn.net/u014627020/article/details/124785833)

[VS Code*修改*系统界面和编辑面板*字体*大小  热门推荐](https://chgl16.blog.csdn.net/article/details/85166528)

[不再更新，新博客 chgl16.space](https://blog.csdn.net/chenbetter1996)
![readCountWhite.png](../_resources/readCountWhite.png)7万+

[描述 Linux下新安装的VS Code可能*字体*很小，包括系统*字体*（标题栏，工具栏、状态栏）和编辑面板的*字体*很小。*修改*Ctrl + Shitf + p，输入 settings，选择打开那个JSON的系统*配置*文件

{ "editor.fontSize": 15, "window.zoomLevel": 1.5 }*修改*这两个值即可，第一个是编辑面板的，第二个是系统界面的。 ...](https://chgl16.blog.csdn.net/article/details/85166528)

 [ *vscode*中*修改**字体*,使用Fira Code_STR_Liang的博客](https://blog.csdn.net/STR_Liang/article/details/109181234)

 11-19

 [ "editor.fontLigatures":true,//这个控制是否启用*字体*连字,true启用,false不启用,这里选择启用 "editor.fontSize":14,//设置*字体*大小,这个不多说都明白 "editor.fontWeight":"normal",//这个设置*字体*粗细,可选normal,bold,"100"~"...](https://blog.csdn.net/STR_Liang/article/details/109181234)

[*vscode*中*c++*的*配置*  最新发布](https://blog.csdn.net/silent_missile/article/details/127405115)

[silent_missile的博客](https://blog.csdn.net/silent_missile)
![readCountWhite.png](../_resources/readCountWhite.png)491

[用*vscode*写*C++*代码，涉及到的一些*配置*](https://blog.csdn.net/silent_missile/article/details/127405115)

[*VSCode*  *修改*界面*字体* 代码*字体* 终端*字体*](https://blog.csdn.net/jiang_huixin/article/details/124556970)

[jianghuixin](https://blog.csdn.net/jiang_huixin)
![readCountWhite.png](../_resources/readCountWhite.png)4228

[界面*字体**VSCode* 默认不支持*修改*界面*字体*, 但可以通过插件 "Customize UI" 来*修改*1. 安装插件 "Customize UI" 安装完成后, 按照右下角的提示操作, 最后重启 *VSCode*2. *修改* "Customize UI" *配置*ubuntu 中安装 "Fira Code" *字体*代码*字体*代码*字体*推荐使用 "JetBrains Mono" 终端*字体*终端*字体*推荐使用 "Cascadia Code"......](https://blog.csdn.net/jiang_huixin/article/details/124556970)

[*VSCode*的一些基本设置：*修改**字体*大小、更换主题颜色、中文](https://blog.csdn.net/mantou_riji/article/details/123261723)

[mantou_riji的博客](https://blog.csdn.net/mantou_riji)
![readCountWhite.png](../_resources/readCountWhite.png)7738
[1.中文插件
搜索Chinese进行安装
2.*修改*代码*字体*大小

ctrl+shift+“+”可以使*VSCode*的侧边*字体*变大 ctrl+shift+“-”可以使*VSCode*的侧边*字体*变小 4.*修改*主题颜色 文件->首选项->颜色主题](https://blog.csdn.net/mantou_riji/article/details/123261723)

[*VSCode*  *修改**字体*为JetBrains Mono（Java的代码样式）](https://blog.csdn.net/m0_50360903/article/details/124362080)

[m0_50360903的博客](https://blog.csdn.net/m0_50360903)
![readCountWhite.png](../_resources/readCountWhite.png)3435

[*VsCode**字体**修改*，*VsCode**修改**字体*为Java*字体*](https://blog.csdn.net/m0_50360903/article/details/124362080)

[*VSCode**修改**字体*的方法](https://blog.csdn.net/u010280030/article/details/115435825)

[一杯原谅绿茶的博客](https://blog.csdn.net/u010280030)
![readCountWhite.png](../_resources/readCountWhite.png)1万+
[*修改*方法：
1. 首先打开设置
右上角“文件”—“首选项”—“设置”
或者按ctrl + ，
2. 搜索*字体*，找到Editor：Font Family*修改*第二个就可以改成你喜欢的*字体*名字了，记得带上引号
默认设置是Consolas, 'Courier New', monospace*字体*的名字：
很多人安装*字体*后却不知道*字体*的确切名字，比如思源黑体，还是思源黑体 CN，抑或是SourceHanSansCN（文件名）？

方法是点开win10设置—*字体*在里面找到*字体*的标题名称，.](https://blog.csdn.net/u010280030/article/details/115435825)

[*VSCode* 如何*修改**字体*](https://blog.csdn.net/qq_41841099/article/details/88610143)

[nepleo](https://blog.csdn.net/qq_41841099)
![readCountWhite.png](../_resources/readCountWhite.png)1万+

[*VSCode* 如何*修改**字体*点开 文件-&gt;首选项-&gt;设置 在页面中可以发现 Font Size 即可*修改*18 看起来比较舒服一点](https://blog.csdn.net/qq_41841099/article/details/88610143)

[*vscode*编程*字体*设置与*修改*](https://blog.csdn.net/weixin_65402230/article/details/126912152)

[聪明勇敢有力气的博客](https://blog.csdn.net/weixin_65402230)
![readCountWhite.png](../_resources/readCountWhite.png)764

[*vscode**字体**修改*](https://blog.csdn.net/weixin_65402230/article/details/126912152)

[*VScode*上*修改**字体*样式](https://blog.csdn.net/HouRuoTong/article/details/110285669)

[HouRuoTong的博客](https://blog.csdn.net/HouRuoTong)
![readCountWhite.png](../_resources/readCountWhite.png)2万+
[*VScode*上*修改**字体*样式 在更换*字体*样式前需要先安装*字体*，下面的文件需要自提
链接：https://pan.baidu.com/s/1lCaG4NNQZysMfKT6jdMJ_Q 提取码：4785
下载后就可以根据下面的步骤来更换 1.打开*VScode*如下图
2.进入设置页面
3.点击文本*编辑器*->找到*字体*->点击
4.找到并打开*字体*里的setting.json文件

5.在setting.json里添加代码如下图所示*修改*后如果也想*修改*终端*字体*，就在设置里搜索font把有关的f](https://blog.csdn.net/HouRuoTong/article/details/110285669)

[VS 和 VS Code 更换*字体*](https://fightsyj.blog.csdn.net/article/details/110144139)
[fightsyj的博客](https://blog.csdn.net/fightsyj)
![readCountWhite.png](../_resources/readCountWhite.png)2万+
[VS
1、工具->选项打开选项窗口，定位到环境下面的*字体*和颜色：
2、在显示其设置下面选择文本*编辑器*，在*字体*下面选择要更换的*字体*：
3、点击确定应用即可，效果如下：
...](https://fightsyj.blog.csdn.net/article/details/110144139)

[*vsCode**修改**字体*为JetBrains Mono (PyCharm默认*字体*)](https://shixuebin.blog.csdn.net/article/details/111314960)

[https://shixuebin.gitee.io/hexo-blog/](https://blog.csdn.net/weixin_40639095)
![readCountWhite.png](../_resources/readCountWhite.png)6421

[效果*字体*下载 JetBrians 官网下载：https://www.jetbrains.com/lp/mono/ 蓝奏云下载：https://bill.lanzous.com/izk2zeb9g7c 安装*字体*解压文件后打开ttf文件，点击安装

随后可以在控制面板里面看见对应*字体*VS Code*修改*文件->首选项->设置->文本*编辑器*->*字体*在settings.json中加入以下代码： "editor.fontFamily": "JetBrains Mono, 'C.](https://shixuebin.blog.csdn.net/article/details/111314960)

[*vscode*更换*字体*](https://blog.csdn.net/Allen_Spring/article/details/118890402)
[祁连山下的小牧童的博客](https://blog.csdn.net/Allen_Spring)
![readCountWhite.png](../_resources/readCountWhite.png)2661

[以Fria Code*字体*为例，首先下载该*字体*： (Fira Code *字体*的下载地址)[https://github.com/tonsky/FiraCode] 下载后解压，如下图： Windows用户，安装ttf文件夹中的*字体*双击*字体*文件，点击安装：

全部安装完，在*vscode*中的Font Family中，添加“Fira Code”
重启*vscode*，可以看到*字体*已改变。
...](https://blog.csdn.net/Allen_Spring/article/details/118890402)
[*vscode*设置*字体*](https://blog.csdn.net/SoWhatWorld/article/details/111082403)
[SoWhatWorld的博客](https://blog.csdn.net/SoWhatWorld)
![readCountWhite.png](../_resources/readCountWhite.png)2万+

[*vscode*设置*字体**vscode*设置*字体*查看*vscode*当前的*字体*github搜索自己喜欢的*字体*设置*vscode**字体**配置**vscode*设置*字体*安装下载完成后总感觉*字体*不好看，想换别的*字体*，怎么办，只需要如下几步即可搞定 查看*vscode*当前的*字体*** ** 如上图，我得*vscode**字体*是已经设置过的，*vscode*默认*字体*是Consolas格式的，我个人不太喜欢，所以就从网上下载别的*字体*。 github搜索自己喜欢的*字体*比如我下载的就是Hack*字体*，github上直接搜索 Hack*字体*是直接可以下载安装](https://blog.csdn.net/SoWhatWorld/article/details/111082403)

[*VSCODE**修改**菜单栏**字体*大小](https://ddelephant.blog.csdn.net/article/details/119855889)

[呆呆象呆呆的博客](https://blog.csdn.net/qq_41554005)
![readCountWhite.png](../_resources/readCountWhite.png)8896

[1 问题描述 有的时候*菜单栏*的字号特别小，包括系统*字体*（标题栏，工具栏、状态栏）和编辑面板的*字体*很小。另外编辑框中代码的*字体*的*修改*可以通过之前的一篇文章进行*修改*。 2 问题解决 2.1 直接打开settings.josn 打开设置的命令面板或者直接使用快捷键ctrl+shift+P

在命令面板输入settings，选择首选项：打开设置
2.2 *修改**配置*文件 在JSON文件中添加如下语句 "editor.fontSize": 15, "window.zoomLevel": 1.5,
这两个参数的功能分](https://ddelephant.blog.csdn.net/article/details/119855889)

[*Vscode*——调整左侧菜单*字体*大小](https://blog.csdn.net/Kiruthika/article/details/124868656)

[月来better](https://blog.csdn.net/Kiruthika)
![readCountWhite.png](../_resources/readCountWhite.png)5340

[不知道键盘按啥了，*vscode*开发工具上左侧目录*字体*特别小，看着真的太难受了 调整左侧菜单*字体*大小 1、按ctrl+shift+p 2、点击打开设置

3、在settings.json文件中加入 “window.zoomLevel”: 1.2 "window.zoomLevel": 1.2
...](https://blog.csdn.net/Kiruthika/article/details/124868656)

[将 Linux 上的*VSCode*  *字体*更改为 Consolas](https://blog.csdn.net/weixin_51396240/article/details/124376781)

[Nebula的博客](https://blog.csdn.net/weixin_51396240)
![readCountWhite.png](../_resources/readCountWhite.png)777

[linux 下安装 Consolas*字体*，供 *VSCode*使用](https://blog.csdn.net/weixin_51396240/article/details/124376781)

[*vscode**修改*整个程序的*字体*（包括左侧文件列表）](https://blog.csdn.net/weixin_44667590/article/details/126155002)

[weixin_44667590的博客](https://blog.csdn.net/weixin_44667590)
![readCountWhite.png](../_resources/readCountWhite.png)216

[*vscode**修改*程序*字体*大小](https://blog.csdn.net/weixin_44667590/article/details/126155002)

[*VSCODE* 设置 *菜单栏**字体*大小](https://blog.csdn.net/weixin_42786501/article/details/124105346)

[weixin_42786501的博客](https://blog.csdn.net/weixin_42786501)
![readCountWhite.png](../_resources/readCountWhite.png)1756

[*VSCODE* 设置*菜单栏**字体*大小](https://blog.csdn.net/weixin_42786501/article/details/124105346)

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

 ©️2022 CSDN  皮肤主题：深蓝海洋   设计师：CSDN官方博客    [返回首页](https://blog.csdn.net/)

- [关于我们](https://www.csdn.net/company/index.html#about)

- [招贤纳士](https://www.csdn.net/company/index.html#recruit)

- [商务合作](https://marketing.csdn.net/questions/Q2202181741262323995)

- [寻求报道](https://marketing.csdn.net/questions/Q2202181748074189855)

- ![tel.png](../_resources/tel.png)  400-660-0108

- ![email.png](../_resources/email.png)  [kefu@csdn.net](https://blog.csdn.net/qq_42731705/article/details/125811610mailto:webmaster@csdn.net)

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

 [![3_qq_42731705](../_resources/3_wq1129)](https://blog.csdn.net/qq_42731705)

 [Cc1924](https://blog.csdn.net/qq_42731705)

 码龄4年    [![nocErtification.png](../_resources/nocErtification.png) 暂无认证](https://i.csdn.net/#/uc/profile?utm_source=14998968)

32万+
访问

[![blog5.png](../_resources/blog5.png)](https://blog.csdn.net/blogdevteam/article/details/103478461)

等级

3943
积分

419
粉丝

273
获赞

68
评论

1681
收藏

 ![qiandao1@240.png](../_resources/qiandao1@240.png)

 ![51_create.png](../_resources/51_create.png)

 ![chizhiyiheng@240.png](../_resources/chizhiyiheng@240.png)

 ![github@240.png](../_resources/github@240.png)

 ![1024up@240.png](../_resources/1024up@240.png)

 ![02d34b42a3ee476fb50850304ab67017.png](../_resources/02d34b42a3ee476fb50850304ab67017.png)

 ![chaoji1024@240.png](../_resources/chaoji1024@240.png)

 ![qixiebiaobing4@240.png](../_resources/qixiebiaobing4@240.png)

 ![blinknewcomer@240.png](../_resources/blinknewcomer@240.png)

 ![yuedu30@240.png](../_resources/yuedu30@240.png)

 [私信](https://im.csdn.net/chat/qq_42731705)

 [关注]()

 [![csdn-sou.png](../_resources/csdn-sou.png)]()

### 热门文章

- [Keil警告：warning: #223-D: function “xxx“ declared implicitly解决![readCountWhite.png](../_resources/readCountWhite.png)20957](https://blog.csdn.net/qq_42731705/article/details/115270040)
- [线性系统大作业——1.一阶倒立摆建模与控制系统设计![readCountWhite.png](../_resources/readCountWhite.png)9650](https://blog.csdn.net/qq_42731705/article/details/122464642)
- [FOC——13.电流采样与运放电路![readCountWhite.png](../_resources/readCountWhite.png)9227](https://blog.csdn.net/qq_42731705/article/details/118770567)
- [linux中sed -i命令修改文件内容、在文件中插入行、删除文件中删除行![readCountWhite.png](../_resources/readCountWhite.png)8550](https://blog.csdn.net/qq_42731705/article/details/123963410)
- [Arduino烧录程序出现avrdude: stk500_getsync() attempt x(1-10) of 10错误![readCountWhite.png](../_resources/readCountWhite.png)7691](https://blog.csdn.net/qq_42731705/article/details/116838845)

### 分类专栏

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-10)   ROS](https://blog.csdn.net/qq_42731705/category_11378338.html)  19篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   算法刷题](https://blog.csdn.net/qq_42731705/category_12022849.html)  67篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-5)   SLAM](https://blog.csdn.net/qq_42731705/category_11383626.html)  59篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   深度学习](https://blog.csdn.net/qq_42731705/category_11517169.html)  4篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-2)   C++](https://blog.csdn.net/qq_42731705/category_11390374.html)  59篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   计算机网络](https://blog.csdn.net/qq_42731705/category_11709389.html)  6篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   作业记录](https://blog.csdn.net/qq_42731705/category_11589365.html)  4篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   暂存](https://blog.csdn.net/qq_42731705/category_11743941.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   git/github](https://blog.csdn.net/qq_42731705/category_11536145.html)  2篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   数学基础](https://blog.csdn.net/qq_42731705/category_11548622.html)  3篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-6)   ceres学习笔记](https://blog.csdn.net/qq_42731705/category_11402129.html)  5篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   g2o](https://blog.csdn.net/qq_42731705/category_11403325.html)  1篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-4)   理论基础](https://blog.csdn.net/qq_42731705/category_11382967.html)  1篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-1)   CS231N](https://blog.csdn.net/qq_42731705/category_11219103.html)  5篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-8)   电机笔记](https://blog.csdn.net/qq_42731705/category_11047145.html)  21篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-22)   Python笔记](https://blog.csdn.net/qq_42731705/category_11047143.html)  6篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-23)   Linux工具操作笔记](https://blog.csdn.net/qq_42731705/category_11047144.html)  58篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-21)   单片机嵌入式笔记](https://blog.csdn.net/qq_42731705/category_11047149.html)  9篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-25)   杂项](https://blog.csdn.net/qq_42731705/category_11047146.html)  7篇

- [   ![resize,m_fixed,h_64,w_64](../_resources/resize,m_fixed,h_64,w_64-24)   树莓派/OpenCV/](https://blog.csdn.net/qq_42731705/category_11088128.html)  8篇

 [![arrowDownWhite.png](../_resources/arrowDownWhite.png)]()

### 最新评论

- [线性系统大作业——2.二阶倒立摆建模与控制系统设计（上）](https://blog.csdn.net/qq_42731705/article/details/122464894#comments_24362129)

...  [努力我要努力:](https://blog.csdn.net/weixin_44891999)  解决了，同学

- [位姿图优化小记2021.10.18](https://blog.csdn.net/qq_42731705/article/details/120824510#comments_24276544)

...  [quanquandage:](https://blog.csdn.net/quanquandage)  请问一下博主为啥视频挂了呢 以及博主的这个工作发论文了吗~

- [线性系统大作业——1.一阶倒立摆建模与控制系统设计](https://blog.csdn.net/qq_42731705/article/details/122464642#comments_24243902)

...  [风有风的风情:](https://blog.csdn.net/qq_43680156)  sys = (A-b*K)*x + Ke*u ; % 注意这里的u就是x(1)的观测误差e(1)，博主，请问这个微分方程怎么理解呀

- [LIO-SAM论文与代码阅读笔记（一）论文阅读](https://blog.csdn.net/qq_42731705/article/details/127833557#comments_24140621)

...  [programmer_ada:](https://blog.csdn.net/community_717)  你好，CSDN 开始提供 #论文阅读# 的列表服务了。请看：https://blog.csdn.net/nav/advanced-technology/paper-reading 。如果你有更多需求，请来这里 https://gitcode.net/csdn/csdn-tags/-/issues/34 给我们提。

- [linux服务器非root用户安装tree命令](https://blog.csdn.net/qq_42731705/article/details/116259663#comments_24102303)

...  [MO__YE:](https://blog.csdn.net/MO__YE)  单词拼错了![014.png](../_resources/014.png)

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

- [利用ROS中image_transport将sensor_msgs/CompressedImage转为sensor_msgs/Image](https://blog.csdn.net/qq_42731705/article/details/128163502)

- [力扣hot100——第6天：32最长有效括号、33搜索旋转排序数组、34在排序数组中查找元素的第一个和最后一个位置](https://blog.csdn.net/qq_42731705/article/details/128156365)

- [力扣hot100——第5天：22括号生成、23合并K个升序链表、31下一个排列](https://blog.csdn.net/qq_42731705/article/details/128156328)

2022

 [12月5篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=12)

 [11月30篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=11)

 [10月28篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=10)

 [09月16篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=09)

 [08月2篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=08)

 [07月26篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=07)

 [06月5篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=06)

 [05月9篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=05)

 [04月35篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=04)

 [03月40篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=03)

 [02月1篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=02)

 [01月5篇](https://blog.csdn.net/qq_42731705?type=blog&year=2022&month=01)

[2021年134篇](https://blog.csdn.net/qq_42731705?type=blog&year=2021&month=12)

![1.png](../_resources/1.png)

### 目录

1. [文章目录](https://blog.csdn.net/qq_42731705/article/details/125811610#t0)
2. [1.修改字体](https://blog.csdn.net/qq_42731705/article/details/125811610#t1)
3. [2.配置C++标准](https://blog.csdn.net/qq_42731705/article/details/125811610#t2)

 ![](data:image/svg+xml,%3csvg%20xmlns='http://www.w3.org/2000/svg'%20aria-hidden='true'%20style='position:%20absolute%3b%20width:%200px%3b%20height:%200px%3b%20overflow:%20hidden%3b'%20data-evernote-id='1922'%20class='js-evernote-checked'%3e%3csymbol%20id='sousuo'%20viewBox='0%200%201024%201024'%20data-evernote-id='1923'%20class='js-evernote-checked'%3e%3cpath%20d='M719.6779726%20653.55865555l0.71080936%200.70145709%20191.77828505%20191.77828506c18.25658185%2018.25658185%2018.25658185%2047.86273439%200%2066.12399318-18.26593493%2018.26125798-47.87208744%2018.26125798-66.13334544%200l-191.77828505-191.77828506c-0.2338193-0.2338193-0.4676378-0.4676378-0.69678097-0.71081014-58.13206223%2044.25257003-130.69075187%2070.51978897-209.38952657%2070.51978894C253.06424184%20790.19776156%2098.14049639%20635.27869225%2098.14049639%20444.17380511S253.06424184%2098.14049639%20444.16912898%2098.14049639c191.10488633%200%20346.02863258%20154.92374545%20346.02863259%20346.02863259%200%2078.6987747-26.27189505%20151.25746514-70.51978897%20209.38952657z%20m-275.50884362%2043.11621045c139.45428506%200%20252.50573702-113.05145197%20252.50573702-252.50573702s-113.05145197-252.50573702-252.50573702-252.50573783-252.50573702%20113.05145197-252.50573783%20252.50573783%20113.05145197%20252.50573702%20252.50573783%20252.50573702z'%20data-evernote-id='1924'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogo_'%20viewBox='0%200%204096%201024'%20data-evernote-id='1925'%20class='js-evernote-checked'%3e%3cpath%20d='M1234.16069807%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3010.8325562%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2141.37671774%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z'%20fill='%23262626'%20data-evernote-id='1926'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1109.8678928%20870.30336371c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157302-372.90540663C385.78470347%20268.40769434%20659.36382925%20126.08500985%20958.9081404%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20fill='%23CA0C16'%20data-evernote-id='1927'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gonggong_csdnlogodanse_'%20viewBox='0%200%204096%201024'%20data-evernote-id='1928'%20class='js-evernote-checked'%3e%3cpath%20d='M1229.41995733%20690.46341551c62.96962316%2023.02318413%20194.30703694%2045.91141406%20300.51598128%2045.91141406%20114.44114969%200%20178.13952547-31.68724287%20183.2407937-80.86454822%204.642424-44.8587714-42.21366937-50.93170978-171.44579784-81.53931916-178.57137886-43.77913792-292.49970264-111.55313011-281.32549604-219.86735976%2012.9825927-125.75031047%20181.27046257-220.78504823%20439.49180199-220.78504822%20125.88526465%200%20247.93783044%208.87998544%20311.17736197%2029.60894839l-21.7006331%20158.57116851c-41.05306337-14.27815288-198.1937175-34.11641822-304.48363435-34.11641822-107.7744129%200-163.56447339%2033.90049151-167.42416309%2071.06687432-4.85835069%2047.04502922%2051.14763648%2049.23128703%20191.14910897%2086.50563321%20189.58364043%2048.09767188%20272.47250144%20115.81768239%20261.6221849%20220.81203906-12.71268432%20123.51007099-164.13128096%20228.53141851-466.48263918%20228.53141851-125.85827383%200-234.33444849-22.96920244-294.09216204-45.93840492l19.730302-157.86940672zM3006.09181546%20172.75216735c688.40130256-129.79893606%20747.80813523%20103.42888812%20726.53935551%20309.80082928l-40.08139323%20381.78539207h-218.51781789l36.57258439-348.20879061c7.90831529-76.68096846%2057.13960232-226.66905073-180.54170997-221.05495659-82.26807176%201.99732195-123.05122675%2013.2794919-123.05122677%2013.27949188s-7.15257186%2092.65954408-15.81663059%20161.13529804l-41.43093509%20394.84895728h-214.3072473l42.53755943-389.15389062%2028.09746151-302.43233073z%20m-869.48282929-18.05687008c49.12332368-5.34418577%20124.58970448-10.76934404%20228.45044598-10.76934405%20173.38913812%200%20313.57954648%2030.17575597%20400.38207891%2093.63121421%2077.94953781%2059.16391512%20129.82592689%20154.95439631%20115.4668015%20293.74128117-13.25250106%20129.15115596-80.405704%20219.57046055-178.16651631%20275.4954752-89.44763445%2052.74009587-202.16137055%2075.27744492-371.66382812%2075.27744493-99.94707012%200-195.27870708-5.39816743-267.77609576-16.14052064L2136.635977%20154.69529727z%20m143.26736381%20569.85754561c16.70732823%203.23890047%2038.67786969%206.45081009%2081.99816339%206.45081009%20173.44311979%200%20295.7386031-85.23706385%20308.01943403-205.07638097%2017.84094339-173.2271931-90.63523129-233.79463176-273.39018992-232.74198912-23.67096422%200-56.57279475%200-73.98188473%203.1849188l-42.6725136%20428.15565036z%20m-1174.74919792%20145.75052083c-41.10704503%2014.25116203-126.26313639%2023.96786342-245.23874671%2023.96786342-342.13585224%200-526.8071603-160.59548129-504.97157303-372.90540663C381.04396273%20268.40769434%20654.62308851%20126.08500985%20954.16739966%20126.08500985c116.00661824%200%20184.32042718%209.33882968%20248.31570215%2024.99351522l-20.5400271%20170.42014604c-42.56455024-14.33213455-142.32268451-27.50366309-223.07926938-27.50366311-176.25016686%200-325.94134993%2052.49717834-343.10752238%20218.57179958-15.30380469%20148.50358623%2089.7715245%20219.48948804%20288.04621451%20219.48948804%2069.0155707%200%20170.77102691-9.8786464%20217.81605614-24.15679928l-16.49140154%20162.40386737z'%20data-evernote-id='1929'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='xieboke1'%20viewBox='0%200%201024%201024'%20data-evernote-id='1930'%20class='js-evernote-checked'%3e%3cpath%20d='M204.70021457%20751.89799169h657.99199211a33.6932867%2033.6932867%200%200%201%200%2067.33536736H163.68452703a33.53966977%2033.53966977%200%200%201-18.74125054-5.68382181c-18.63883902-9.4218307-18.17798882-29.44322156-15.20806401-39.17228615C199.0675982%20570.27171976%20309.41567149%20409.58853908%20435.38145354%20290.12586836A243.22661203%20243.22661203%200%200%201%20536.97336934%20234.20935065c138.10150976-33.79569759%20228.3257813-29.95527721%20318.60125827-28.52152054-17.15387692%2020.48224105-36.20236071%2041.6301547-57.29906892%2062.93168529-3.1747472%203.22595323-164.67721739%2019.91897936-187.97576692%2047.05794871-23.29854894%2027.13896932%20129.60138005%207.37360691%20125.19769798%2011.11161576-21.6599699%2018.33160576-44.90731339%2036.4071831-69.94685287%2053.8682939-4.50609297%203.1747472-149.52035944-0.35843931-174.61110436%2027.85584737-25.19315641%2028.16308124%20101.89914903%2018.12678338%2096.0617103%2021.40394206-67.43777825%2037.63611797-125.96578207%2064.62147036-212.70807253%2093.8086635-57.65750823%2019.4069231-121.8181284%20133.13456658-146.5504346%20179.06599187a435.75967738%20435.75967738%200%200%200-23.04252112%2049.10617311z'%20fill='%23CA0C16'%20data-evernote-id='1931'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='gitchat'%20viewBox='0%200%201024%201024'%20data-evernote-id='1932'%20class='js-evernote-checked'%3e%3cpath%20d='M892.08971773%20729.08552746h-108.597062v-162.89559374H403.40293801v-108.59706198h488.68677972v271.49265572z%20m-651.58237345%2054.298531V783.49265572h488.68678045v108.59706201H131.91028227V131.91028227h760.17943546v217.19412473h-108.597062V240.50734428H240.50734428v542.87671418z%20m542.98531145%200h108.597062v108.59706199h-108.597062v-108.59706199z'%20fill='%23FF9100'%20data-evernote-id='1933'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='1934'%20class='js-evernote-checked'%3e%3cpath%20d='M1061.51168438%20433.79527648A78.51879902%2078.51879902%200%201%201%201129.35192643%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H319.35199503c-41.30088817%200-76.00619753-28.81639958-80.717325-66.97653526L189.01078861%20472.74060007H187.12633728a78.51879902%2078.51879902%200%201%201%2067.76172401-38.86680556l193.31328323%20119.81968805%20158.13686148-336.06046024A78.5973179%2078.5973179%200%200%201%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607z'%20fill='%23FDD840'%20data-evernote-id='1935'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1050.8331274%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643684%2067.133573-80.79584389%2067.13357302H659.02432018C658.47468805%20793.25433807%20658.23913228%20505.32590231%20658.23913228%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685077%20137.721974l158.13686147%20335.82490362%20193.54883986-119.89820607A78.51879902%2078.51879902%200%200%201%201050.8331274%20394.22180104z'%20fill='%23FFBE00'%20data-evernote-id='1936'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='toolbar-m-memberhead'%20viewBox='0%200%201303%201024'%20data-evernote-id='1937'%20class='js-evernote-checked'%3e%3cpath%20d='M1062.74839935%20433.79527648A78.51879902%2078.51879902%200%201%201%201130.58864141%20472.74060007h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H320.58871c-41.30088817%200-76.00619753-28.81639958-80.71732499-66.97653526L190.24750358%20472.74060007H188.36305226a78.51879902%2078.51879902%200%201%201%2067.761724-38.86680556l193.31328324%20119.81968805%20158.13686147-336.06046024A78.5973179%2078.5973179%200%200%201%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607z'%20fill='%23D6D6D6'%20data-evernote-id='1938'%20class='js-evernote-checked'%3e%3c/path%3e%3cpath%20d='M1052.06984238%20394.22180104a78.51879902%2078.51879902%200%201%201%2078.51879903%2078.51879903h-1.80593246l-48.05350474%20403.97922198c-4.55409058%2038.16013652-39.41643685%2067.133573-80.79584389%2067.13357302H660.26103515C659.71140302%20793.25433807%20659.47584726%20505.32590231%20659.47584726%2080.14660493a78.51879902%2078.51879902%200%200%201%2051.58685076%20137.721974l158.13686148%20335.82490362%20193.54883985-119.89820607A78.51879902%2078.51879902%200%200%201%201052.06984238%20394.22180104z'%20fill='%23C1C1C1'%20data-evernote-id='1939'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3csymbol%20id='csdnc-upload'%20viewBox='0%200%201024%201024'%20data-evernote-id='1940'%20class='js-evernote-checked'%3e%3cpath%20d='M216.37466416%20723.16095396v84.46438188h591.25067168v-84.46438188c0-23.32483876%2018.90735218-42.23219094%2042.23219093-42.23219021s42.23219094%2018.90735218%2042.23219096%2042.23219021v84.46438188c0%2046.64967827-37.81470362%2084.46438188-84.46438189%2084.46438189H216.37466416c-46.64967827%200-84.46438188-37.81470362-84.46438189-84.4643819v-84.46438187c0-23.32483876%2018.90735218-42.23219094%2042.23219096-42.23219021s42.23219094%2018.90735218%2042.23219094%2042.23219021zM469.76780906%20275.55040991L246.55378774%20499.53305726a42.30820888%2042.30820888%200%200%201-59.99082735%200c-16.56346508-16.62259056-16.56346508-43.57095155%200-60.19354139L480.51167818%20144.38144832A42.21952103%2042.21952103%200%200%201%20512%20131.93984464a42.20262858%2042.20262858%200%200%201%2031.48409853%2012.44160369l293.95294108%20294.95806754c16.56346508%2016.62259056%2016.56346508%2043.57095155%200%2060.19354139a42.30820888%2042.30820888%200%200%201-59.99082735%200L554.23219094%20275.55040991V680.92876375c0%2023.32483876-18.90735218%2042.23219094-42.23219094%2042.23219021s-42.23219094-18.90735218-42.23219094-42.23219021V275.55040991z'%20data-evernote-id='1941'%20class='js-evernote-checked'%3e%3c/path%3e%3c/symbol%3e%3c/svg%3e)