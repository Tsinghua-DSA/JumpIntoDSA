# 命令行参数

C/C++的main函数有一种特殊的写法，用来接受“命令行参数"。

```c
int main(int argc, char** argv){
    // ...
    return 0;
}
```

“命令行参数”，是通过命令行执行程序的时候可以传入的参数。

例如用`./main 123`的命令执行程序，就给程序传入了一个命令行参数“123”。

在命令行用不同方式运行下面的程序，就可以理解命令行参数的用法。

```c
#include<cstdio>
int main(int argc, char** argv){
    printf("%d\n",argc);
    for(int i=0;i<argc;++i){
        printf("%s\n",argv[i]);
    }
    return 0;
}
```
编译这个程序(例如`g++ a.cpp -o a`), 然后在命令行用下列方式运行这个程序，观察程序输出即可。

```bash
./a 
./a 123
./a 1 2 3
./a 12 3
../tmp/a 123  #假设编译出的文件位于一个叫做tmp的文件夹内, 可以将tmp改成实际的目录名称
```