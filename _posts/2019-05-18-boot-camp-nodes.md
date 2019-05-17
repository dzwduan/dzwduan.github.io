---
layout:     post
title:      15-213-c-programming
subtitle:   c-learn
date:       2019-05-18
author:     Dzw
header-img: img/tag-bg-o.jpg
catalog: 	 true
tags:
    - C
    - csapp

---



# 变量声明 和 限定符

## 全局声明

> 定义外部函数，能被所有文件看见
>
> 在另一个文件中使用 extern 关键字来使用全局声明



## Const声明

> 用于不会改变的变量
>
> 存储在只读数据部分



## 静态声明

> ■在本地，保持调用之间的值
>
> ■使用要SPARINGLY（很少）
>
> ■注意：静态在引用函数时具有不同的含义（在目标文件外部不可见）



# 类型转换

## Integer Casting

> signed<->unsigned   : 二进制比特位不变，只是换了解释的方式
>
> small->large               :符号拓展

注意点

> 使用 int x = (int) y;
>
> 向下转换会造成截断数据
>
> 跨指针类型转换：解引用一个指针可能会导致未定义的内存访问



## Pointers Casting

> int*    char*    int**
>
> 通过解引用来访问值（例如* a）。 可用于读取或写入给定地址的值
>
> 解引用NULL会导致未定义行为（通常是段错误）

指针运算

> 会自动scale



# 值调用 vs 引用调用

## 值调用

> 对传递给函数的参数所做的更改不会反映在调用函数中
>
> 即形参，实参不影响

## 引用调用

> 对传递给函数的参数所做的更改将反映在调用函数中
>
> 形参影响实参

c语言是值调用

为了让函数外的值改变，使用指针。



# Arrays/Strings

Arrays

> 数组：相同类型元素的固定大小集合

Strings

> 空字符（'\ 0'）终止字符数组
>
> ■空字符告诉我们字符串结束的位置
>
> ■字符串上的所有标准C库函数都假定为空字符终止。



# Struct

> 放置在一个内存块下面的一组值的集合
>
> 对于结构实例  ： 使用 .操作符访问内容
>
> 对于结构指针  ： 使用 ->操作符



# Stack  Heap  Data

> 高地址
>
> |						stack
>
> | 							.
>
> |                             .
>
> |						heap
>
> |						uninitalized data
>
> |						initialized data
>
> |						text
>
> 低地址



> ■局部变量和函数参数放在栈上
>
> ■在变量离开作用域后取消内存分配
>
> ■不返回指向栈分配变量的指针！
>
> ■不要在其范围之外引用变量的地址！ 
>
> ■通过调用malloc / calloc分配的内存块放在堆上



> 例子：
>
> ■ int* a = malloc(sizeof(int)); 
>
> ■ //a是指针，存在栈上，指向存在堆上的内存块



# Malloc, Free ,Calloc

都是在堆上处理动态内存

```c
void* malloc (size_t size)
//只分配内存块而不初始化
    
  
void* calloc (size_t num, size_t size)
//分配内存块，内存初始为0
    
    
void free(void* ptr)
//释放内存块
   
```



内存管理规则

> 有malloc就有free
>
> 分配多少就释放多少
>
> 只free一次
>
> 必要时才malloc



# valgrind

内存检查工具



# Debug

GDB



# 头文件

> <>	引用标准库
>
> “  ”    引用自己写的库

避免头文件多次引用的方法

```c
#ifndef A_H
#define A_H


#end if
```



# 宏定义

> ■没有函数调用开销
>
> ■像在文本编辑器中一样考虑“查找和替换”
>
> #define MAX(A,B)     ((A)>(B)  ?  (A) : (B))



# C Library

## String.h

> 所有string 都以 '\0' 结尾
>
> 确保分配空间足够大
>
> 确保src / dest不重叠！

```c
void *memset (void *ptr, int val, size_t n);
//分配内存并初始化


void *memcpy (void *dest, void *src, size_t n);
//内存复制


char *strcpy (char *dest, char *src); 
//字符串复制


char *strncpy (char *dest, char *src, size_t n); 
//复制n个字符，其中包括'\0';如果src比n-1长，复制就不包括'\0'


char *strcat (char *dest, char *src);
//字符串拼接 ，顺序为 dest src


char *strncat (char *dest, char *src, size_t n);
//从src中取n个拼接到dest后面，最后添加'\0'



int strcmp(char *str1, char *str2);
//字符串比较，先比ascii码再比长度


int strncmp (char *str1, char *str2, size_t n);
//比双方的前n个



char *strstr (char *haystack, char *needle); 
//返回指向haystack中第一次出现needle的指针，如果未找到任何匹配则返回NULL。



char *strtok (char *str, char *delimiters);
//使用分隔符中提供的任何分隔符字符破坏性地标记str。
//➢每次调用都会返回下一个标记。 第一次调用后，继续使用str = NULL调用。 如果没有更多令牌，则返回NULL。 
//➢不可重入。



size_t strlen (const char *str);
```



## stdlib.h

```c
 int atoi(char *str); 
 //string to int
 
 
 int abs(int n);
 //绝对值
 
 
 void exit(int status); 
 //终止过程➢将状态返回给父进程
 
 
 void abort(void)
 //终止过程异常
     
     
     
//size_t,64位无符号类型，适用于不同的机器
```



# stdio.h

```c
FILE *fopen (char *filename, char *mode);
//mode(read, write, append)



int fclose (FILE *stream);
//关闭流，失败返回EOF


char *fgets (char *str, int num, FILE *stream)
//从流读取最多数字1个字符到std;换行符或EOF停止; 追加终止'\0';错误时返回str或NULL


//读取
    
int scanf (char *format, ...); 
//format describes types of values to read


int fscanf (FILE *stream, char *format, ...); 



int sscanf (char *str, char *format, ...);



//输出

int printf (char *format, ...);



int fprintf (FILE *stream, char *format, ...); 



int sprintf (char *str, char *format, ...); 



int snprintf (char *str, size_t n, char *format, ...);
```



placeholder

> ■ %d: signed integer 
>
> ■ %u: unsigned integer 
>
> ■ %x: hexadecimal 
>
> ■ %f: floating-point
>
> ■ %s: string (char *)
>
> ■ %c: character 
>
> ■ %p: pointer address
>
> 
>
> ■ %ld for long 
>
> ■ %lf for double
>
> ■ %zu for size_t
>
> ■ h: short 
>
> ■ l: long
>
> ■ ll: long long 
>
> ■ z: size_t 

