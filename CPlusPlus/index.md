# 入门C++ 
## _the ABCs of C++_


我们认为，完成编程练习，是更能体现编程能力、提高编程能力的。所以请尽快着手，开始完成这些编程练习。将会不断更新补充。

作业1: 实现一个分数类，支持你能想到的尽量多的接口和操作。

基础版: 分子分母均为unsigned int范围内的正整数，通过函数支持分数的加减乘除、比较大小

升级版: 通过运算符重载支持上述操作。

挑战版：支持分子分母是超出long long范围的数字（可搜索"大整数运算、高精度”等等）

可在 [DSA自测课堂](https://dsa.cs.tsinghua.edu.cn/oj/course.shtml?courseid=167) 中 JumpIntoDSA 里面的 Fraction题目来测试你的有理数类。

- 提示1: 在约分时, 可以考虑"辗转相除法" 或"辗转相减, 有2除2"的方法

- 提示2: 对于挑战版，你可以考虑实现一个“大整数类”

- 提示3：可以看看[python的有理数类的文档](https://docs.python.org/3/library/fractions.html), 尝试实现里面的一些接口，也可以在自己电脑上安装python，尝试用一用python里面的有理数类。



作业2：实现一个矩阵类,矩阵里的每个元素是你在作业1里实现的分数类型。然后尝试实现一些你在线性代数里学到过的操作，比如矩阵乘法。

作业3: 用模板实现作业2中的矩阵类，矩阵里的元素类型是模板参数T。


-----------------------------------------------------------------------------

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
- [运算符重载   overloading operators](Overloading.md)
- [简单的模板 basics of templates](Templates.md)
- [C++内存分配  memory allocation in C++](Memory.md)
- [引用  referece](Reference.md)

2022年是我们第一次做这样的尝试，希望同学们多多反馈。

This is our first try, and we treasure your opinions.





