# 多态
## Polymorphism

在数据结构课程中我们并不会经常用到多态。因此你只需要知道大致的概念:

一个基类可能有多个派生类，这些派生类对基类的接口有不同的实现。在程序运行时，将按照一个对象所“真正属于”的类别，来调用它的成员函数。

例如，基类Shape有派生类Rectangle和Circle,分别实现了Rectangle::area()和Circle::area()
现在我们将一个Rectangle类型的对象赋给一个Shape类型的指针。
```c++
Shape* pt = new Rectangle();
pt->area();
```
这个时候，虽然pt是一个Shape类型的指针，但被调用的函数将会是Rectangle::area()

如果你暂时感到迷糊，不要紧，课程中我们很少用到多态。