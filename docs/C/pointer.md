# 指针

对本文档感到疑惑，可咨询liurd22艾特mails点tsinghua点edu点cn

为了较好地理解指针，我们需要理解内存的寻址方式。

计算机内部有硬盘(固态硬盘或机械硬盘)、内存、缓存、寄存器等不同层次的存储，我们写程序时最先要认识的是内存。

通常来说，程序运行时所需要使用的数据都要加载到内存中。而需要长期存储（关机断电后也保留）的数据存储在硬盘中。而缓存和寄存器，加速了我们读取内存的速度。（CPU的指令常常以寄存器为直接操作的对象）。

什么是内存？内存本质上是一种电路，能够储存一系列的0/1开关状态，并给这些开关状态编号，用编号查询开关状态。一根内存条中，包含大量相同的存储单元。

一般来说，我们将8个连续的0/1开关状态称作一个“字节”，按字节来编码地址。内存地址常常写作十六进制。

例如 地址`0x0`表示内存中的第一个字节，`0x1`表示内存中的第二个字节......

不同的程序可能拥有不同的地址空间，以实现“这个程序默认不能去读写另一个程序所属的内存”这样的特性。

计算机内部最原始的访存方法，就是给内存控制器发出一个“地址信号”, 内存控制器按照这个信号里的地址, 找到对应的存储电路单元，读出里面的数据，返还给CPU。

在这样的内存的基础上，我们在高级语言中抽象出变量的概念。一个变量相当于一块特定的内存加上一个类型。“一块特定的内存”表示这个变量存储在哪个地址、占用多少个字节，一个类型表示这个变量中存储的数据具有什么意义、应当如何解读。

也可以认为“变量的类型”就包含了“变量占用多少字节”的信息，此时认为一个变量由一个起始地址和一个类型组成。

指针，就是在C语言中，揭开变量的一层面纱，绕过一层抽象，直接操作地址的手段。

指针变量，就是在一个变量中，存储了一个地址，并且可以读取这个地址中对应的数据。而指针变量的不同类型，表示的是这个地址中数据的含义，或者说这个地址的内存能被当作什么类型的变量使用。

你可能会想到，在程序运行中，哪里存储了“变量类型”这一信息？

事实上，对于C语言这种高级语言，“变量类型”在编译结束后就不存在了。编译结束之后得到的机器语言里，每条指令眼中只有“某个内存地址”，而没有变量的名字和类型。C语言里写的赋值操作`int a = 1`, 在编译之后的机器语言程序中，假设变量`a`被放到地址`0x1234`, 那么机器语言只会有一条指令表示`将数值1放到地址0x1234开始、长度为4个字节的内存中`。

数组是若干个类型相同的变量顺序摆放。因此数组名称可以看作是指向数组中第一个元素的指针。可以通过指向第一个元素的指针做加法，得到指向后面元素的指针。

（未完待续......)