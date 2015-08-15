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

 2. list
 3. deque
 4. map
 5. hash_map
 6. string库函数

## 调试工具

 1. gdb
