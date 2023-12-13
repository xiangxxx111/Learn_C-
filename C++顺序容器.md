# C++顺序容器
## 容器库概览
### 顺序容器的迭代器
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec; //定义了一个vector容器vec，存放int数据类型

int main(void) {
	int value = 1;
	int n;
	std::cin >> n;
	vec.resize(n); //定义vector容器的大小

	for (int i = 1; i <= n; i++) {
		static std::vector<int>::iterator it = vec.begin(); //定义一个静态局部变量，并使其指向vector容器的首元素
    //这里使用点运算符来调用首元素迭代器
		*it = i; //迭代器可以理解为一个指针类型
		++it;
	}
	for (std::vector<int>::iterator n_it = vec.begin(); n_it != vec.end(); ++n_it) {
		std::cout << *n_it << std::endl;
	}
	std::cout << "*****************" << std::endl;
	for (std::vector<int>::reverse_iterator N_it = vec.rbegin(); N_it != vec.rend(); ++N_it) {
		std::cout << *N_it << std::endl;
	}
	return 0;
}
```
