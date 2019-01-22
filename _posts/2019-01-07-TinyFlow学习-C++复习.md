---
layout:     post
title:      TinyFlow学习-C++复习
subtitle:   框架学习
date:       2019-01-07
author:     Dzw
header-img: img/tag-bg-o.jpg
catalog: 	 true
tags:
    - C++
    - TinyFlow
---





## TinyFlow学习-C++复习



### 构造函数

```c++
构造函数是类的一种特殊的成员函数，在每次创建类的新对象时执行。
构造函数的名称与类的名称完全相同，不会返回任何类型，也不会返回void。
可以为某些成员变量设置初始值

class	Line {			
public:						
	void	setLength(	double	len	);						
	double	getLength(	void	);						
	Line();		//	这是构造函数
private:						
	double	length; };

```

### 析构函数

```c++
析构函数是类的一种特殊的成员函数，他会在删除创建的对象时执行。
析构函数的名称与类的名称是完全相同的，只是在前面加了一个~作为前缀，不返回任何值，
不带任何参数。析构函数有助于跳出程序（关闭文件，释放内存等）前释放资源


class	Line {			
public:						
    void	setLength(	double	len	);						
    double	getLength(	void	);						
    Line();			//	这是构造函数声明						
    ~Line();		//	这是析构函数声明
private:						
    double	length; };

```

### 拷贝构造函数

```c++
拷贝构造函数是一种特殊的构造函数，创建对象时是使用同一类中之前创建的对象来初始化新创建的对象。
常用于以下场景：
*通过使用另一个同类型的对象来初始化新创建的对象。
*复制对象把它作为参数传递给函数。
*复制对象，并从函数返回这个对象。
classname (const classname &obj){
//函数主体
}
```

### C++接口（抽象类）

### C++多态

```
当类之间通过继承来关联时会用到多态。
c++多态意味着调用成员函数时会根据调用对象的类型来执行不同的函数。
```

### 虚函数

```
在基类中使用virtual声明的函数
```

### default关键字

```c++
default关键字的作用显示声明该函数自动生成函数体，即默认函数，不过仅适用于累的特殊成员函数，且该成员函数没有默认参数。default使用

class  A{
public:
    A() = default；
};
```

### final关键字

```c++

// final，(1)作用在虚函数：表示此虚函数已处在“最终”状态，后代类必定不能重写这个虚函数。
//        (2)作用在类：表示此类必定不能被继承

```

### posix_memalign函数

```
int posix_memalign (void **memptr,
                    size_t alignment,
                    size_t size);
预对齐内存的分配
调用posix_memalign( )成功时会返回size字节的动态内存，并且这块内存的地址是alignment的倍数。参数alignment必须是2的幂，还是void指针的大小的倍数。返回的内存块的地址放在了memptr里面，函数返回值是0.

调用失败时，没有内存会被分配，memptr的值没有被定义
```

### namespace

```c++
namespace内的成员都位于相同的作用域中，无须特殊符号即可相互访问，而从namespace外访问他们就需要显式符号。
using std::string //用 string 表示 std::string

在一个函数中可以安全使用using指示以方便符号表示，但是对于全局using指示必须谨慎，避免过度使用造成名字冲突。
```

### static const

```
const 常量的在超出其作用域的时候会被释放，但是 static 静态变量在其作用域之外并没有释放，只是不能访问。

static 修饰的是静态变量，静态函数。对于类来说，静态成员和静态函数是属于整个类的，而不是属于对象。可以通过类名来访问，但是其作用域限制于包含它的文件中。

static 变量在类内部声明，但是必须在类的外部进行定义和初始化。

const 常量在类内部声明，但是定义只能在构造函数的初始化列表进行。

如果想让 const 常量在类的所有实例对象的值都一样，可以用 static const (const static)
```

### 内联函数

```
c++内联函数通常与类一起使用，如果一个函数是内联的，那么在编译时编译器会把该函数的代码副本放在每个调用该函数的地方。
对内联函数进行任何修改都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧函数。
定义内联函数，需要在函数名前面放置关键字inline,调用函数前需要对函数进行定义。
inline int Max(int x, int y){
	return (x > y)? x : y
}//用内联函数来返回最大值
```

### 函数声明

```c++
声明语句不仅把类型和名字关联起来，大多数声明同时也是定义。大多数声明同时也是定义，可以把定义看作是一种特殊的声明，定义提供了在程序中使用该实体所需的一切信息。并且实体需要内存空间存储某些信息时，定义语句预留了内存。

double sqrt(double); //函数声明
extern int error_number; //变量声明
struct User;  //类型名字声明
```

### 指针与const

```
默认情况下const只在一个文件内有效，为了解决该问题，只需要在前面添加extern，
就能作用于多个文件。


通常有两种方式将const关键字作用于指针。
1.让指针指向一个常量对象，可以防止使用该指针来修改所指向的值。
2.将指针本身声明为常量，这样可以防止改变指针所指向的位置。

例子：
int age = 39;
const int *pt = &age;
pt指向一个const int ,因此不能使用pt来修改这个值。即*pt值为const
但是，pt的声明并不意味着它所指向的值是常量，而仅仅对于pt而言它是const类型。
因此可以直接修改age,但不能通过*pt来修改age
```

### const常见用法

