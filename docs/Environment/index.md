# 环境搭建

数据结构编程作业大部分是单文件项目, 有一些是轻量级的多文件项目，不引入第三方依赖。因此只需要轻量级的C++开发环境。

因此我们推荐同学们采用如下的 C++ 编译和开发环境：

操作系统：Windows 中的 WSL2、Linux 或 macOS

C++ 编译和调试工具：g++, gdb

C++ 开发环境：VS Code, 安装 C/C++ Extension Pack

使用统一的开发环境，将有助于减少平台不同导致的编译问题/未定义行为，方便同学们之间互相帮助调试，方便助教为同学们解决开发环境的问题。

## 安装 VS Code

通过[VS Code 官网](https://code.visualstudio.com/) 下载安装即可。

关于VS Code C++开发的配置，

- linux可参考: [https://code.visualstudio.com/docs/cpp/config-linux](https://code.visualstudio.com/docs/cpp/config-linux)

- Windows(WSL)可参考: [https://code.visualstudio.com/docs/cpp/config-wsl](https://code.visualstudio.com/docs/cpp/config-wsl)

- Mac OS 可参考: [https://code.visualstudio.com/docs/cpp/config-clang-mac](https://code.visualstudio.com/docs/cpp/config-clang-mac) 

注意这些配置会较多使用VS Code中的C++开发辅助功能。如果熟练使用，可能有助于提升开发效率，但并不是必须的。

从快速上手的角度，建议先不折腾这些高级功能，使用命令行来完成编译、运行、调试。

## 安装WSL2(Windows)

参考 [微软WSL2文档](https://docs.microsoft.com/zh-cn/windows/wsl/install)

然后安装 VS Code的 Remote - WSL扩展。

## VS Code快速上手指南

VS Code需要打开一个当前工作目录来进行开发。在Windows里，建议在WSL的`/home`目录下建立工作目录。

你需要了解三件事情: 

如何打开当前工作目录

如何在当前工作目录下新建文件和编辑文件

如何在VS Code中打开当前工作目录的命令行终端

利用好搜索引擎和VS Code的帮助文档即可。

在Windows中, `Ctrl + Shift + P`呼出命令面板, 然后输入`remote-wsl`, 就会蹦出很多和wsl相关的候选命令。可以通过`New WSL Window` 打开一个运行在WSL上的VS Code窗口。

打开一个工作目录(你的代码所在的文件夹), 一般是在菜单栏的`File`中选择`Open Folder`。

点击VS Code最左侧的一列大按钮中最上面的一个, 会打开资源管理视图, 看到当前工作目录下的文件树。可以直接在这里右键菜单新建文件、选中打开文件等。

在菜单栏`Terminal`中选择`New Terminal`就可以在当前工作目录下打开一个新的终端, 在里面用g++进行编译。

另外, 也可以先打开新的终端，用`mkdir`新建一个文件夹，再打开这个文件夹作为当前工作目录。

## bash快速上手指南

WSL、linux、Mac OS的命令行都支持bash操作。

你可以通过网上的文档摸索bash的使用，但并不意味着数据结构课上你需要精通它/理解它。

1. 学习这些命令的基础用法: ls, rm, mkdir, cd
2. 学习在你的命令行如何安装g++(Mac OS下可以用clang++), 然后使用g++从命令行编译运行`Hello, World`程序。
3. 学习如何在命令行让程序以某个文件为输入/输出到某个文件，形如`./a < 1.in > 1.out`

使用 [计算机系学生科协技能引导文档](https://docs.net9.org/basic/linux/#shell-101) 来快速上手shell的使用（wsl/linux/Mac OS的shell语法基本上是通用的，除了通过命令行安装软件的方法需要你通过搜索引擎解决一下）

## 算法演示

我们的一些算法演示基于 `java applet` 构建, 需要安装较早版本的Java SE(推荐为Java SE8)

建议从Oracle官方的旧版本存档链接下载适合你操作系统的版本: https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html 

如果你使用的是ARM芯片的Mac电脑，可以下载其中适合于x86 MAC电脑的版本，可以正常运行。

也可以考虑使用 https://www.cs.usfca.edu/~galles/visualization/Algorithms.html 之类基于javascript的演示。