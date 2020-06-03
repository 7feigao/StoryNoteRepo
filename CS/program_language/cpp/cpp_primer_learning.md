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
	* 标准错误（cerr）
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

3. 操纵符`endl`:效果是结束当前行，并将与设备关联的缓冲区中的内容刷到设备中。
	4. 前缀`std::`指出名字`cout`和`endl`是定义在命名空间std中的