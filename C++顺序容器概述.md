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
### 容器定义与初始化
#### 拷贝构造
当调用拷贝构造函数时，需要保证两个容器的类型及其元素类型必须匹配  
但当通过传递迭代器参数来拷贝一个范围时，就不要求容器类型相同，且新容器和原容器的元素类型也可以不同，只要能将要拷贝的元素转换  
```
#include<iostream>
#include<vector>

std::vector<double> dou{ 1.2,2.4,3.6,4.8,5.2,6.8,7.4,8.7,9.9,10.1 };

int main(void) {
	show(dou);
	std::cout << std::endl;
	std::vector<int> vec(dou.begin(), dou.end());
	show(vec);

	return 0;
}
```
```
//打印结果
1.2 2.4 3.6 4.8 5.2 6.8 7.4 8.7 9.9 10.1
1 2 3 4 5 6 7 8 9 10
```
结果如下，可以看到，通过拷贝构造，使得原本类型为double类的vector转换成为int类型的vector数组  
#### 列表初始化
通过一个值列表，我们不仅可以通过列表中的值对容器进行初始化，还可以隐式的指定容器的大小（当未指明容器的大小时）
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec{ 1,2,3,4,5,6,7,8 };

int main(void) {
	show(vec);
	std::cout << std::endl;
	std::cout << vec.size() << std::endl; //结果为列表中的元素个数 8

	return 0;
}
```
#### 与顺序容器大小相关的构造函数
除了以上的构造函数，容器还提供了一个接受容器大小和一个（可选的）元素的初始值，如果不提供元素初始值，则会执行值初始化（如内置类型int会被初始化为0）
```cpp
std::vector<int> vec(10,-1);
show(vec); //结果为10个-1
std::vector<std::string> str(10,"Hi");
show(str); //结果为10个Hi
```
注意：  
如果类类型是内置类型或具有默认构造函数的类类型，可以只为构造函数提供一个容器大小的参数  
如果元素类型没有默认构造函数，除了大小参数之外，还要提供一个显示的元素初始值

#### 标准库array具有固定大小
因为array的固定大小特性，所以在定义一个array类型时，必须要声明其类型和大小，而且一旦声明了大小，它将是一个不可更改的常量，所以array不支持插入和删除操作，因为这样会导致array容器大小的改变
```cpp
#include<array> //引入array的头文件
std::array<int, 10> arr{ 1,2,4,5,3,6,7,8,9,10 }; //初始化array容器，声明元素类型和容器大小
```
且该容器支持拷贝和赋值操作
```cpp
std::array<int, 10> arr{ 1,2,3,4,5,6,7,8,9,10 };
std::array<int, 10> ar = arr; //支持，只要保证数组类型匹配即可
```
### 赋值与swap
与内置数组不同，array支持赋值操作，但赋值等号两边必须是相同类型  
由于左右两边运算对象大小可能不同，所以array类型不支持assign和花括号赋值操作
#### assign操作（仅限顺序容器）
assign允许我们从一个不同但相容的类型赋值（即容器的类型可以不同，但元素类型应相同），或者从容器的一个子序列赋值。
```cpp
#include<iostream>
#include<vector>
#include<array>

std::vector<int> vec{ 1,2,3,4,5,6,7,8,9,10 };
std::vector<int> n_vec{ 1,2,3,4,5 };

int main(void) {
	show(n_vec);
	std::cout << std::endl;
	n_vec.assign(vec.begin(), vec.end());
	show(n_vec);
	return 0;
}
//结果为：
//1 2 3 4 5
//1 2 3 4 5 6 7 8 9 10
//assgin的第一个版本，接受两个迭代器，将目标容器中的元素全部替换成两个迭代器之间的值
//该操作可能改变目标容器中元素的个数。
```
assign的第二个版本是接受一个整数型和一个元素值。它用指定数目且具有相同给定值的元素替换容器中的原有的元素，这里还是有可能改变容器中元素的个数
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec{ 1,2,3,4,5,6,7,8,9,10 };
int main(void) {
	show(vec);
	std::cout << std::endl;
	vec.assign(5, 1); //这里改变了vec容器的大小
	show(vec);

	return 0;
}
```
#### swap函数
swap交换两个相同类型容器的内存
```cpp
#include<iostream>
#include<vector>

std::vector<int> vec{ 1,2,3 };
std::vector<int> s_vec{ 4,5,6,7 };

int main(void) {
	show(s_vec);
	std::cout << std::endl;
	show(vec);
	std::cout << std::endl;

	vec.swap(s_vec);
	show(s_vec);
	std::cout << std::endl;
	show(vec);
	std::cout << std::endl;

	return 0;
}
```
这里还要保证容器中的元素类型相同，才可以调用swap操作进行交换  
对于array容器来说，swap函数的具体实现与其他容器不同，其他容器只是交换了两个容器内部的数据结构，而并没有交换元素本身；而对于array容器来说，swap会真正交换它们的元素，所以交换两个array容器的时间与元素数量成正比
```cpp
#include<iostream>
#include<vector>
#include<array>

std::array<int, 10> ar{ 1,2,3,4,5,6,7,8,9,10 };
std::array<int, 10> arr{ 5,4,3,2,1,6,7,8,9,10 };

int main(void) {
	arr.swap(ar);
	return 0;
}
```
对于array的交换，不仅需要保证容器类型和元素类型相同，还要保证每个array中的元素个数相同，这样才能完成交换操作
