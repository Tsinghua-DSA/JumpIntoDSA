# 引用
## Reference

"引用"有些时候可以用来代替指针，是更具有可读性的语法特性。如果你不熟悉指针, 可以先去自学一下指针。

例如, 我们希望一个函数修改它的参数.

```c++
#include<cstdio>
void increment(int &x){//传递了"x的引用"作为参数
    x++;
}
void increment_wrong(int x){//传递了"x的数值"作为参数, x的数值被复制了一次
    x++;
}
void increment_pointer(int *p){//传递了"指向某个变量地址的指针"作为参数
    *p++;
}
int main(){
    int a=0;
    increment(a);
    printf("%d\n",a);
    increment_wrong(a);
    printf("%d\n",a);
    increment_pointer(&a);
    printf("%d\n",a);
}
```

练习1: 猜测上面程序的输出结果. 运行一下, 看看结果和你想的是否一致. 程序中哪里有错误? 尝试用`-Wall`编译选项编译上面的程序。你可以查查你的编译器/IDE如何设置"编译选项"。

练习2: 实现一个Int类型,里面包含一个整数x, 重载它的`++`运算符, 重新定义它的拷贝构造函数, 在拷贝构造函数里打印"copy constructor is called"。把上面的程序三个函数参数里的int改为Int，然后看看上面的程序中拷贝构造函数被调用了几次。你也可以用类似的方法看看构造函数、析构函数被调用了几次。

练习3: 自学如何将一个数组作为函数的参数。然后编写一个模板函数，接受一个类型T的数组的起始地址(指针类型)和它的长度(int类型), 将这个数组里的内容前后反转。

推荐阅读: The C++ Programming Language Ch.7 Pointers, Arrays and References

