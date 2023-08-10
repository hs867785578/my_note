三大主流编译器中clang是LLVM的一个子项目，是LLVM的前端
LLVM的结构如下：

LLVM早期的前端使用的是gcc，后来apple公司重新为C C++ Objective C重新设计了LLVM的前端，取名为clang，代替掉gcc的前端。从而使LLVM变成了前端clang，后端LLVM，不再是前端gcc，后端LLVM。

gcc的前端、中端、后端都叫gcc。

![](images/c++编译器gcc%20clang%20MSVC_image_1.png)


clang-format是用来格式化代码的，可以单独下载
clangd是用来智能提示的，和vscode中c/c++ 插件是冲突的，他的提示依赖于Cmake生成的compile_commands.json

![](images/c++编译器gcc%20clang%20MSVC_image_2.png)