# 继承
## Inheritance

继承，可以让一个类“是”另一个类的子类型。完成编程作业的时候，可以不需要使用继承的特性。也就是说，学习数据结构课程的时候，你能看懂用到继承的示例代码就可以。

```c++
#include<cstdio>
class Point{
    int x,y;
public:
    Point(int a, int b){//constructor
        x = a;
        y = b;
    }
    void print_xy(){
        printf("%d %d\n",x,y);
    }
};
class Point3D:Point{
    int z;
public:
    Point3d(int a,int b,int c):Point(a,b){
        z=c;
    }
    void print_xyz(){
        print_xy();
        printf("%d\n",z);
    }
}
int main(){
    Point p1(1,2);
    Point3D p2(3,4,5); 
    p1.print_xy();
    p2.print_xyz();
}
```

练习0:

这是一段具有语法错误的代码。你需要借助编译器/IDE修改里面的语法错误。你可以选中括号中内容查看提示(<font color="white">一处是大小写错误，一处是少写了分号</font>)。

我们定义了新类型`Point3D`, 它“继承”了`Point`。一个Point3D对象内部“包含”一个Point对象和额外的成员变量z。

练习1: 看看`sizeof(Point3D)`的大小。想想为什么是这么大。

练习2: 定义一个`Point4D`类型继承`Point3D`, 额外给它加一个int成员变量`w`.