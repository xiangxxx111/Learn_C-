#  C++类的初探
## 定义成员函数
成员函数的声明必须要在类内，但其定义可以在类内或类外实现
```cpp
#include<iostream>
class print {
public:
  int a = 10;
  int b = 20;
	void show_a(const int& a) {
		std::cout << a << std::endl;
	}//类内声明，类内定义

	void show_b(const int& b);//类内声明

};

void print::show_b(const int& b) {
	std::cout << b << std::endl;//类外定义
}

int main(int argc, char* argv[]) {
	print a;
	a.show_a(10);

	a.show_b(20);
	return 0;
}
```

## 类中的this指针
当我们试图通过类类型访问类中成员时，通常使用点运算符（成员访问运算符）来实现，这里实际上发生了一次名为this的隐式参数来访问类对象
```cpp
#include<iostream>

class print {
public:
	int a = 10;
	int b = 20;
	inline int Get_Value() {
		return a;//这里是一个类成员函数，它得到a的值，返回int类型
    //完整代码应为return this->a;
    //这里因为this为指针类型，所以不能使用普通的点运算符
	}
};


int main(int argc, char* argv[]) {
	print a;
	int ans = a.Get_Value();
	std::cout << ans << std::endl;
	return 0;
}
```
针对上例，这只是一次简单的类成员函数的调用，但是却调用了一个隐式指针参数this对类成员a进行调用
所以不建议声明一个名称为this的参数或对象，这可能导致错误
