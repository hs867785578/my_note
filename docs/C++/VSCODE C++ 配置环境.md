**工具链**

**1. 代码提示**
选择c/c++而不选择clangd，主要是因为c/c++插件可以配置c_cpp_properties.json，来包含头文件进行提示。

clangd依赖于cmake生成的compile_commands.json，不太方便。值得注意的是在代码量大的时候clangd的提示速度大于C/C++。且C/C++和clang插件不能共存，所以不能下载clangd插件。

clangd也可以像c_cpp_properties.json一样自己配置头文件路径。具体在seetings.json中。
![](../_resources/ec6fde9da896007af81252adad3f5ff1.png)

clangd下载地址：github搜clangd，直接下载。clangd和LLVM不是绑定的，需要单独下载。

**2.代码格式化**
这个毫无疑问，选择clang-format，安装该插件，可以通过设置查找“format”或“clang”来配置。注意：
2.1 如果代码提示用的clangd，需要在clang插件中需要配置clang的path，和clang format的path。
clangd下载地址：github搜clangd，直接下载

2.2 如果代码提示用的c/c++，需要配置如下三项：第一项是备选风格。第二项是安装的clang-format的路径（可以直接安装LLVM，在bin文件夹下有clang-format.exe文件）。第三项是是否使用.clang-format文件。

clang-format下载地址：和llvm绑定的，需要下载llvm
![](../_resources/85c99138c37b16515365ee1733e1e809.png)

![](../_resources/29e8f0701c00cfc2313b85c76019f035.png)

**3.编译器**
选择GCC而不是clang