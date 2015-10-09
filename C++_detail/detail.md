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

 1. malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。
 2. 对于非内部数据类型的对象而言，光用maloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。 
 3. 因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以一个能完成清理与释放内存工作的运算符delete。注意new/delete不是库函数。
 4. C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存。
 5. new可以认为是malloc加构造函数的执行。new出来的指针是直接带类型信息的。而malloc返回的都是void指针。


####malloc/alloc/realloc的区别

malloc调用形式为(类型*)malloc(size)：在内存的动态存储区中分配一块长度为“size”字节的连续区域，返回该区域的首地址。 

calloc调用形式为(类型*)calloc(n，size)：在内存的动态存储区中**分配n块**长度为“size”字节的连续区域，返回首地址。 

realloc调用形式为(类型*)realloc(*ptr，size)：将ptr内存大小增大到size。(也可以缩小，缩小的内容消失)。

另外malloc 只管分配内存，并不能对所得的内存进行初始化，所以得到的一片新内存中，其值将是随机的。calloc在动态分配完内存后，自动初始化该内存空间为零。

realloc有个细节需要注意：

 1.如果当前连续内存块足够 realloc 的话，只是将p所指向的空间扩大，并返回p的指针地址。 这个时候 q 和 p 指向的地址是一样的。

 2.如果当前连续内存块不够长度，再找一个足够长的地方，分配一块新的内存，q，并将 p指向的内容 copy到 q，返回 q。并将p所指向的内存空间删除。

这样也就是说 realloc 有时候会产生一个新的内存地址 有的时候不会。所以在分配完成后。我们需要判断下 p 是否等于 q。并做相应的处理。

这里有点要注意的是要避免 p = realloc(p,2048); 这种写法。有可能会造成 realloc 分配失败后，p原先所指向的内存地址丢失。

####智能指针\悬挂指针\野指针

智能指针是一种资源管理类，通过对原始指针进行封装，在资源管理对象进行析构时对指针指向的内存进行释放；通常使用引用计数方式进行管理。

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

造成上面结构体大小不一样的结果，是由于**字节对齐**造成的。字节对齐的目的是为了提高存取效率。字节对齐的规则如下：

（1）不同类型的对齐值不同，如下表：

| 类型     | 对齐值     |
| -------- |:----------:|
| char     | 1      |
| short    | 2      |
| int      | 4      |
| float    | 4      |
| double   | 4      |

（2）在结构体中使用成员最大的对齐值来对齐。

（3）程序员可以指定对齐值。使用宏 #pragma pack(n)来指定。例如下面的结构体D占用7个字节：

```c++
#pragma pack(1)
struct D
{
    int a;
    char b;
    short c;
};
// sizeof(D) = 7
```

## STL

####vector
在vector尾部插入数据效率高，在前部或中间插入数据效率低，因为需要把插入位置之后的数据都往后挪一次。

vector分配的内存一般会大于需要存储的数据量。

#### list
####deque
####sets/multisets
sets和multisets底层使用红黑树实现。不能对元素直接改值。而是先删除再插入。直接改值会导致红黑树中原来的次序错误。
#### map
####hash_map
####string库函数

## 调试工具

 1. gdb

## 单例模式
```C++
class Singleton
{
private:
 Singleton();
 Singleton(const Singleton &s);
 Singleton& operator=(const Singleton &s);
public:
 static Singleton* GetInstance()
 {
  static Singleton instance;
  return &instance;
 }
};
```
