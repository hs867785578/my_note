
## 1后缀
- libxxx：只会安装libxxx.so.x.x的动态库
- libxxx-dev后缀（develope）：包含了库的接口（.h文件即头文件）和静态库以及动态库，如果编译需要用到该库，那么需要安装dev版本。devel包含普通包，但比普通包多了头文件。
在编译一个用 C 语言编写的 Python 扩展模块时，里面会有 `#include<Python.h>` 这样的语句，因此需要先安装 `python-dev` or `python-devel` 开发包
- libxxx-dbg后缀（debug）：包含调试符号，通常仅供开发人员使用该软件或调试软件的人员使用。
- libxxx-utils后缀（utility）：通常提供一些额外的命令行工具。 它可能会将用户暴露给内部功能或仅提供CLI。

## 2不带后缀

如果安装库没有-dev后缀，那么只会安装.so.x.x动态库

