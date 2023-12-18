# 隐式类型转换
## 算术转换
目的是将一种算术类型转换成另一种算术类型。
### 整型提升
负责将小整数类型转换成较大的整数类型，如bool、char、signed char、unsigned char、short和unsigned short等类型，可能会被转换成int或long long int等整数类型，如bool类型的true和false，就分别对应1和0
## 其他隐式类型转换
### 数组转换成指针
一般在大多数情况下，数组名一般表示首元素的地址
```cpp
int example[10] = {1,2,3,4,5,6,7,8,9,10};
int* p_exam = example; //这里的p_exam指向数组的首元素1
```
注意，当数组被用作decltype关键字的参数，或者作为取地址（&）、sizeof 及 typeid等运算符的运算对象是，上述转换不会发生。
