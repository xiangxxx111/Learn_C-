# 参数绑定和bind函数
## bind函数
bind函数，可以看作是一个通用的函数适配器，它接受一个新的可调用对象来适应元对象的参数列表，定义在functional头文件中  
其调用模板为：
```cpp
auto new_object = std::bind(original_object,parameters_list);
// 这里的new_object是指bind函数生成的新的可调用对象，original_object是指函数调用之前的可调用对象，parameters_list是指参数列表
```
接下来我将举一个例子来解释bind函数的具体使用方法
```cpp
#include<iostream>
#include<functional> //引入bind头文件functional
#include<vector>
#include<algorithm>

std::vector<int> vec{ 0,1,2,3,4,5,6,7,8,9 };
bool is_bigger(int x, int target) {
	return x > target ? true : false; //这是原先的可调用对象，需要用bind函数进行修改
}
int main(void) {
	std::sort(vec.begin(), vec.end());
	int target = 0;
	std::cin >> target;

    // 使用了lambda表达式，相较于bind函数来说，lambda表达式更加简洁
    //std::cout << *(std::find_if(vec.begin(), vec.end(), [target](int x) { return x > target ? true : false; }));

	std::vector<int>::iterator it = std::find_if(vec.begin(), vec.end(), std::bind(is_bigger, std::placeholders::_1, target));

    // 注意，这里使用占位符_n时要声明器命名空间 std::placeholders::_n  

    // 这里的占位符中的n是与原先可调用对象的形参列表紧密相关的，这里的占位符_1表示is_bigger形参列表中的第一个int x，随后给出另一个参数target  

    // 这样通过bind函数实现了lambda表达式的功能，在find_if的一元谓词的情况下向可调用对象is_bigger中传递了两个参数  

	std::cout << *it << std::endl;
	std::cout << "The position of the bigger number is " << std::distance(vec.begin(), it) << std::endl;
	return 0;
}
```
## bind的参数
可以通过bind绑定给定的可调用对象中的参数或重新安排参数的顺序
### 绑定给定的可调用对象中的参数




### 重新安排参数的顺序
