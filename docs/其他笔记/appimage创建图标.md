有的时候Linux的一些软件是不带桌面图标的，可能直接通过命令启动程序，但是习惯了GUI界面的话就需要创建一个图标来加快效率了。

---

首先明白图标展示的一些原理：

- 应用要能展示相应的图标需要有 `.desktop` 的文件
- 所有的图标文件应该放在 `/usr/share/applications` 文件夹下才能生效
- AppImage文件运行的本质是将AppImage文件解压到`/tmp` 下运行，而在这个临时文件夹下我们可以查看到该软件相关的文件，包括了`desktop` 文件和`icon` 文件。  
    运行AppImage文件的时候可以查看到文件所在位置如下图：

[![运行AppImage显示的路径](https://img2023.cnblogs.com/blog/2035938/202212/2035938-20221218161016995-1727728935.png)](https://img2023.cnblogs.com/blog/2035938/202212/2035938-20221218161016995-1727728935.png)


---

看完原理我们需要实现的就是去写一个desktop文件，由于尝试过直接将/tmp文件夹里的desktop文件直接拷贝出来失败了（但是可以查看，所以可以复制内容），所以只能手写了一个了。

这个xxx.desktop文件可以在任何地方创建，最后挪到`/usr/share/Applications` 里面即可

[Desktop Entry]
Type=Application       # 基本固定这么写
Name=$AppName          # 展示的名字
Exec=$AppImagePath     # AppImage文件所在的路径
Icon=$AppIconPath      # 图标的路径
Keywords=$AppCorrelate # 描述App的关键词[可选的]

以上只有`Name` 、`Icon` 、`Exec` 是必须得有的，甚至可以只有`Exec`

最后将 `xxx.desktop` 文件挪到对应文件夹中：`sudo mv xxx.desktop /usr/share/Applications/`

此时再去查看应用程序中则可以找到这个应用的图标了