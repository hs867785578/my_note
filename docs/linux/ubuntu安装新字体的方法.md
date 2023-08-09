[参考这篇文章](https://blog.csdn.net/bitcarmanlee/article/details/79729634)

ubuntu里新装的terminator里，字体实在是不忍直视。尤其是字母i，跟别的字母挤在一起，根本就看不清楚。所以特意下载了一个苹果的Monaco字体来代替。
linux系统的字体文件放在/usr/share/fonts/目录
以及用户的/.fonts和/.local/share/fonts目录下，
第一个位置为系统所用用户共享，将字体安装到这个目录需要管理员权限；
后面两个位置则为当前登陆用户所有,安装字体到这个目录不需要管理员权限。

1.安装到 /usr/share/fonts/

wget https://github.com/fangwentong/dotfiles/raw/master/ubuntu-gui/fonts/Monaco.ttf
sudo mkdir -p /usr/share/fonts/custom
sudo mv Monaco.ttf /usr/share/fonts/custom
sudo chmod 744 /usr/share/fonts/custom/Monaco.ttf

sudo mkfontscale (支持字体放大缩小) #生成核心字体信息
sudo mkfontdir（支持字体加粗）
sudo fc-cache -fv（更新字体）



2.安装到 ~/.fonts/ (安装到 ~/.local/share/fonts 原理相同)
wget https://github.com/fangwentong/dotfiles/blob/ubuntu/fonts/Monaco.ttf?raw=true
mkdir -p ~/.fonts
mv Monaco.ttf ~/.fonts
fc-cache -vf  #刷新系统字体缓存
