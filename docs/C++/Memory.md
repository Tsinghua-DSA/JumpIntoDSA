# 内存分配
## Memory Allocation

这一段会尝试向你介绍, 一个C++程序是如何使用“内存”的。

### 什么是内存?

程序所使用的“内存”，是对物理存储资源的一种抽象。

首先让我们看看计算机的组成：

一台计算机的核心，可以划分为数据存储系统和数据运算系统。另外，在处理数据之前需要从外界获得数据，处理数据之后可能要把数据发送给计算机之外。

数据存储系统的功能，是把数据用电子的方式（底层可能是高低电平、高低电压、电荷等物理量）表示出来。

数据运算系统的功能，是遵循特定的规则（也就是“程序”），对电子形式表示的数据进行变换和运算。这是CPU主要做的事情。

我们说一个程序在运行时“占据了多少空间”，最终都可以对应到物理世界中，它所涉及的信息使用了多少电子元器件来存储。我们描述程序使用的存储空间时，通常用“能存储多少个比特的信息”来描述存储空间的大小。

操作系统向上提供了一层抽象，将物理存储空间抽象为“内存”，并提供分配内存的机制。当程序运行时，需要向操作系统申请使用一定数量的内存，来存储程序运行过程中所处理的信息数据。（内存的数量，按照这些内存能够存储的二进制位的数量来计量。能够存储8个二进制位的内存就是1Byte内存）

>memory hierarchy: 实际上, 计算机里会混合使用多种性能不同的存储元器件。在一台计算机里, 有内存, 有磁盘。在CPU中，还有寄存器和缓存。寄存器、缓存、内存、磁盘，都是计算机存储的一部分，它们组成“memory hierarchy”。因为不同的存储元器件有着不同的读取速度和成本，最为经济高效的方法是，使用一系列速度不同的存储元器件，磁盘容量大价格低速度慢，寄存器容量极小速度极快价格极高，缓存、内存介于之间。

### 程序使用内存的类型

程序从操作系统那里获得的内存，可以按照获得内存的方式、使用内存的方式来分为几类。

全局内存(static): 全局变量、全局数组等。只要程序开始运行，这些内存就需要被分配，因为在程序的任何地方都可以访问到这些数据。

栈内存(stack): 函数调用栈所需要的内存，包括函数参数、函数内部声明的局部变量等。当函数被调用时，就会在函数调用栈中为函数分配一个栈帧以存储这些数据。只有当前正在运行、没有退出的函数，才会占据这些空间。

对于“递归”，每次进入函数时，都需要在函数调用栈中为函数分配一个栈帧，以存储这次调用函数时的参数、局部变量、函数返回到哪里等信息。因此，如果递归层数过多，分配了过多的栈帧，而函数调用栈的大小又是固定的，就会导致“栈溢出”错误，可能导致“Segmentation Fault”等报错信息。

堆内存(heap): 使用`new`操作，在程序运行中随时向操作系统申请使用的内存。在程序运行过程中, 通过`new`来随时申请内存更加灵活，但也具有一定的额外时间开销。（栈内存、全局内存相当于都是在程序开始运行时一次性申请好的)。`new`分配的内存如果不被`delete`, 就不会归还给操作系统, 操作系统就会认为程序依然占据着这片内存, 不会把这片内存用在其他用处。如果应当被释放的内存没有被释放，在程序持续运行的过程中，就会造成`内存泄漏(memory leaking)`, 最终会没有内存可用。


练习1: 在C++中 `delete a` 和 `delete[] a` 有何不同?

练习2: 阅读The C++ Programming Language(4th edition)中的6.4 Objects and Values, 说说左值和右值有什么区别。

练习3: 查找`RAII(Resource Acquisition is Initialization`的含义，思考C++的构造函数、析构函数等语法特性如何支持了`RAII`这种编程风格的实现。

练习4: 查找`垃圾回收(garbage collectinon)`的含义。 阅读The C++ Programming Language中的 34.3 Resource Management Pointers, 尝试使用C++中的"智能指针"`shared_ptr`和`weak_ptr`。