# 环境搭建

数据结构编程作业大部分是单文件项目, 有一些是轻量级的多文件项目，不引入第三方依赖。因此只需要轻量级的C++开发环境。

因此我们推荐同学们采用如下的 C++ 编译和开发环境：

操作系统：Windows 中的 WSL2、Linux 或 macOS, 掌握命令行的bash基础使用。

C++ 编译和调试工具：g++, gdb, 掌握从命令行使用它们的方法。

C++ 开发环境：VS Code, 安装 C/C++ Extension Pack

使用统一的开发环境，将有助于减少平台不同导致的编译问题/未定义行为，方便同学们之间互相帮助调试，方便助教为同学们解决开发环境的问题。


!!! question "为什么要求掌握命令行的使用？而不使用Visual Studio/Dev CPP等以图形界面为主的IDE? "

    对于编程开发来说，命令行的操作具有比图形界面更高的自由度,图形界面只是将一些常用操作实现成了容易使用的形式。而一些较为复杂的代码工作甚至是没有图形界面可用的。另外，本课程的LAB题目形式多样，一些任务使用命令行操作更容易完成。

!!! question "我使用M1芯片的Mac，gdb尚未为其提供支持，怎么办?"

    可以尝试使用 lldb 作为 gdb 的替代品。lldb扩展了gdb的一部分命令，但gdb的命令在lldb也可以使用。

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

[这个链接](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment)是微软WSL2文档中关于安装WSL的分步教程。

然后安装 VS Code的 `WSL` 扩展, 用`Remote - WSL`的方式进行开发。

这是一个相当常见的环境配置步骤，你可以尝试在一些视频网站(bilibili等）选择合适的关键词，搜索视频教程。

!!! question "WSL 是什么?"

    在本课程中，可以这样理解：是一种在windows操作系统的电脑上，安装linux双系统/虚拟机的便捷途径。
    
    安装WSL的时候，Windows操作系统会将自己的一部分磁盘划分给WSL，在里面安装一个Linux操作系统。随后我们就可以在Windows里打开这个Linux操作系统，对于用户来说，这就相当于你有了两台电脑，一台安装了Windows，一台安装了Linux，虽然你只使用了一台电脑的硬件。

!!! question "Remote - WSL 是什么?"

    Remote-WSL的逻辑和Remote-SSH相同。Remote-SSH的工作方式是，通过ssh网络协议远程连接到另一台电脑上，然后你在本地的机器上使用VS Code编程，就好像直接在远程的另一台电脑上工作一样。而Remote-WSL的工作方式很类似，也是打开了WSL的Linux操作系统（“虚拟机”）之后，使用VS Code 编程，就好像你直接在一台Linux操作系统的机器上使用VS Code编程一样。

!!! question "我在安装WSL的过程中/在WSL里安装软件的过程中遇到错误信息, 怎么办?"

    这种错误很可能有人遇到过，首先可以尝试将错误信息粘贴到搜索引擎里进行搜索，尝试一些解决方法。如果无效，请描述目前做过的步骤和解决问题的尝试，将错误信息截图发到网络学堂答疑区，助教会帮你解决。

    一些常见问题的解决方法可以参考文档的[这一部分](./wsl_error.md)

## VS Code快速上手指南

VS Code需要打开一个当前工作目录来进行开发。在Windows里，建议在WSL的`/home`目录下建立工作目录。

你需要了解三件事情: 

如何打开当前工作目录

如何在当前工作目录下新建文件和编辑文件

如何在VS Code中打开当前工作目录的命令行终端

利用好搜索引擎和VS Code的帮助文档即可。

在Windows下的VS Code中, `Ctrl + Shift + P`呼出命令面板, 然后输入`remote-wsl`, 就会蹦出很多和wsl相关的候选命令。可以通过`New WSL Window` 打开一个运行在WSL上的VS Code窗口。

打开一个工作目录(你的代码所在的文件夹), 一般是在菜单栏的`File`中选择`Open Folder`。

点击VS Code最左侧的一列大按钮中最上面的一个, 会打开资源管理视图, 看到当前工作目录下的文件树。可以直接在这里右键菜单新建文件、选中打开文件等。

在菜单栏`Terminal`中选择`New Terminal`就可以在当前工作目录下打开一个新的终端, 在里面用g++进行编译。

另外, 也可以先打开新的终端，用`mkdir`新建一个文件夹，再打开这个文件夹作为当前工作目录。

## bash快速上手指南

WSL、linux、Mac OS的命令行都支持bash操作。

你可以通过网上的文档摸索bash的使用，但并不意味着数据结构课上你需要精通它/理解它。

0. 理解当前工作目录的概念。正如同你操作图形界面时, 需要打开一个“当前文件夹”, 然后在这个文件夹里操作, 命令行界面中也会有一个“当前工作目录/当前路径”的概念，以当前路径为起点访问其他文件。
1. 学习这些命令的基础用法: ls, rm, mkdir, cd
2. 学习在你的命令行如何安装g++(Mac OS下可以用clang++), 然后使用g++从命令行编译运行`Hello, World`程序。
3. 学习如何在命令行让程序以某个文件为输入/输出到某个文件，形如`./a < 1.in > 1.out`

使用 [计算机系学生科协技能引导文档](https://docs.net9.org/basic/linux/#shell-101) 来快速上手shell的使用（wsl/linux/Mac OS的shell语法基本上是通用的，除了通过命令行安装软件的方法需要你通过搜索引擎解决一下）

Mac OS下建议使用`brew` , 可能需要从 https://brew.sh/ 安装，通过`sudo brew install g++`这种命令来安装g++。

Linux/WSL下建议使用`apt`, 大部分情况无需安装`apt`, 直接通过`sudo apt install g++`这种命令来安装g++。可能需要先执行`sudo apt-get update`。

!!! question "如何通俗理解bash和shell？他们究竟是什么东西，起什么作用呢？"

    bash和shell属于“命令行界面”（command-line interface），这是相对于我们使用的“图形用户界面”（graphic user interface)而言。
    
    图形用户界面中，我们用鼠标点击图标、选择菜单中的选项来要求执行某一项操作，而在命令行界面中，我们像写代码一样，按照语法（bash语法）来编写一些命令，请求操作系统执行某一项操作。

    例如，在图形用户界面中，我们打开一个文件夹folder1，右键点击一个文件FileA的图标，然后点击“删除”选项，就请求操作系统删除FileA这个文件。

    而在“命令行界面”（shell）中，我们将当前路径调整到folder1（“当前路径”相当于图形界面中“当前打开的文件夹”），然后在窗口中输入“rm FileA”，就请求操作系统删除FileA这个文件。

    更多的用法可以自由探索。

## 算法演示

我们的一些算法演示基于 `java applet` 构建, 需要安装较早版本的Java SE(推荐为Java SE8)

建议从Oracle官方的旧版本存档链接下载适合你操作系统的版本: https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html 

如果你使用的是ARM芯片的Mac电脑，可以下载其中适合于x86 MAC电脑的版本，可以正常运行。

也可以考虑使用 https://www.cs.usfca.edu/~galles/visualization/Algorithms.html 之类基于javascript的演示。