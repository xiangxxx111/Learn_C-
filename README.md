# C++ 中的底层 const 和顶层 const
主要区别在于它们应用的位置和影响的范围

底层 const：

底层 const 是指指针或引用本身是 const 的情况。它影响的是指针或引用所指向的对象。
底层 const 表示指针或引用所指向的数据是常量，不能通过这个指针或引用来修改数据。
```cpp
const int value = 42;
const int* constPtr = &value;  // 底层const指针
```
在上述例子中，constPtr 是一个底层 const 指针，意味着不能通过 constPtr 来修改 value 的值。

顶层 const：

顶层 const 是指指针或引用所指向的数据是 const 的情况。它影响的是指针或引用所指向的对象的 const 属性。
顶层 const 表示数据本身是常量，不能通过其他非 const 的方式来修改数据。
```cpp
int value = 42;
const int* ptrToConst = &value;  // 顶层const指针
```
在上述例子中，ptrToConst 是一个顶层 const 指针，这意味着不能通过 ptrToConst 来修改 value 的值。

总体来说，底层 const 主要涉及指针或引用的常量性，而顶层 const 主要涉及数据本身的常量性。在处理函数参数、返回值、指针和引用时，理解这两种 const 是非常重要的，特别是在涉及到函数签名和函数重载时。
