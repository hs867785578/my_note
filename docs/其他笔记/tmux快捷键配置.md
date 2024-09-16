tmux的三个核心概念：session、window、panel，其中一个session有多个window，一个window有多个session，session可以用tmux ls查看，window显示在每一个session的下方，session为window中的窗口分割


配置文件：
vim ~/.tmux.conf

刷新配置：
tmux source-file ~/.tmux.conf

```
#开启鼠标(v2.1以上生效)
set-option -g mouse on
# 使用前缀+上键选择上方的窗格
bind Up select-pane -U

# 使用前缀+下键选择下方的窗格
bind Down select-pane -D

# 使用前缀+左键选择左侧的窗格
bind Left select-pane -L

# 使用前缀+右键选择右侧的窗格
bind Right select-pane -R

# 使用前缀+e进行左右分割
bind e split-window -h

# 使用前缀+l进行上下分割
bind l split-window -v

# 使用前缀+w关闭当前panel
bind w kill-pane

# 使用前缀+W关闭当前window
bind W kill-window
```

其他常用快捷键：
prefix+0-9，选择window
prefix+c 新建window
prefix +d detach某个session，暂时退出
tmux ls
tmux a -t 0-9，进入某个session
