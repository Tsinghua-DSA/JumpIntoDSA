# 递归

对本文档感到疑惑，可咨询liurd22艾特mails点tsinghua点edu点cn

为了较深入地理解递归，我们需要从函数调用栈的角度理解函数调用。

我们需要区分“一段函数的代码”和“一段函数的代码的一次执行”。

在我们心目中，CPU执行代码时，会保存一个“程序当前执行到的位置”，从而一句一句的执行代码。

每一次，从“程序当前执行到的位置”读出一句代码，执行之。随后，如果是普通指令，就把“程序当前执行到的位置”加一; 如果是跳转指令、循环指令等等，就把”程序当前执行到的位置“跳转到对应位置。

那么，如果遇到一个函数调用呢？除了修改”程序当前执行到的位置“到函数的开始，还需要做一些其他的工作。

CPU需要记录，函数是从哪里被调用的，从而在函数执行结束之后，跳转回来。

CPU需要将函数的参数拷贝一份，在被调用的函数里进行使用。

还需要分配一块内存区域，存储被调用的函数的局部变量。

内存区域总是需要分配的。主要的两种方式就是动态分配（new/delete）和通过函数调用栈分配。

为了完成以上所说的处理，每次调用一个函数的时候，都会在内存中为它分配一个栈帧。实际上，这个函数的局部变量、函数的参数、被调用的位置（返回时回去的位置）、以及调用它的函数的栈帧所在的位置，都在这个函数的栈帧中存储。我们说一个栈帧描述了一个函数调用的实例。

下面我们给一个函数嵌套调用的例子，看看“栈帧”是如何工作的。

```c++
int g(int d,int e, int f){
    int q = d*e*f;
    return q*q;
}
int h(int b, int c){
    return g(b,c,1) + 10;
}
int main(){
    int a = 1;
    int m = h(a,10);
    return 0;
}
```

首先，程序开始执行时，`main`函数自身具有一个栈帧（可以认为这是操作系统创建的）。这个栈帧中，包含了`main`函数结束之后需要跳转到的位置（操作系统设定的位置）、包含了局部变量`a,m`所需要的内存空间。

随后，当代码执行到调用函数`h(a,10)`的时候，程序在内存中创造一个新的栈帧，栈帧中包含函数`h()`的参数`b,c`所需的存储空间，包含函数`h()`此次被调用后将返回的位置(`main`函数中给`m`赋值的操作), 也包含`main`函数的栈帧所在的位置。因为一个程序某一时刻执行的状态，需要由“代码执行到的位置”和“当前栈帧”共同描述，因此函数返回后需要复原“当前栈帧”，因此函数中需要保存“调用它的函数的栈帧所在的位置”。

创建完栈帧、完成参数传递（将参数拷贝到函数`h()`的栈帧里），程序进行跳转，开始执行函数`h()`的代码。当前栈帧切换为`h()`的栈帧。

随后，函数`h()`执行到`return`语句时，需要先调用函数`g()`, 然后就会创建`g(b,c,1)`的栈帧, 里面包含了参数`d,e,f`和局部变量`q`所需的存储空间，包含了`h()`中调用`g()`的位置,也就是`g()`需要返回的位置, 还包含了`h()`的栈帧所在的位置。创建完栈帧， 就会跳转到函数`g()`的代码，当前栈帧切换为`g()`的栈帧。

随后, 函数`g()`完成内部的运算，销毁自己的栈帧，返回到函数`h()`, 当前栈帧切换为`h()`的栈帧，返回值被拷贝到`h()`的栈帧的临时变量中，“当前代码执行到的位置”跳转到`h()`中获取`g()`的返回值的语句。随后, `h()`完成将`g()`的返回值加10的运算，根据`h()`的栈帧，需要返回到`main()`函数中对应的位置。`h()`的栈帧销毁，返回值被拷贝到`main()`函数的局部变量`m`中，随后`main()`函数返回，销毁`main()`函数的栈帧，程序结束。

注意，整个过程中，CPU只知道”当前的栈帧是什么、当前正在执行的程序语句是什么“，程序的正确执行，依赖于每次函数调用都正确创建了新的栈帧并切换过去，且通过栈帧中保存的接下来要返回的位置，每个栈帧都能够成功返回之前的函数。

注意，上面的描述不是完全准确的，忽略了寄存器、编译优化的存在，但整体来说，正确反映了递归的实现原理。更加符合实际的机制，在学习计算机系统概论/汇编语言程序设计/计算机组成原理/计算机系统结构/编译原理等课程的时候，都有可能涉及到。

下面我们给一个函数递归调用的例子，看看“栈帧”是如何工作的。

```c++
int f(int x){
    int g;
    if(x<=0)g = 1;
    else g = f(x-1) + f(x-2);
    return g;
}
int main(){
    int m = f(2);
    return 0;
}
```

首先程序开始执行，创建`main()`函数的栈帧，包括局部变量`m`所需的内存空间。

然后程序创建`f(2)`这一次调用的栈帧，包括局部变量g所需的空间、此次函数调用将要返回的位置等。

然后当程序执行到`int g = f(x-1) + f(x-2)`时，C语言并没有规定`+`两侧的函数调用谁先执行、谁后执行。

第一种可能:

假设左侧的`f(x-1)`先执行，那么就会先执行`f(1)`, 创建`f(1)`的栈帧，在里面分配`f(1)`中局部变量`g`的空间，存储将来要返回到`f(2)`中的位置。

然后，`f(1)`要调用`f(0)`和`f(-1)`, 无论谁先调用，都是创建完栈帧，分配完局部变量g，就返回一个数值1回到原先的位置并销毁自己的栈帧。这个过程对`f(0)`和`f(-1)`会分别发生一次。

然后, `f(1)`就可以销毁自身的栈帧，返回到`f(2)`。接下来，`f(2)`调用一次`f(0)`,创建`f(0)`栈帧、分配局部变量g、销毁`f(0)`栈帧并返回数值1到`f(2)`中的位置，然后完成了`f(2)`对局部变量`g`的计算，`f(2)`的栈帧被销毁，数值返回到`main`中，赋值给局部变量`m`, 随后整个程序结束。


第二种可能:

假设在`f(2)`中右侧的`f(x-2)`先执行，那么首先，`f(2)`调用一次`f(0)`,创建`f(0)`栈帧、分配局部变量g、销毁`f(0)`栈帧并返回数值1到`f(2)`中的位置。

然后，执行`f(1)`, 创建`f(1)`的栈帧，在里面分配`f(1)`中局部变量`g`的空间，存储将来要返回到`f(2)`中的位置。

然后，`f(1)`要调用`f(0)`和`f(-1)`, 无论谁先调用，都是创建完栈帧，分配完局部变量g，就返回一个数值1回到原先的位置并销毁自己的栈帧。这个过程对`f(0)`和`f(-1)`会分别发生一次。

然后, `f(1)`就可以销毁自身的栈帧，返回到`f(2)`。然后完成了`f(2)`对局部变量`g`的计算，`f(2)`的栈帧被销毁，数值返回到`main`中，赋值给局部变量`m`, 随后整个程序结束。

你学会了吗？如果学会了，尝试把上面的例子的文字描述画成示意图，用一个矩形表示一个栈帧/函数的一次调用，用箭头表示函数调用的关系（从调用函数的栈帧指向被调用函数的栈帧）。
