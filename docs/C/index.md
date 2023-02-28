# C语言专题讲解

这一部分讲解几个C语言中重要的概念，有助于大家对数据结构的学习。

包括: 指针、递归、命令行参数。  

如果有闲暇时间，推荐阅读《The ANSI C Programming Language》，这是一本200多页的小书，每天看一个小时，两个星期就可以看完。

一些零散知识:

### 函数的static变量

函数内可以定义static变量，它只初始化一次，只在这个函数内可用。你也可以把函数里的static变量理解为“专属这个函数使用的全局变量”。

在第一次进入函数时，static变量被初始化。第二次进入函数时，static变量的取值等于第一次退出函数时的变量取值。

可以执行下面的程序，看看结果如何。
```c
#include <cstdio>
void func(){
    static bool first = true;
    if(first){
        printf("first\n");
        first = false;
    }else{
        printf("not first\n");
    }
}
int main(){
    func();
    func();
}
```