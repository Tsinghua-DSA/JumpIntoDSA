# 类和对象 
## Class and Object

类和对象是比较基本的语法特性。我们通过例子来学习，讲解一段用到类和对象的C++代码。

在这之前，请确保你可以编译运行C++代码。

如果你哪里没看懂，可以查书，或者查搜索引擎，然后可以找人问。

```c++
#include<cstdio>

class Point{
    int x,y;
public:
    void set_xy(int a, int b){
        x = a;
        y = b;
    }
    void print_xy(){
        printf("%d %d\n",x,y);
    }
};

int main(){
    Point p1,p2;
    p1.set_xy(1,2);
    p2.set_xy(3,4);
    p1.print_xy();
    p2.print_xy();
}
```

这段代码中，定义了`Point`类, 在main函数里定义了p1, p2两个`Point类型的对象`, 或者也可以叫做两个`Point对象`, 或者简称为两个`Point`.

`Point`类拥有两个int类型的成员变量x, y

`Point`类拥有两个成员函数（也叫"方法") `set_xy()`和`print_xy()`, 可以分别用来设置`Point`里面x,y的数值和打印x,y的数值。

练习1: 尝试在main函数加入一句`printf("%d",p1.x)`, 并尝试理解一下编译错误信息。

练习2：尝试通过`sizeof`获取一个Point对象占据的空间大小。

练习3：为point对象定义一个新的成员函数，返回x+y的值。然后在主函数里调用这个成员函数。

练习4：尝试定义一个新的类Segment,它拥有两个Point类型的成员变量，代表一条线段。定义成员函数以修改Segment的成员变量。