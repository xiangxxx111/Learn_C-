# C++String容器操作
## 构造String容器的其他方法
```cpp
string str(char_arr,n); //这里的str_arr表示一个const char*数组，n表示拷贝该数组中前n个元素到string容器中
string str(str2,pos2); //这里str是string类型str2从下标pos2开始的字符拷贝，应该保证pos2小于str2.size()
string str(str3,pos2,len); //这里同上面一样，只不过指定了len长度的拷贝数量
```
注意，这里的string类型同const char*一样，在末尾有空字符的存在，但在计算size时不计入其中  
且当拷贝的字符数量大于size时，拷贝只会进行到size长度，不会更多

### substr操作
```cpp
string str;
str.substr(pos,n);
//返回一个string类型，包含str中从pos开始的n个字符的拷贝。pos默认值为0，n默认值为str.size()-pos，即拷贝从pos开始的所有字符
```
考虑到sbustr返回一个string类型的子序列（有可能是它本身），所以可以调用来进行子序列的拷贝构造

```cpp
#include<iostream>
#include<string>

std::string str{ "abcde" };
std::string str1;
int main(void) {
	str1 = str.substr(); //这里调用了substr的默认值，将整个str的元素赋值给了str1
	std::cout << str1 << std::endl;
	std::cout << str << std::endl;
	return 0;
}
```
## 改变string容器的其他方法
除了支持普通顺序容器的操作之外，string容器还提供了额外的insert和erase版本  
这个版本的insert和erase不同于之前的接受迭代器的版本，它可以接受下标，指出要操作的具体位置（这是一个左闭合区间）
```cpp
#include<iostream>
#include<string>

std::string str = "abcdefg";
std::string s = "abc";

int main(void) {
	str.insert(str.size(), 5, 'H'); //注意，这里插入的应当是一个字符常量，而不能是字符串
  str.erase(str.size()-3 , 3); /删除str末尾的三个字符
	return 0;
}
```








