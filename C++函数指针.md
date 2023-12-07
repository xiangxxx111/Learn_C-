### 函数指针的声明
```cpp
returnType (*pointerName)(parameterType1,parameterType2,...);
//returnType 是函数的返回类型
//pointerName 是函数指针的名称
//parameterType1,parameterType2 等是函数的形参类型
//注意，这里形参类型不一定需要提供形参名
```
##### 无形参类型
```cpp
int (*funcPtr)();
```
##### 有形参类型
```cpp
int (*sum_Ptr)(int,int);//int类型形参
```
```cpp
void (*show_Ptr)(const char*);//char*类型形参
```
##### 使用typedef简化函数指针的声明
```cpp
typedef int(*def_func_ptr)();//typedef简化函数声明
def_func_ptr func();//使用简化后的声明来定义函数指针
```
### 函数指针的使用
```cpp
#include<iostream>

int add(const int a,const int b){
    return a+b;
}

int main(int argc,char* argv[]){

    //声明一个函数指针，指向add函数
    int (*addPtr)(const int,const int) = add;

    //需要保证传入实参与函数形参列表相符
    const int a = 3;
    const int b = 4;
    //为函数指针传参
    int result = addPtr(a,b);
      
    std::cout<<result<<std::endl;
}
```
