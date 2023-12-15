# C++顺序容器概述
## C++顺序容器分类
1. vector  可变大小数组。支持快速随机访问。支持快速尾插和尾删
2. deque  双端队列。支持快速随机访问。支持快速尾插/删和头插/删
3. list  双向链表。只支持双向顺序访问。支持指定位置的快速插入和删除
4. forward_list  单向链表。只支持单向顺序访问。支持指定位置的快速插入
5. array  固定大小数组。支持快速随机访问。不允许插入或删除元素。
6. string  与vector相似。但用于字符的存储，支持快速随机访问，支持快速尾插和尾删
## 容器库概览
容器允许嵌套使用  
```cpp
vector<vector<int>> //容器vector内嵌套vector，可以看作一个二维数组
```
### 迭代器（iterator,可以认为是一个指针类型）
#### begin和end
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec{ 1,2,3,4,5,6,7,8,9,10 };

void n_show(std::vector<int>) {
	for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) { //begin指向vector容器的首元素，end指向vector容器的尾元素的下一个元素
		std::cout << *it << ' ';
	}
}

void r_show(std::vector<int>) {
	for (std::vector<int>::reverse_iterator it = vec.rbegin(); it != vec.rend(); ++it) { //reverse_iterator是反向迭代器，对应的是rbegin和rend
		std::cout << *it << ' ';
	}
}

int main(void) {
	
	n_show(vec);
	std::cout << std::endl;
	r_show(vec);
	return 0;
}
```
这里的迭代器和反向迭代器是一个容器类型成员，可以用作用域运算符来访问迭代器类型
#### 迭代器类型
1. 普通迭代器  std::vector<int>::iterator
2. 反向迭代器  std::vector<int>::reverse_iterator
3. 常量迭代器  std::vector<int>::const_iterator(可以改变迭代器指向位置，但不能通过迭代器改变元素的值)
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec{ 1,2,3,4,5,6,7,8,9,10 };
void cshow(std::vector<int>) {
	for (std::vector<int>::const_iterator it = vec.cbegin(); it != vec.cend(); ++it) {
		std::cout << *it << ' ';
		//*it = 10; 无法通过一个const_iterator来改变元素的值
	}
}

int main(void) {
	cshow(vec);
	return 0;
}
```
