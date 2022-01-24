# 模板
## Templates

“模板”是C++中的一种语法特性，最突出的特点是“以类型为参数”。

我们可以从“简化程序实现”的角度对比“函数”和“模板”这两种语言特性：

函数，可以避免在程序里重复完全相同的代码, 也可以避免重复“大部分完全相同，只有几个数值参数会改变”的代码。

模板，可以用来避免在程序里重复编写“仅有数据类型不同”的代码。

可以在定义函数时，让函数的参数类型、返回类型不是某一个特定的类型，而是“某一类”类型。这种使用，可以称作`函数模板`或者`模板函数`, 用你喜欢的名字, 语序不影响理解。

也可以在定义新类型时，让新类型里包含的某些成员变量不是一个特定的类型，而是“某一类”类型。这种使用，可以称作`类模板`或者`模板类`, 用你喜欢的名字, 语序不影响理解。

> There are people who make semantic distinctions between the terms class template and template class. I don’t; that would be too subtle: please consider those terms interchangeable. Similarly, I consider function template interchangeable with template function. Bjarne Stroustrup, The C++ programming language 4th edition p670

一个函数模板的例子, 支持各种类型的min/max函数:
```c++

template <typename T> 
T min(T a, T b){
    return a<b?a:b;
}

template <typename T> 
T max(T a, T b){
    return a>b?a:b;
}
```
`template <typename T>` 声明, 在接下来的**一个**定义里, 将会使用`T`作为一个模板类型参数。
随后的函数定义和正常函数差不多, 把`T`想象成一个你自定义的类型即可。函数里的操作, 通常会要求`T`类型定义了特定的运算符或定义了特定的成员函数、具有特定的成员变量等等。

练习1: 尝试用上面给出的`max`函数计算`max(1ll,2.0)`, 观察编译器给出的错误信息。尝试修改`max`函数使得其能够正确处理`max(1ll,2.0)`。

一个类模板的例子, 支持不同类型的坐标的二维点类型.

```c++
template <typename T>
class Point{
    T x, y;
public:
    Point(T a, T b):x(a),y(b){
    }
};
```
可以使用`Point<int>` 和`Point<double>`分别表示整数坐标的点和浮点数坐标的点。

练习2: 尝试为`Point`模板类添加一些方法。尝试基于`Point`模板类来定义一个`Polygon`模板类, 通过顺序存储多边形边界上的所有点来表示一个多边形.

练习3: 尝试使用C++标准库中的`vector`, 使用`vector`重新实现你在[构造析构](Cons&Dest.md)里的练习2中实现的五子棋棋局类型。