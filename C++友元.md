# C++类中的友元
## 声明关键字
```cpp
template<class T>
friend T T_Name//这里的T表示声明的类型，T_Name表示类型名称
```
## 友元声明的作用
在一个类中，如果某个成员被private修饰而封装起来，对于外部的函数是无法访问到该成员信息的
但如果对外部访问函数进行友元修饰，函数就可以对类中的封装成员进行访问
```cpp
#include<iostream>

class Building {
	friend void visit(Building* building);//友元的声明必须包含函数的签名，不能只包含函数名
private:
	std::string m_Sroom;
	std::string m_Broom;
public:
	Building() {
		m_Broom = "卧室";
		m_Sroom = "客厅";
	}
};

void visit(Building* building) {//这里如果未声明visit为友元函数，对于Building函数中的私有成员是无法进行访问的
	std::cout << building->m_Sroom << std::endl;
	std::cout << building->m_Broom << std::endl;
}

void test01() {
	Building* b = new Building;
	visit(b);
}
int main(void) {
	test01();
	return 0;
}
```
## 友元的不同类型
### 类类型的友元
```cpp
#include<iostream>
class Add;

class members {
	friend void Add::add(members* m);
private:
	int a = 10;
	int b = 20;
};

class Add {
public:
	void add(members* m) {
		std::cout << m->a + m->b << std::endl;
	}
};

void test01() {
	Add a;
	auto m = new members;
	a.add(m);
}
int main(void) {
	test01();
	return 0;
}
```
### 成员函数的友元