```c++
const T
//定义一个常量，声明的同时必须初始化，一旦声明，这个值将不能改变
int i = 5;
const int constInt = 10;        //正确
const int constInt2 = i;        //正确
constInt = 20;                  //错误，常量值不可被改变
const int constInt3;            //错误，未被初始化


const T*
//指向常量的指针，不能用于改变其所指向的对象的值
const int i = 5;
const int i2 = 10;
const int* pInt = &i;           //正确，指向一个const int对象，即i的地址
//*pInt = 10;                   //错误，不能改变其所指缶的对象
pInt = &i2;                     //正确，可以改变pInt指针本身的值,此时pInt指向的是i2的地址
const int* p2 = new int(8);     //正确，指向一个new出来的对象的地址
delete p2;                      //正确
//int* pInt = &i;               //错误，i是const int类型，类型不匹配，不能将const int * 初始化为int *
int nValue = 15;
const int * pConstInt = &nValue;    //正确，可以把int *赋给const int *，但是pConstInt不能改变其所指向对象的值，即nValue
*pConstInt = 40;                    //错误，不能改变其所指向对象的值



const int* 与 int* const的区别
//指针本身就是一种对象，把指针定义为常量就是常量指针，也就是int* const的类型，也可以写成int *const，声明时必须初始化。
int nValue = 10;
int* const p = &nValue;
int *const p2 = &nValue;
//const int* 指针指向的对象不可以改变，但指针本身的值可以改变；int* const 指针本身的值不可改变，但其指向的对象可以改变。
int nValue1 = 10;
int nValue2 = 20;
int* const constPoint = &nValue1;
//constPoint = & nValue2;           //错误，不能改变constPoint本身的值
*constPoint = 40;                   //正确，可以改变constPoint所指向的对象，此时nValue1 = 40
 
const int nConstValue1 = 5;
const int nConstValue2 = 15;
const int* pPoint = &nConstValue1;
//*pPoint  = 55;                    //错误，不能改变pPoint所指向对象的值
pPoint = &nConstValue2;             //正确，可以改变pPoint指针本身的值，此时pPoint邦定的是nConstValue2对象，即pPoint为nConstValue2的地址



const int* const
//指向常量对象的常量指针，既不可以改变指针本身的值，也不可以改变指针指向的对象
const int nConstValue1 = 5;
const int nConstValue2 = 15;
const int* const pPoint = &nConstValue1;
//*pPoint  = 55;                    //错误，不能改变pPoint所指向对象的值
//pPoint = &nConstValue2;           //错误，不能改变pPoint本身的值



const T&
//对常量(const)的引用，又称为常量引用，常量引用不能修改其邦定的对象。
int i = 5;
const int constInt = 10;
const int& rConstInt = constInt;    //正确，引用及邦定的值都是常量
rConstInt = 5;                      //错误，不能改变引用所指向的对象
//允许为一个常量引用邦定一个非常量对象、字面值，甚至是表达式；引用的类型与引用所指向的类型必须一致。
int i = 5;
int& rInt = i;                      //正确，int的引用
const int constInt = 10;
const int& rConstInt = constInt;    //正确，引用及邦定的值都是常量
const int& rConstInt2 = rInt;       //正确，用rInt邦定的对象进行赋值
rInt = 30;                          //这时，rConstInt2、rInt、i的值都为30
//rConstInt2 = 30;                  //错误，rConstInt2是常量引用，rConstInt2本身不能改变所指向的对象
 
int i2 = 15;
const int& rConstInt3 = i2;         //正确，用非常量的对象为其赋值
const int& rConstInt4 = i + i2;     //正确，用表达式为其赋值,值为45
i = 20;                             //此时i=20, rInt = 20, rConstInt4 = 45,说明rConstInt4邦定的是i + i2的临时变量
const int& rConstInt5 = 50;         //正解，用一个常量值为其赋值


const T*&与T *const&
//指向常量对象的指针的引用，这可以分两步来理解：1.const T*是指向常量的指针；2.const T*&指向常量的指针的引用。
const int nConstValue = 1;                      //常量对象
const int nConstValue2 = 2;                     //常量对象
const int* pConstValue = &nConstValue;          //指向常量对象的指针
const int* pConstValue2 = &nConstValue2;        //指向常量对象的指针
const int*& rpConstValue = pConstValue;         //指向常量对象的指针的引用
//*rpConstValue = 10;                           //错误，rpConstValue指向的是常量对象，常量对象的值不可改变
rpConstValue = pConstValue2;                    //正确，此时pConstValue的值等于pConstValue2
//指向常量对象的指针本身是对象，引用可以改变邦定对象的值
 
int nValue = 5;
int nValue2 = 10;
int *const constPoint = &nValue;                //常量指针
int *const constPoint2 = &nValue2;              //常量指针
int *const &rpConstPoint = constPoint;          //对常量指针的引用,邦定constPoint
//rpConstPoint = constPoint2;                   //错误，constPoint是常量指针，指针本身的值不可改变
*rpConstPoint = 20;                             //正确，指针指向的对象可以改变


//总结
// 指向const的指针（指针指向的内容不能被修改）const关健字总是出现在*的左边而const指针（指针本身不能被修改）const关健字总是出现在*的右边，那不用说两个const中间加个*肯定是指针本身和它指向的内容都是不能被改变的。
```

 

### 函数指针

```c++
//typedef 常见形式
typedef existing_type new_type_name ;
int i;
typedef int myInt;
myInt j;

//函数指针常见形式

//定义一个函数指针pFun,指向一个返回类型为char,有一个整形的参数的函数

char (*pFun)(int);
char glFun(int a) {return ;}
pFun = glFun;
(*pFun)(2);


//typedef 可以让函数指针更直观方便

typedef char(*PTRFUN)(int);//定义了一个PTRFUN类型

PTRFUN pFun;
char glFun(int a){return ;}

pFun = glFun;
(*pFun)(2);
```



### define

```c++
#define 标识符 字符串 //用标识符来取代字符串
```



### Protected

```
一个类使用protected来声明是希望 与派生类分享但不想被其他公共访问使用的成员。
所以protected看作是public private的中间产物。
```

