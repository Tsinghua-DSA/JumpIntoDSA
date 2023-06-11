# gdb快速入门

翻译和改编自 [Stanford CS107 gdb guide](https://web.stanford.edu/class/archive/cs/cs107/cs107.1186/guide/gdb.html)

原作者 Nate Hardison, CS107 TA

GDB是一个功能强大的调试器，可以调试C语言等。它是GNU软件包的一部分，和gcc编译器、emacs编辑器等自由软件一样，在几乎所有的UNIX机器上都有安装。 因此斯坦福的很多计算机课程都使用GDB作为调试器。学着爱上GDB。同那些出bug时不停猜测、写一大堆printf语句的人比起来，熟练使用GDB的人在调试bug的时候要轻松得多。

这份文档是面向在CS107中使用GDB的一个简短介绍。从这个简短的介绍中掌握基础，当你想了解更多的时候，去看[完整的GDB手册](http://sourceware.org/gdb/current/onlinedocs/gdb/index.html)。

### 在GDB里运行一个程序 


GDB 是一个命令行工具，在终端里运行。你需要在一个UNIX环境里运行它（这个UNIX环境可以是一个虚拟机，可以是WSL，也可以在Mac里安装和gdb类似的lldb）。

GDB把你想调试的可执行文件作为它的参数。不是.c文件或者.o文件， 而是编译后的可执行文件的名字，通常没有扩展名。

这里我将在LAB0 Jump Into DSA里运行GDB。

# TODO: 下面cs107e assignment1里用gdb的演示，替换为用lab0 jump into dsa的演示

Here's how I'd start GDB on my Assignment 1. I have cd'ed to my assign1 directory and I've already compiled and compiled my "myprogram" executable. I issue the command gdb and the name of my executable as below:

gdb myprogram
This starts up gdb, prints the following output, and now I have the (gdb) prompt:

GNU gdb (Ubuntu 7.7.1-0ubuntu5~14.04.2) 7.7.1
Copyright (C) 2014 Free Software Foundation, Inc.
... blah blah...
Reading symbols from myprogram...done.
(gdb)
Once you've got the (gdb) prompt, the run command starts the executable running. If the program you are debugging requires any command-line arguments, you specify them to the run command. If I wanted to run my program with the arguments "julie", I'd do the following:

(gdb) r julie
Starting program: /afs/ir.stanford.edu/users/z/e/zelenski/myprogram julie
This starts the program running. When the program stops, you'll get your (gdb) prompt back.

Setting breakpoints
Normally, your program only stops when it exits. Breakpoints allow you to stop your program's execution wherever you want, be it at a function call or a particular line of code. Breakpoints are an essential tool that allow you to stop and examine the program state at a specific context within the execution.

Before you start your program running, you want to set up your breakpoints. The break command allows you to do so.

To set a breakpoint at the beginning of the function named main:

(gdb) b main
Breakpoint 1 at 0x400a6e: file myprogram.c, line 44.
To set a breakpoint at a particular line number in myprogram.c:

(gdb) b myprogram.c:47
Breakpoint 2 at 0x400a8c: file myprogram.c, line 47.
Every time you create a breakpoint, it's assigned a number. If you want to delete a breakpoint, just use the delete command.

To delete the breakpoint numbered 2:

(gdb) del 2
If you lose track of your breakpoints, or you want to see their numbers again, the "info break" command lets you know which ones are active:

(gdb) info break
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000000400a6e in main at myprogram.c:44
Finally, notice that it's much easier to remember function names than line numbers (and line numbers change from run to run when you're changing your code), so ideally you will set breakpoints by name. If you have decomposed your code into small, tight functions, setting breakpoints will be easy. On the other hand, wading through a 50-line function to find the right place for a breakpoint is unpleasant, so yet another reason to decompose your code cleanly from the start!

The following sections deal with things you can do when you're stopped at a breakpoint, or when you've encountered a segfault or bus error.

Examining program state
Backtrace
Easily one of the most immediately useful things about GDB is its ability to give you a backtrace (or a "stack trace") of your program's execution at any given point. This works especially well for locating things like segfaults and bus errors. If a program named reassemble segfaults during execution of a function named read_frag, GDB will print the following information:

Program received signal SIGSEGV, Segmentation fault.
0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
51      if (strlen(unusedptr) == MAX_FRAG_LEN)
Not only is this information vastly more useful than the terse "Segmentation fault" error that you get outside of GDB, you can use the backtrace command to get a full stack trace of the program's execution when the error occurred:

(gdb) backtrace
#0  0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
#1  0x0000000000400bd7 in read_all_frags (fp=fp@entry=0x603010, arr=arr@entry=0x7fffffff4cb0, maxfrags=maxfrags@entry=5000) at reassemble.c:69
#2  0x00000000004010ed in main (argc=<optimized out>, argv=<optimized out>) at reassemble.c:211
Each line represents a stack frame (ie. a function call). Frame #0 is where the error occurred, during the call to read_frag. The hexadecimal number 0x0000000000400ac1 is the address of the instruction that caused the segfault (don't worry if you don't understand this, we'll get to it later in the quarter). Finally, you see that the error occurred from the code in reassemble.c, on line 51. All of this is helpful information to go on if you're trying to debug the segfault.

You're likely to want to check into the values of certain key variables at the time of the problem. The print command is perfect for this:

Variables and expressions
To print out the value of variables:

(gdb) p nread
$1 = 0
(gdb) p fp
$2 = (FILE *) 0x603010
(gdb) p start
$3 = 123 '{'
You can also use print to evaluate expressions, make function calls, reassign variables, and more.

Sets the first character of buffer to be 'a':

p buffer[0] = 'Z'
$4 = 90 'Z'
Print result of function call:

(gdb) p strlen (buffer)
$5 = 10
The following commands are handy for quickly printing out a group of variables in a particular function

info args prints out the arguments to the current function you're in:

(gdb) info args
fp = 0x603010
nread = 0
info locals prints out the local variables of the current function:

(gdb) info locals
start = 123 '{'
end = 125 '}'
nscanned = 3
Stack frames
If you're stopped at a breakpoint or at an error, you may also want to examine the state of stack frames further back in the calling sequence. You can use the up and down commands for this.

up moves you up one stack frame (e.g. from a function to its caller)

(gdb) up
#1  0x0000000000400bd7 in read_all_frags (fp=fp@entry=0x603010, arr=arr@entry=0x7fffffff4cb0, maxfrags=maxfrags@entry=5000) at reassemble.c:69
69          char *frag = read_frag(fp, i);
down moves you down one stack frame (e.g. from the function to its callee)

(gdb) down
#0  0x0000000000400ac1 in read_frag (fp=fp@entry=0x603010, nread=nread@entry=0) at reassemble.c:51
51      if (strlen(unusedptr) == MAX_FRAG_LEN)
The commands above are really helpful if you're stuck at a segfault and want to know the arguments and local vars of the faulting function's caller (or that function's caller, etc.)

Controlling execution
run will start (or restart) the program from the beginning and continue execution until a breakpoint is hit or the program exits. start will start (or restart) the program and stop execution at the beginning of the main function.

Once stopped at a breakpoint, you have choices for how to resume execution. continue will resume execution until the next breakpoint is hit or the program exits. finish will run until the current function call completes and stop there. You can single-step through the C source using the next or step commands, both of which execute a line and stop. The difference between these two is that if the line to be executed is a function call, next executes the entire function, but step calls function and stops at first line.

Useful tips
Most gdb commands can be abbreviated. The super-common commands can be shortened to just the first letter (s for step, b for break, c for continue, etc.). Others can be shortened to their unique prefix (cond for condition)
You can restart your program at any point during its execution. Just use the r command again (it will prompt you to make sure you really want to restart). If you use without any arguments, since it remembers the last ones you entered.
If you're using a Makefile, you can recompile from within GDB so that you don't have to exit and lose all your breakpoints. Just type make at the (gdb) prompt, and it will rebuild the executable. The next time you run, it will reload the updated executable and reset your existing breakpoints.
Use GDB inside Emacs! It's just another reason why Emacs is really cool. Use "Ctrl-x 3" to split your Emacs window in half, and then use "Esc-x gdb" or "Alt-x gdb" to start up GDB in your new window. If you are physically at one of the UNIX machines, or if you have X11 forwarding enabled, your breakpoints and current line show up in the window margin of your source code file.
Looking to learn some fancier tricks? See these articles Julie wrote for a programming journal: Breakpoint Tricks and gdb's Greatest Hits. There's also the full online gdb manual to learn all the ins and outs: http://sourceware.org/gdb/current/onlinedocs/gdb/index.html.

Common questions about gdb
When I start gdb, it complains the program is missing. What is wrong?
gdb myprogram
myprogram: No such file or directory.
(gdb)
This errors means there's no program named myprogram in your current working directory. Here are some of more likely causes for this error: you're in the wrong directory, you mistyped the name, or you haven't yet compiled your code.

When I run my program under gdb, it complains about missing symbols. What is wrong?
gdb myprogram
Reading symbols from myprogram...(no debugging symbols found)...done.
(gdb)
This means the program was not compiled with debugging information. Without this information, gdb won't have full symbols for setting breakpoints or printing values or other program tracing features. There's a flag you need to pass to gcc to build debugging info into the executable. If you're using raw gcc, add the -g flag to your command. If you're using a Makefile, make sure the CFLAGS line includes the -g flag.

When I view my code from within in gdb, it warns that the source file is more recent. What does this mean?
(gdb) list
warning: Source file is more recent than executable.
This means that you have edited one or more of your .c source files, but have not recompiled those changes. The program being executed will not match the edited source code and gdb's efforts to try to match up the two will be hopelessly confused. You can quit out of gdb, make, and then restart gdb, or even more conveniently, make from with gdb will rebuild the program and reload it at next run, all without leaving gdb.

How do I use gdb on a program that takes command-line arguments? I tried passing them when I start gdb, but it complains.
gdb myprogram julie
Reading symbols from myprogram...done. 
"julie" is not a core dump: File format not recognized"
(gdb)
The program arguments aren't specified when invoking gdb, they are given to the run command once within gdb. A correct sequence is shown below:

gdb myprogram
Reading symbols from myprogram...done.
(gdb) run julie
I step into a library function and gdb complains about a missing file. What is this and what should I do about it?
(gdb) s
_IO_new_fopen (filename=0xffffdc9f "samples/input", mode=0x804939a "r") at iofopen.c:102
102 iofopen.c: No such file or directory.
The step command usually executes the next single line of C code. If that line makes a function call, step will advance into that function and allow you trace inside the call. However, if the function is a library function, gdb will not be able to display/trace inside it because the necessary source files are not available on the system. Thus, if asked to step into a library call, gdb responds with this harmless complaint about "No such file". At that point, you can use finish to continue through the current function. As alternative to step, the next command will execute the entirety of the next line, completing all function calls rather than attempting to step into them.

My program crashes within a library function. It's not my fault the library is broken! What can I do?
Program received signal SIGSEGV, Segmentation fault.
__strcmp_ssse3 () at ../sysdeps/i386/i686/multiarch/strcmp-ssse3.S:232
232 ../sysdeps/i386/i686/multiarch/strcmp-ssse3.S: No such file or directory.
The example crash shown above is occurring during a call to strcmp, a function from the standard library. The arguments to strcmp are expected to be valid char*s. If given an invalid address, the function will crash trying to read from that location. The library function does not have a bug, the mistake is that you are passing an invalid argument; look at your call to strcmp to resolve your bug. The complaint about missing files (discussed above) is a harmless warning you can safety ignore. On the other hand, the bug in your use of the library function needs to be investigated further! :-)

When I start gdb, it gives a long warning about auto-loading being declined. What's wrong?
Reading symbols from bomb...(no debugging symbols found)...done.
warning: File "/afs/ir.stanford.edu/users/z/e/zelenski/a5/.gdbinit" auto-loading has been declined by your `auto-load safe-path' set to "$debugdir:$datadir/auto-load".
To enable execution of this file add
    add-auto-load-safe-path /afs/ir.stanford.edu/users/z/e/zelenski/a5/.gdbinit
line to your configuration file "/afs/ir/users/z/e/zelenski/.gdbinit".
... blah blah blah ...
A .gdbinit file is used to set a startup sequence for gdb. However, for security reasons, loading from a local .gdbinit is disabled by default. If there is a .gdbinit in the current directory and you have not configured gdb to allow loading it, gdb complains to alert you that this .gdbinit file is being ignored. To enable loading, you must edit your personal gdb configuration file to change your auto-load setting. Your personal gdb configuration is ~/.gdbinit (a hidden file in your home directory). A .gdbinit file is a plain text file you can edit with your favorite editor. If file doesn't yet exist, you will need to create it. The line you need to add is set auto-load safe-path /. Alternatively, you can copy and paste the command below to append the proper setting to your personal configuration file, creating the file if it doesn't already exists. You will need to make this configuration change only once.

bash -c 'echo set auto-load safe-path / >> ~/.gdbinit'
You can check your current configuration by searching your personal configuration file for the setting. See command below and expected response:

myth> grep auto-load ~/.gdbinit
set auto-load safe-path /
Once your personal configuration is appropriately set, there will be no further complaints from gdb about declining auto-load and it will load any local .gdbinit file on start.

When my program finishes, gdb prints a message calling my program "inferior". What have I done to offend gdb?
[Inferior 1 (process 25178) exited normally]
                  or
[Inferior 1 (process 25609) exited with code 01]
Don't take it personally, gdb runs your program as a sub-process which it terms the "inferior". The message indicates the program has run to completion and exited in a controlled fashion-- there was no segmentation fault, abort, hang, or other catastrophic termination condition.

When I run gdb without a program, some things (like sizeof and typecasts) behave differently. What's happening?
myth$ gdb
(gdb) p sizeof(long)
$1 = 4
(gdb) p/x (long)-1
$2 = 0xffffffff
(gdb)
By default, gdb determines your CPU architecture based on the program you're debugging. If you don't specify a program when starting gdb, it defaults to assuming a 32-bit system, rather than the 64-bit system that all of the programs we write will use. Starting gdb on one of your CS107 programs (any one will do) will cause gdb to use the proper architecture. You can also manually put gdb into 64-bit mode with the following command:

set architecture i386:x86-64
After that, the above commands should now print correctly:

myth$ gdb
(gdb) set architecture i386:x86-64
The target architecture is assumed to be i386:x86-64
(gdb) p sizeof(long)
$1 = 8
(gdb) p/x (long)-1
$2 = 0xffffffffffffffff
(gdb)
