---
title: C++中的数据类型及其大小
date: 2019-07-29 19:58
updated: 2019-07-29 19:58
meta:
    date: false
categories: 
    - Computer Science
    - C & C++
tags:
    - C++
---

在C/C++中的数据类型及其所占空间大小总结

---

<!-- more -->

## 1. 内置数据类型

常见的内置数据类型如下表所示，可以使用sizeof查看其大小：

type | size
- | :-
char | 1B
unsigned char | 1B
signed char | 1B
int | 4B
unsigned int | 4B
signed int | 4B
short (int) | 2B
long (int) | 8B
float | 4B
double | 8B
long double | 16B
wchar_t | 2B or 4B

其中wchar_t是宽字符型，其定义为

```c++
typedef short int wchar_t;
```

也就是说，实际上wchar_t和short int是等价的。

## 2. 数组与字符串

对数组使用sizeof运算，其大小为**数组中元素的个数*元素所占内存大小**，而对于char数组型的字符串，例如
`char str[] = "Hello"`，其大小为字符个数+1，因为结尾需要保存**\0**。但需要注意的是，C++中的string类型的字符串其空间大小和char数组字符串不同，string是一个类，其声明的变量的大小是可变的，为了减少对内存的申请，一般在一开始声明变量时都会申请一块较大的内存，因此string类型的变量用sizeof得到的结果通常都比字符串中的字符数要大。

## 3. 自定义数据类型

### 3.1. class及struct

#### 3.1.1. 空类

对于一个空类，例如

```c++
class A
{

}
```

其大小为1B，因为C++中**每一个类都有一个独一无二的地址**，因此即使是空类也会为其分配1B的内存空间。

#### 3.1.2. 类对象所占内存大小

**类所占的内存大小由其成员变量决定，成员函数不计算在内**，但是如果一个struct、union或class B作为另一个类A的成员变量，这些玩意也不计入其内存大小，例如

```C++
// sizeof(A) = 4
class A
{
    int a;
};

// sizeof(B) = 4, 成员函数不计入
class B
{
    int a;
    int fun1() { return 0; }
};

// sizeof(C) = 1, 结构体不计入
class C
{
    struct stc1
    {
        int sa;
        char sb;
    };
};

// sizeof(D) = 1, 联合体不计入
class D
{
    union un1
    {
        int ua;
        short ub;
    };
};

// sizeof(E) = 1, class不计入
class E
{
    class cl1
    {
        int ca;
        short cb;
    };
};
```

#### 3.1.3. 类对象的字节对齐

由于不同的硬件对存储空间的处理方式不同，一些硬件对某些特定类型的数据只能**从某些特定地址开始存取**，如果不对其，会给存取效率带来损失。

字节对齐的细节和编译器的实现有关，一般而言，字节对齐遵守三个准则：

1. 结构体变量的首地址能够被其最宽基本类型成员的大小所整除；
2. 结构体每个成员相对于结构体首地址的偏移量都是成员大小的整数倍，如有需要，编译器会在成员之间加上填充字节；
3. 结构体的总大小为结构体最宽基本类型成员大小的整数倍，如有需要，编译器会在最末一个成员之后加上填充字节。

如果类中包含虚函数，则**无论其有多少个虚函数，这些虚函数所占内存均为4B**，因为内存中需要保存一个虚表指针成员

举例如下：

```C++
class A
{
    int i;
};

class B
{
    char ch;
};

class C
{
    int i;
    short s;
};

class D
{
    int i;
    short j;
    char ch;
};

class E
{
    int i;
    int ii;
    short j;
    char ch;
    char ch2;
};

class F
{
    int i;
    int ii;
    int iii;
    short j;
    char ch;
    char ch2;
};

struct G
{
    char ch;
    int i;
    short b2;
};

class H
{
    int i;
    virtual void fun1() {}
    virtual void fun2() {}
};

class I : public A, public B {};

class J : virtual public A {};

class K : virtual public A, virtual B {};

class L
{
    int i;
    static int ii;
}

// sizeof(A) = 4
// sizeof(B) = 1
// sizeof(C) = 4 + 1 + 3(规则3补齐) = 8
// sizeof(D) = 4 + 2 + 1 + 1(规则3补齐) = 8
// sizeof(E) = 4 + 4 + 2 + 1 + 1 = 12
// sizeof(F) = 4 + 4 + 4 + 2 + 1 + 1 = 16
// sizeof(G) = 1 + 3(规则2填充) + 4 + 2 + 2(规则3补齐) = 12
// sizeof(H) = 4 + 4 = 8 (!!!在Mac中，指针的大小为8，因此sizeof(H)= 4 + 4(规则2填充) + 8 = 16)
// sizeof(I) = 4 + 1 + 3(规则3填充) = 8 (继承也需要用相同规则进行字节对齐)
// sizeof(J) = 8
// sizeof(K) = (4 + 4) + (1 + 4) + 3(规则3对齐) = 16
// sizeof(L) = 4 (静态成员变量和全局变量一样存放于静态存储区中，被每一个类的实例共享，不计入类空间中)
```

### 3.2. union

联合体的大小取决于该体中**所有的成员中占用空间最大的一个成员的大小**，并且同样的需要对齐：

```C++
union u1
{
    double a;
    int b;
};

union u2
{
    char a[13];
    int b;
};

union u3
{
    char a[13];
    char b;
};

// sizeof(u1) = 8;
// sizeof(u2) = 13 + 3(以int的整数倍补齐);
// sizeof(u3) = 13;
```
