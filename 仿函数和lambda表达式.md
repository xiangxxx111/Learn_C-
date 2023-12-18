# 仿函数和lambda表达式
## 仿函数
仿函数，就是一种类类型，通过重载调用运算符"()"来实现像函数一样的调用功能，所以称之为仿函数
```cpp
#include<iostream>
class Add {
private:
	int a;
	int b;
public:
	int operator()(int a, int b) { //operator就是对调用运算符进行重载
		return a + b;
	}
};
int main(void) {
	Add add;
	add(10, 20); //像函数一样的调用，本质是一个类内的成员函数
	return 0;
}
```
## lambda表达式
```cpp
// lambda表达式实现加法运算
#include<iostream>

auto add = [](int a, int b) {return a + b; }; // lambda表达式，需要用auto来接收器返回值
// []中的值为捕获列表，表示一个局部变量，这里的捕获列表为空

// lambda表达式还可以与尾置返回相联系
auto n_add = [](int a, int b)->decltype(a + b) {return a + b;};

int main(void) {
	std::cout << n_add(10, 20);
	return 0;
}
```
lambda表达式的基本模板：
```cpp
auto function_name = [](int T, ...)->decltype(T) {return T; };
// 这里的function_name表示一个lambda表达式名，可以理解成一个函数名
// []中的值是捕获值列表，它表示一个lambda表达式所在函数中定义的局部变量的值的列表，通常为空
// ()中的值是lambda表达式的参数列表，可以理解成函数的参数列表
// 这里的->表示一个尾置返回一般用于decltype声明的返回类型和较为复杂的返回类型
// {}中的是lambda表达式的函数体，如果函数体只是一个return语句，则返回类型由返回的表达式来推断，否则，返回类型为void
```
