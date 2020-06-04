---
title: 2020-6-4未命名文件 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## Chapter 1
### 1.2
1. 标准库定义了四个IO对象：
	* 标准输入(cin)
	* 标准输出(cout)
	* 标准错误（cerr）:默认情况下，写入到cerr中的数据是不缓冲的。
	* clog

```c++

#include<iostream>
int main(){

        std::cout<<"standard output"<<std::endl;
        std::clog<<"standard log"<<std::endl;
        std::cerr<<"standard error"<<std::endl;
        int a;
        std::cout<<"input a number:"<<std::endl;
        std::cin>>a;
        std::cout<<"input number is:"<<a<<std::endl;
}

```
2. 输入运算符`<<`:接受两个运算对象：
	* 左侧的必须是一个ostream对象。
	* 右侧的运算对象是要打印的值。
3. 输出运算符`>>``:
	* 左侧必须是istream.
	* 将从istream中读入的数据写入到右侧对象中。
3. 操纵符`endl`:效果是结束当前行，并将与设备关联的缓冲区中的内容刷到设备中。
4. 前缀`std::`指出名字`cout`和`endl`是定义在命名空间std中的。
	* 命名空间可以帮助避免不经意见的命名冲突。
	* 标准库中的所有命名都定义在std中。

5. `::`：作用域运算符，用于指出想使用哪个命名空间中的变量。

### 1.3 程序中的注释

### 1.4 控制流
#### 1.4.1 `while`语句
```cpp
#include<iostream>
int main(){
int a=10;
while(a>0){
        std::cout<<a<<std::endl;
        a--;
}
return 0;
}
```

#### 1.4.2 `for`语句
```cpp
#include<iostream>
int main(){
for(int a=0;a<10;a++){
        std::cout<<a<<std::endl;
}
return 0;
}

```
1. for 循环执行流程：
	* 创建并初始化变量
	* 检测循环条件是否满足，若满足继续，不满足退出循环
	* 执行循环体
	* 执行累加操作
	* 跳转到第二步

### 1.4.3 读取不确定数量的数字
```cpp
   #include<iostream>
int main(){
int a;
while(std::cin>>a){
        std::cout<<"intput is "<<a<<std::endl;
}
std::cout<<"finished input"<<std::endl;
return 0;
}
```
> 在Linux系统中，使用`Ctrl+D`来输入文件结束符。
> 在Windows系统中，使用`Ctrl+Z`来输入文件结束符。

1. 编译器：编译器可以用于帮助检查形式上的错误。