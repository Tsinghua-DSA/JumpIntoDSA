# gdb快速入门

翻译和改编自 [Stanford CS107 gdb guide](https://web.stanford.edu/class/archive/cs/cs107/cs107.1186/guide/gdb.html)

原作者 Nate Hardison, CS107 TA

GDB是一个功能强大的调试器，可以调试C语言等。它是GNU软件包的一部分，和gcc编译器、emacs编辑器等自由软件一样，在几乎所有的UNIX机器上都有安装。 因此斯坦福的很多计算机课程都使用GDB作为调试器。学着爱上GDB。同那些出bug时不停猜测、写一大堆printf语句的人比起来，熟练使用GDB的人在调试bug的时候要轻松得多。

这份文档是面向在CS107中使用GDB的一个简短介绍。从这个简短的介绍中掌握基础，当你想了解更多的时候，去看[完整的GDB手册](http://sourceware.org/gdb/current/onlinedocs/gdb/index.html)。

### 在GDB里运行一个程序 


GDB 是一个命令行工具，在终端里运行。你需要在一个UNIX环境里运行它（这个UNIX环境可以是一个虚拟机，可以是WSL，也可以在Mac里安装和gdb类似的lldb）。

GDB把你想调试的可执行文件作为它的参数。不是.c文件或者.o文件， 而是编译后的可执行文件的名字，通常没有扩展名。

这里我将在数据结构与算法的LAB0里运行GDB。

首先，在命令行里，`cd` 到 LAB0的目录下，通过`g++ solution_1.cpp -o sol1 -g`编译为可执行文件。然后我启动gdb:

```
gdb sol1
```
这会打印出类似下面的提示信息，并且在一行的开始有一个`(gdb)`提示符。
```
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04.1) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
......(省略)
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from sol1...
(gdb)
```
在有 `(gdb)`提示的情况下，命令`run`会启动程序的运行。如果你想在程序运行时给他什么命令行参数，你要把参数加到`run` 命令的后面。

如果我想让程序把输出重定向到`1.out`,我需要这么做:

```
(gdb) r sol1 > 1.out
Starting program: /home/my/path/LAB1/sol1 sol1 > 1.out
```
这会让程序开始运行。等到程序结束的时候，行首的`(gdb)`提示符又会出现。

### 设置断点(breakpoints)

通常来说，程序在return退出主函数的时候停止运行。通过断点，你可以让程序的执行在任意时刻挺下来，比如在某个函数调用或者某行代码的地方。断点是一个很重要的工具，通过它你可以在执行到特定场景时，让程序停下来，检查程序的执行状态。

通过你需要在程序启动之前把断点设置好，使用的命令为`break`。

例如，这样做可以在名字叫做main的函数开始时设置断点：

```
(gdb) b main
Breakpoint 1 at 0x555555555189: file solution_1.cpp, line 3.
```

给solution_1.cpp的某一行设置断点:

```
(gdb) b solution_1.cpp:10
Breakpoint 2 at 0x555555555225: file solution_1.cpp, line 11.
```

每次你设置断点时，gdb会给这个断点一个编号。如果你想删除一个断点，使用`delete`命令。

例如删除编号为2的断点:

```
(gdb) del 2
```

如果你搞不清设置了哪些断点，或者想看一下当前断点的编号都是什么，可以用`info break`命令查看当前生效的断点。

```
(gdb) info break
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000555555555189 in main() at solution_1.cpp:3
2       breakpoint     keep y   0x0000555555555225 in main() at solution_1.cpp:11
```

最后，注意函数名比行号更容易记住，而且在你修改程序的时候行号经常会发生变动，所以理想情况下我们通过函数名来设置断点。如果你将函数分解成小的、紧凑的函数，设置断点会很容易。反过来，在一个50行的函数里找打断点的合适地方会很难受。这也说明为什么要从一开始就保持良好的代码风格。

接下来的部分会介绍，当你的程序停在一个断点处，或因为触发段错误等原因而暂停运行时，你可以做哪些事情。

!!! question "什么是段错误?"

    系统报错"segmentation fault"的一类程序运行错误，通常和内存错误有关，如数组越界、访问空指针都可能导致段错误。


### 检查程序的状态

**回溯 Backtrace**

GDB最有用的一个功能就是，它能给你看一下当前的函数调用路径（或者说，“函数调用栈”）。这对于定位段错误之类的bug尤其有用。

如果一个叫做reassemble的程序在一个叫做read_flag的函数里发生了段错误，GDB会打印下面这样的信息:

```
Program received signal SIGSEGV, Segmentation fault.
0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
51      if (strlen(unusedptr) == MAX_FRAG_LEN)
```

这比不用GDB时的一个“Segmentation Fault”的系统错误信息更有用。你还可以通过 `backtrace` 命令来获取程序此时的完整函数调用状态。
```
(gdb) backtrace
#0  0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
#1  0x0000000000400bd7 in read_all_frags (fp=fp@entry=0x603010, arr=arr@entry=0x7fffffff4cb0, maxfrags=maxfrags@entry=5000) at reassemble.c:69
#2  0x00000000004010ed in main (argc=<optimized out>, argv=<optimized out>) at reassemble.c:211
```

每一行代表一个栈帧(stack frame), 也就是一次函数调用. 栈帧 #0 是错误发生的地方, 对read_frag的调用.十六进制的 0x0000000000400ac1 是导致段错误的指令地址(程序是一长串指令连成的数据,那么里面每一条指令当然有一个地址). 最后,你会看到出错的地方是 reassemble.c 的51行. 如果你要调试这个段错误, 这些都是很有用的信息.

在调试的时候, 你也很可能想看一下特定变量的取值. `print` 命令此时很好用.

**变量和表达式**

为了打印变量的数值:

```
(gdb) b main
Breakpoint 1 at 0x1189: file solution_1.cpp, line 3.
(gdb) r
Starting program: /home/wsliu/LAB1/sol1 

Breakpoint 1, main () at solution_1.cpp:3
3       int main(){
(gdb) p n
$1 = 32767
(gdb) p m
$2 = 1431655277
(gdb) p q
$3 = 21845
```
这打印了`solution_1.cpp`的n,m,q未初始化时的数值.

你甚至可以通过print来求值表达式, 调用函数, 给变量重新赋值,等等.

设置变量n的数值为1:

```
p n=1
$4 = 1
```

打印函数调用的返回值:

```
(gdb)  p memset(matrix, 10, 0)
$5 = (void *) 0x555555558040 <matrix>
```

下面的命令可以快速打印一个函数中的较多变量.

`info args`打印当前函数的所有参数.

如果当前函数没有参数,打印 "No arguments".
```
(gdb) info args
No arguments.
```

`info locals`打印当前函数所有局部变量.
```
(gdb) info locals
n = 32767
m = 1431655277
q = 21845
sum = 1431654560
(gdb) print n=10
$1 = 10
(gdb) info locals
n = 10
m = 1431655277
q = 21845
sum = 1431654560
```
**栈帧**

当你暂停程序运行时, 除了当前函数, 你有时也需要看一下上层调用它的函数. `up`和`down`命令可以用来做这个事情, 在函数调用序列里上下移动.


up 会让你向上移动一个栈帧 (从被调用的函数移动到调用它的函数), down反之.

下面的例子先从read_frag向上移动到调用它的read_all_frags, 然后向下移动回read_frag.

```
(gdb) up
#1  0x0000000000400bd7 in read_all_frags (fp=fp@entry=0x603010, arr=arr@entry=0x7fffffff4cb0, maxfrags=maxfrags@entry=5000) at reassemble.c:69
69          char *frag = read_frag(fp, i);
(gdb) down
#0  0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
51      if (strlen(unusedptr) == MAX_FRAG_LEN)
```

当发生段错误时, 你常常需要了解调用当前函数的函数的局部变量, 这时up和down会很有用.

### 控制程序执行

`run`命令可以让程序开始执行,或者在程序停在断点时让程序继续执行. `start`让程序从头开始执行,并停在main函数开始时.

当你停在断点时, 你可以选择如何让程序继续执行. `continue`命令会让程序继续执行到下一个断点或直到程序结束. `finish`会让当前函数执行完毕并在此处暂停. 你可以用`next`或`step`命令来单步执行, 这两个命令都会执行一行代码, 然后暂停一下. 区别在于, 如果这一行有函数调用, `next`会执行完成这个函数, `step`会调用函数并暂停在函数里的第一行.

### 实用建议
大部分gdb命令都可以缩写. 最常见的命令可以只用首字母.(s-step, b-break, c-continue, 等等). 其他命令也可以尽量的缩短(cond-condition) 

你可以在程序执行到任何地方的时候重启,用命令`r`就可以(gdb会要你确认是否真的想从头开始). 如果你没有输入参数, 它会自动复用上一次使用的参数. (如 > 1.out 之类的参数).

如果你用Makefile编译, 你可以在GDB里面重新编译, 不需要退出GDB之后重新打断点. 在(gdb)提示符后面敲make命令, 根据正确的Makefile, 就会编译可执行文件. 下一次输入run命令的时候, 就会重新加载新的可执行文件并把已有的断点加进去.

### 常见问题

**尝试启动gdb, 报错"No such file or directory", 怎么办?**

```
gdb myprogram
myprogram: No such file or directory.
(gdb)
```

错误信息表示当前目录里没有找到你指定的名字的可执行文件. 可能原因: 打错了程序名字, 当前在错误的目录下, 忘了编译代码为可执行文件.

**报错no debugging symbols found, 也无法使用函数名和变量名来调试, 怎么办?**
```
gdb myprogram
Reading symbols from myprogram...(no debugging symbols found)...done.
(gdb)
```

错误信息表示, 编译时, 编译器没有加入调试所需的符号, 这是编译选项不合适导致的. 在编译选项里加上`-g`.

**gdb警告 the source file is more recent, 怎么办**

```
(gdb) list
warning: Source file is more recent than executable.
```

这表示你修改了一些代码, 但修改后没有重新编译. gdb 此时无法把当前的可执行文件和代码文件放在一起调试. 退出gdb, 重新编译, 然后再进入gdb.

**尝试给程序传入命令行参数, 但是报错了**

```
gdb myprogram julie
Reading symbols from myprogram...done. 
"julie" is not a core dump: File format not recognized"
(gdb)
```

程序运行时的命令行参数不应当在启动gdb的时候传入,而应该在 run 命令之后加入. 正确做法是下面这样:

```
gdb myprogram
Reading symbols from myprogram...done.
(gdb) run julie
```

**我单步调试进了一个库函数, gdb说找不到源文件, 怎么办?**

```
(gdb) s
_IO_new_fopen (filename=0xffffdc9f "samples/input", mode=0x804939a "r") at iofopen.c:102
102 iofopen.c: No such file or directory.
```
step命令通常执行下一行代码, 如果下一行是函数, 会跳到函数里执行函数的下一行代码. 但在库函数里, gdb缺乏必要的源文件来调试. 因此, gdb会表示"No such file". 这是你可以通过 finish 命令来结束当前函数. 或者, 一开始的时候不要用step, 而是用next命令将下一行执行完, 不管是不是一个函数调用, 不去函数里面单步运行.

**我的程序在库函数里段错误了, 怎么办?**
```
Program received signal SIGSEGV, Segmentation fault.
__strcmp_ssse3 () at ../sysdeps/i386/i686/multiarch/strcmp-ssse3.S:232
232 ../sysdeps/i386/i686/multiarch/strcmp-ssse3.S: No such file or directory.
```

有可能是你传给库函数的参数有问题, 例如, 对于需要访问内存的库函数, 你传给他一个空指针, 就会导致内存访问出错. 这里建议你仔细检查一下传给它的参数.


**程序结束的时候, gdb说我的程序是 "inferior"的(中文: 更内部的, 更弱小的), 我冒犯到gdb了吗?**
```
[Inferior 1 (process 25178) exited normally]
                  or
[Inferior 1 (process 25609) exited with code 01]
```

别太在意, gdb将你的程序作为一个子进程运行, 因此是"内部的". "exited normally" 表示程序正常结束, 没有发生段错误或死循环等无法正常退出的情况.