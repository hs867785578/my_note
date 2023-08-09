
- ctrl+s后屏幕锁定了，这时只要ctrl+q即可退出
- ctrl+z退出了vim，再次打开时会有swap，这是因为ctrl+z在后台挂起了vim，这时先退出，然后jobs命令查看一下vim是否在后台，然后fg %vim进入后台的vim