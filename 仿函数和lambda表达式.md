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
注意：模板中的参数列表和返回类型可以省略，但是必须永远包含捕获列表和函数体。  
这里的返回类型指的是lambda表达式所要求的尾置返回类型，对于大括号中的return语句，不受影响，因为它属于函数体中的内容。
```cpp
#include<iostream>
auto func = [] {return 10; };
// 这里的func函数省略了参数列表和返回类型，对于器返回类型，只要它不是复杂类型，编译器可以推导出返回类型

int main(void) {
	std::cout << func() << std::endl;
	return 0;
}
```
下面再演示一下捕获列表内不为空的情况
```cpp
#include<iostream>

auto add = [c = 30](int a, int b) ->decltype(a + b) {return a + b + c; };
// 注意，这里捕获列表中的值，只限于在函数体中才能使用，也就是必须在大括号内的内容才可以使用捕获列表中的值
// 捕获列表中不可以使用全局变量或静态变量
// 捕获列表中的值无需声明类型，编译器会自动推导器类型
int main(void) {
	int ans = add(10, 20);
	std::cout << ans << std::endl;
	return 0;
}
```
### 捕获列表
捕获列表的使用分为三种形式
#### 1. 值捕获
```cpp
#include<iostream>
	int main(void) {
	int x = 5;
	auto lambda = [x]() {std::cout << x << std::endl;};
	return 0;
}
```
使用外部变量的值创建lambda的副本。捕获后，lambda函数体内不能修改这些变量的值。  
注意，这里只是外部变量的拷贝而非引用  
  
#### 2. 引用捕获
```cpp
#include<iostream>
int main(void) {
	int y = 10;
	auto lambda_ref = [&y]() {y++; std::cout << y << std::endl; }; //无return语句，默认返回void
	return 0;
}
```
使用外部变量的引用。捕获后，lambda函数体内可以修改这些变量的值。  
注意，是在函数体内修改  
  
#### 3. 隐式捕获
###### (1) 按值隐式捕获
```cpp
#include<iostream>
int main(void) {
	int a = 3, b = 7;
	auto lambda_implicit = [=]() {std::cout << a + b << std::endl;};

	return 0;
}
```
##### (2) 按引用隐式捕获
```cpp
#include<iostream>
int main(void) {
	int aa = 10;
	int& a = aa;
	auto lambda_implicit_ref = [&]() {std::cout << a << std::endl; };

	lambda_implicit_ref();
	return 0;
}
```
 由捕获列表的=或&表示。= 表示按值捕获，& 表示按引用捕获。通过使用 = 或 & ，可以捕获所有可见的外部变量。  
 这两种捕获同上述捕获的特性一样，比如引用捕获可以修改捕获列表的值
 
