# 入门C++ 
## _the ABCs of C++_

本课程的的编程作业需使用C++完成, 本课程的示例代码都使用C++编写。因此，入门基本的C++语法特性，对学习本课程是有必要的。我们假定至少学习过一门C语言/C++语言的程序设计类课程。

You need to finish the Programming Assignments in C++. All of our example codes will be in C++. Thus it's necessary for you to be familiar with the ABCs of C++ if you want to learn DSA.

We assume that you have finished at least one course on C/C++ programming.

这个网页**不是**一份完整的C++快速入门教程， 只是指出了你需要了解的内容。你可能需要查阅更多相关资料以完成学习。

我们使用_The C++ programming language(4th edition) by Bjarne Stroustrup_作为一本参考书, 网上很容易找到pdf版本。没时间就不必通读，将其当作随时查阅的“C++语言字典”。

这本书的[练习题](https://www.stroustrup.com/4thExercises.pdf)可在stroustrup的网站上下载，也是很好的学习资料。

我们第一轮计划介绍下面这些C++语法特性：
We will introduce the following C++ features:

- [类和对象 classes and objects](./Class&Object.md)
- [构造函数和析构函数 constructors and destructors](Cons&Dest.md)
- [继承 inheritance](Inheritance.md)
- [多态 polymorphysim](Polymorphism.md)
- 运算符重载   overloading operators
- 简单的模板 basics of templates 
- C++内存分配  memory allocation in C++
- 引用  referece

2022年是我们第一次做这样的尝试，希望同学们多多反馈。

This is our first try, and we treasure your opinions.


-------------------------------------------------------------

作业1: 实现一个分数类，支持你能想到的尽量多的接口和操作。
基础版: 分子分母均为unsigned int范围内的正整数，通过函数支持分数的加减乘除、比较大小
升级版: 通过运算符重载支持上述操作。
挑战版：支持分子分母是超出long long范围的数字（可搜索"大整数运算、高精度”等等）

作业2：实现一个矩阵类。实现一些你在线性代数里学到过的操作。

作业3: 阅读The C++ Programming Language(4th edition)的 Chapter 16 Classes, 跟着实现其中的Date类。尝试将Date类的实现放在date.h/date.cpp文件里, 然后在main.cpp里include这些文件，在main函数里编写一些用来测试Date类的用例。

