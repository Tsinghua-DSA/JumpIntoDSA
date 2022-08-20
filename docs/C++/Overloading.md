# 运算符重载
## Overloading

运算符重载是一种使用符号进行简写的方法。`x + y * z` 比起 `multiply y by z and add the result to x` 更加清晰。

对于我们自定义的类型，比如一个有理数类，我们可以用成员函数来表示加减乘除等运算。但如果能直接使用加减乘除等符号，会更加合适。

再比如，如果我们自定义了一个新的动态数组类型，直接使用数组下标`[]`也会很方便。

如果我们想对我们自定义的类型使用C++的标准输入输出`cin`和`cout`来输入输出，就需要重载对应的`<<` `>>`操作符。

下面给出一个例子，定义了一个复数类，为复数类之间的相加重载了加法运算符。
```c++
class complex { // very simplified complex
    double re, im;
public:
    complex(double r, double i) :re{r}, im{i} { }
    complex operator+(complex);//以成员函数的形式声明运算符: complex + complex
    //第一个complex表示的是+运算符的返回类型为complex, operator+ 可以理解为“函数名”, (complex)是参数列表
    complex operator*(complex);//以成员函数的形式声明运算符: complex * complex
};
complex complex::operator+(complex operand2){
	return complex(re + operand2.re, im+operand2.im);
}
complex complex::operator*(complex operand2){
	return complex(re * operand2.re - im*operand2.im, re * operand2.im - im*operand2.re);
}
int main()
{
    complex a = complex{1,3.1};
    complex b {1.2, 2};
    complex c {b};
    a = b+c; //b+c相当于进行了这样的函数调用: b.operator+(c)
    b = b+c*a;//相当于b = b.operator+(c.operator*(a))
    c = a*b+complex(1,2);
    return 0;
}
```
如果对运算符重载感兴趣，建议阅读*the C++ Programming Language*的第18章 *Overloading*和第19章*Special Operators*

练习1: 通过运算符重载, 为`complex`类型实现和`double`类型之间的加法、乘法重载; 

练习2: 通过运算符重载, 实现用`cin`, `cout`来输入和输出为`3+4i`格式的复数

练习3：通过运算符重载，实现`+=`, `*=`运算符

练习4:(来自TC++PL的练习题) 你可以把自己“混淆”之后的程序发到我们的微信群里让大家看看。

    [7] (∗2) Write a program that has been rendered unreadable through use of operator overloading
    and macros. An idea: Define + to mean − and vice versa for INTs. Then, use a macro to
    define int to mean INT. Redefine popular functions using reference type arguments. Writing
    a few misleading comments can also create great confusion.
    [8] (∗3) Swap the result of §X.19[7] with a friend. Without running it, figure out what your
    friend’s program does. When you have completed this exercise, you’ll know what to avoid.