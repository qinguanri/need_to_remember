# C++语言细节

## 关键字

#### static

####const

####extern

####explict

####volatile

使用volatile来防止编译器对变量的操作进行优化。一般在三种场景下可以使用volatile：1.变量要存在硬件寄存器中；2.中断服务的程序中要访问的变量；3.多线程环境中要共享的变量。

一个变量可以同时是const和volatile类型，表示程序不应该改变变量的值，但是这个变量的值可能会被别的情况改变，因此编译器不要对变量的操作进行优化。

####public/protected/private

public声明的成员可以被所有人访问；protected声明的成员可以被创建者及其子类访问；private只能被创建者和类的内部成员函数访问。

## 内存管理

####malloc\free

####new\delete

####智能指针\悬挂指针\野指针

####内存泄露

####字节对齐

>看下面这段代码：

```c++
struct A
{
    int a;
    char b;
    short c;
};

struct B
{
    char a;
    int b;
    short c;
};

int main() {
    cout << sizeof(A) << "   "<< sizeof(B) << "   "<<endl;
    return 0;
}
----output:
8 12

```

造成上面结构体大小不一样的结果，是由于**字节对齐**造成的。字节对齐的目的是为了提高存取效率。


## STL

 1. vector
 2. list
 3. deque
 4. map
 5. hash_map
 6. string库函数

## 调试工具

 1. gdb
