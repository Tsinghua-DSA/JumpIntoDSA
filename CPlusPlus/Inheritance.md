# 继承
## Inheritance

继承，可以让一个类“是”另一个类的子类型。

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

我们定义了新类型Point3D, 它“继承”了Point。一个Point3D对象内部“包含”一个Point对象和额外的成员变量z。

练习1: 看看sizeof(Point3D)的大小。想想为什么是这么大。