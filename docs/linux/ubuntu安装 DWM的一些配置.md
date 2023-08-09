

[dwm个人桌面美化教程](https://www.bilibili.com/video/BV1pr4y1U78u/?spm_id_from=333.337.search-card.all.click&vd_source=d31a858cc26ae1ffa19e14058b339f40)

[谁再说linux图形界面不炫酷就拍他脸上，持续使用更新了几年的dwm有多帅(解说版)](https://www.bilibili.com/video/BV1FT411Z7A7/?spm_id_from=333.337.search-card.all.click&vd_source=d31a858cc26ae1ffa19e14058b339f40)

[ubuntu安装dwm](https://www.bilibili.com/video/BV1co4y1z7BT/?spm_id_from=333.337.search-card.all.click&vd_source=d31a858cc26ae1ffa19e14058b339f40)

[dwm 美化 - masimaro - 博客园 (cnblogs.com)](https://www.cnblogs.com/lanuage/p/15970577.html)

[dwm美化CSDN](https://blog.csdn.net/lanuage/article/details/123306485)


dwm中的重要概念：config.def.h和config.h的区别：

	1.config.h是编译时实际用的配置，如果config.h不存在，那么系统会从config.def.h复制一份config.h，并参与编译。
	2.config.def.h是默认配置，用户不要自行修改。补丁会打到config.def.h下，当然如果自己把config.def.h删了，也可以选择打到config.h下。THW CW大佬建议删掉config.def.h，只维护config.h,但是袁帅不建议这么做，袁帅建议用git来管理补丁，具体思路如下：
		1. 首先建立一个主分支，保持suckless最原始的配置，也就是config.def.h，删除config.h
		2. 先创建一个myconfig的分支来保存自己的config（使自己之前修改的config.h能够保存下来）git branch myconfig ，git checkout myconfig， cp config.h config.def.h，然后可以删除config.h并git add .
		3. 每当想打一个补丁xxx时，先git branch xxx分支，然后git checkout xxx，然后 patch < xxx.diff
		4. 然后git chekout master ，git merge xxx -m “xxx补丁”进行合并。
	袁帅这个思路可以利用git来解决冲突，是一个很好的思路。

dwm中的补丁：.diff格式，这个文件描述了对于config.def.h，要删除那些行，增加哪些行。

打补丁：pathch < ***.diff

删除：pathch -R < ***.diff

打补丁的教程（the cw）
[(# 【极简主义】打造Linux下精巧强大的终端：Simple Terminal （st） —— 【Suckless的极简主义01】](https://www.bilibili.com/video/BV1vE411i7H3/?spm_id_from=333.999.0.0&vd_source=d31a858cc26ae1ffa19e14058b339f40)


袁帅的教程
	
![[Pasted image 20230518194119.png]]

![[Pasted image 20230518194222.png]]


![[Pasted image 20230518194349.png]]




### 0.卸载gnome

	sudo apt remove gnome-shell
	sudo apt remove gnome
	sudo apt autoremove



### 1.依赖(xorg,xinit)
	sudo apt install xserver-xorg x11-xserver-utils
	sudo apt install libxft-dev libxcursor-dev libxrandr-dev libxinerama-dev gcc make xinit
	sudo add-apt-repository ppa:aslatter/ppa
	sudo apt install alacritty 
	 (可以通过ctrl + 和ctrl -来控制字体大小，需要在dwm/config.h中将st换成这个alacritty)

### 2.安装dwm

	mkdir ~/dwm_tool(主要装所有跟dwm相关的文件夹)
	cd dwm_tool
	git clone https://git.suckless.org/dwm
	cd dwm
	sudo make clean install


	touch ~/.xinitrc
	vim ~/.xinitrc
	exec dwm
	之后就可以通过startx进入dwm了

	
### 2.安装slstatus：元帅（也可以用xsetroot代替：thecw）
	cd ~/dwm_tool
	git clone https://git.suckless.org/slstatus
	cd slstatus
	sudo make clean install
	可以先测试一下slstatus能否运行
	vim ~/.xinitrc
	在exec dwm上边增加一行
	slstatus &
	
### 3. 安装st（也可以安装alacritty）,快捷键 alt shift enter

	cd ~/dwm_tool
	git clone https://git.suckless.org/st
	cd st
	sudo make clean install

### 4.安装dmenu（也可以安装rofi，他们都是启动器）快捷键 alt p

	cd ~/dwm_tool
	git clone https://git.suckless.org/dmenu
	cd dmenu
	sudo make clean install
	





### 4.安装ranger（文件管理器）
	sudo apt install ranger



安装字体：nerd font


### 3.安装后的简单配置
https://zhuanlan.zhihu.com/p/408552473

### 2.安装中文输入法（加入~/.xinitrc）


### 3.设置分辨率:xrandr（加入~/.xinitrc）


### 4.设置壁纸:feh（加入~/.xinitrc）


![[Pasted image 20230518144328.png]]



### 系统音量调节
	sudo apt install alsa-utils
	使用：amixer
	cd ~/dwm

修改config.h
![[Pasted image 20230518191902.png]]



### 屏幕亮度调节

	sudo apt install light

![[Pasted image 20230518193543.png]]



### 一些重要的配件：

	picom：能够实现终端的半透明效果
	xrandr：更改分辨率
	feh：修改壁纸；也可以用 nitrogen
	rofi：一个启动器（比dmenu好用）
	alacritty：终端（比st终端好用），支持放大缩小字体
	ranger：一个终端文件管理器
	alsa：声音管理（增大缩小）
	


### 一些重要的插件

	alpha：状态栏半透明
	autostart：启动dwm时自动启动的脚本
	barpadding：终端之间有一个间隔
	uselessgap：

自启动的一些实例：
1. 设置分辨率
2. 右下角状态栏
3. 自动更换壁纸
4. 窗口渲染器
5. 
![[Pasted image 20230518203127.png]]