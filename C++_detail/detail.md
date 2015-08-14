# C++语言细节

## 关键字

 1. static
 2. const
 3. extern
 4. explict
 5. volatile
使用valatile来防止编译器对变量的操作进行优化。一般在三种场景下可以使用volatile：1.变量要存在硬件寄存器中；2.中断服务的程序中要访问的变量；3.多线程环境中要共享的变量。

一个变量可以同时是const和volatile类型，表示程序不应该改变变量的值，但是这个变量的值可能会被别的情况改变，因此编译器不要对变量的操作进行优化。

## 内存管理

 1. malloc\free
 2. new\delete
 3. 智能指针\悬挂指针\野指针
 4. 内存泄露

## STL

 1. vector
 2. list
 3. deque
 4. map
 5. hash_map
 6. string库函数

## 调试工具

 1. gdb
