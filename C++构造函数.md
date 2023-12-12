# 类中的构造函数
## 构造函数初始值列表
### 初始化和赋值操作的不同
初始化：初始化是在创建变量时为其赋予一个初始值的过程。
```cpp
int x(42); //直接初始化
int x = y; //拷贝初始化,其中 y 可以是另一个已经存在的变量或一个表达式。
int x{42}; //列表初始化（C++11及以后版本）
```
赋值：赋值是在变量已经存在的情况下，为其赋予一个新的值的过程。
赋值使用赋值运算符 =
### 构造函数初始化实例
```cpp
#include<iostream>

class Print {
	friend void show(Print p);
public:
	Print(int val1,int val2) :a(val1), b(val2) {};//初始化const变量a和b，初始化列表
private:
	const int a;
	const int b;
};
void show(Print p) {
	std::cout << p.a << ' ' << p.b << std::endl;
}
void test01() {
	Print p(1, 2);
	show(p);
}
int main(void) {
	test01();
	return 0;
}
```
### 成员初始化的顺序
成员的初始化顺序和它们在类定义中出现的顺序一致，而与构造函数初始化列表无关，其只是用来说明用于初始化成员的值
```cpp
#include<iostream>
class X {
	friend void show(X x);
private:
	int i;
	int j;
public:
	X(int val) :j(val), i(j) {};
};
void show(X x) {
	std::cout << x.i << ' ' << x.j << std::endl;
}
void test01() {
	X x(10);
	show(x);
}
int main(void) {
	test01();
	return 0;
}
```
这里初始化顺序为i->j,所以i使用的是未经初始化的j的值。
### 默认实参和构造函数
我们可以通过构造函数来重写一个默认构造函数，所以当我们为类中的所有参数都提供了默认实参，则它实际上也定义了默认构造函数，因为默认构造函数就是在未为类中参数提供初始值时对该参数赋予初始值，当我们为所有参数都提供了初始值，就相当于我们重新定义了一个默认构造函数

## 委托构造函数
```cpp
class Print {
public:
	Print(int x, int y) :m_x(x), m_y(y) {};
	Print() :Print(0, 0) {};
  Print(int x) :Print(x, 0) {};
private:
	int m_x;
	int m_y;
};
```
其实委托构造函数可以理解为构造函数的嵌套，上例中，我利用Print的有参构造函数定义了一个默认构造函数，其类似于一种构造函数的嵌套。
## 默认构造函数的作用
默认初始化在以下情况下发生

1. 在块作用域内不使用任何初始值定义一个非静态变量或数组
2. 当一个类本身含有类类型的成员且使用合成的默认构造函数（由编译器提供的默认构造函数）
3. 当类类型的成员没有在构造函数初始值列表中显示的初始化时
### 静态变量的初始化
1. 在全局或命名空间作用域下，默认初始化值为0
2. 函数内部作用域下，不会执行默认初始化
### 某些数据成员缺少默认构造函数
```cpp
#include <iostream>

class NoDefaultConstructor {
private:
    int a;
    double b;

public:
    //NoDefaultConstructor() = default;//为该函数提供一个默认构造函数
    NoDefaultConstructor(int x, double y) : a(x), b(y) {}//初始化a和b的值为x、y，这是类的有参构造函数
    //在提供了有参构造函数的版本后，编译器就不会在提供任何默认构造函数了
    void show() {
        std::cout << "a: " << a << ", b: " << b << std::endl;
    }
};

int main() {
    //引发错误，该类没有编译器提供的默认构造函数
    //因为声明了一个有参构造函数，编译器不会再提供任何默认构造函数
    //当然也可以通过 NoDefaultConstructor() = default; 来提供一个默认构造函数
    NoDefaultConstructor obj;  // This line will cause a compilation error
    obj.show();
    return 0;
}
```
### 使用默认构造函数
这里应当注意默认构造函数的使用声明
```cpp
Myclass example();
std:cout<< example.members <<std::endl;//错误，这里的使用声明将example定义为了一个函数类型

//正确的应当把声明时example后的括号删去
Myclass example;
std::cout<< example.members <<std::endl;
```
