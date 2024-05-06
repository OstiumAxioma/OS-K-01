---
Date: 2024-01-17
tags:
  - 计算机/编程语言
  - 计算机/编程语言/Cpp
ToolSet: 
KnowledgeLink:
---
## 什么是变量

变量其实只不过是程序可操作的存储区的名称。

在 C++ 中，有多种变量类型可用于存储不同种类的数据。

C++ 中每个变量都有指定的类型，类型决定了变量存储的大小和布局，该范围内的值都可以存储在内存中，运算符可应用于变量上。

变量的名称可以由字母、数字和下划线字符组成。它必须以字母或下划线开头。

> [!important]
> 大写字母和小写字母是不同的，因为 C++ 是大小写敏感的。

## 变量的名称

由字母/数字/下划线构成

数字不可以占首位
## 变量的命名

1. 小驼峰式命名法
	第一个单词首字母小写，其他单词首字母大写
	
    ```cpp
    int myName;
    ```
    
1. 帕斯卡命名法
	每个单词的第一个字母都要大写
	
    ```cpp
    int MyName;
    ```
    
1. 下划线命名法
	单词与单词之间通过下划线连接即可
	
    ```cpp
    int my_name;
    ```
    
1. 匈牙利命名法
	变量名=属性+类型+对象描述 m_(成员变量) p(指针) MyName(对象描述)
	
```cpp
int m_pMyName;
```

## 变量数据类型

### 整数类型 Double

|数据类型|二进制位数要求|数值范围|
|---|---|---|
|short|>16|-32768~+32767|
|int|>16|-2147483648~+2147483648|
|long|>32|-2147483648~+2147483648|
|long long|>64|-9223372036854775808~+-9223372036854775808|
|unsigned short|>16|0~+65535|
|unsigned int|>16|0~4294967295|
|unsigned long|>32|0~4294967295|
|unsigned long long|>64|0~18446744073709551615|

1 字节 = 8 位

编译器会按照这些类型分配内存空间

Unsigned = 无符号整型 （不包含负数时使用）

sizeof(数据类型/表达式) 命令可以获得变量占据的内存字节数

e.g.: sizeof(int); sizeof(score)

### 浮点类型 Float

任何带有小数性质的数据都可以称之为浮点数

由于计算机只能阅读二进制数，C++使用尾数与指数来存储浮点数

> 比如$10.12$用科学计数法表示为$1012×10^{-2}$，再转化为二进制表达$11101×2^{1001}$，此时C++就可以将其编译为尾数11101，指数1001的二进制浮点数

这使得C++能够只使用两个数（尾数与2的指数）就能储存浮点类型的数据

|数据类型|二进制位数要求|数值范围|指数/尾数/有效位数|
|---|---|---|---|
|float|>32|$-2^{128}$~$+2^{128}$|8/23/6~7|
|double|>48|$-2^{1024}$~$+2^{1024}$|11/52/15~16|
|long double|同上|同上|同上|

浮点数只能存储近似值

1 字节 = 8 位

### 字符类型 Char

处理文本类数据，使用整数数据编码得来

编码为8位整数编码

常用编码规则为ASCII（美国信息交换标准代码）

乘法和除法对Char类型意义不大，加法和减法可以用于查找ASCII表前后的字符

ASCII分为控制字符与显示字符两种，前者用于文本的换行排版等操作，后者为自然语言的表示字符

#### ASCII编码查询

| 二进制       | 十进制 |    | 显示字符名称／意义 |
|:----------|:----|:---|:----------|
| 0010 0000 |  32 | 20 | (space)   |
| 0010 0001 |  33 | 21 | !         |
| 0010 0010 |  34 | 22 |           |  

| 二进制       | 十进制 | 控制字符名称／意义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|:----------|:----|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0000 0000 |   0 | [https://zh.wikipedia.org/wiki/空字符（Null）](https://zh.wikipedia.org/wiki/%E7%A9%BA%E5%AD%97%E7%AC%A6%EF%BC%88Null%EF%BC%89)                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 0000 0001 |   1 | [https://zh.wikipedia.org/w/index.php?title=标题开始&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E6%A0%87%E9%A2%98%E5%BC%80%E5%A7%8B&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 0010 |   2 | [https://zh.wikipedia.org/w/index.php?title=本文开始&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E6%9C%AC%E6%96%87%E5%BC%80%E5%A7%8B&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 0011 |   3 | [https://zh.wikipedia.org/w/index.php?title=本文结束&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E6%9C%AC%E6%96%87%E7%BB%93%E6%9D%9F&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 0100 |   4 | [https://zh.wikipedia.org/w/index.php?title=傳輸结束&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%82%B3%E8%BC%B8%E7%BB%93%E6%9D%9F&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 0101 |   5 | [https://zh.wikipedia.org/w/index.php?title=请求&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E8%AF%B7%E6%B1%82&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 0000 0110 |   6 | [https://zh.wikipedia.org/w/index.php?title=確認回應&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E7%A2%BA%E8%AA%8D%E5%9B%9E%E6%87%89&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 0111 |   7 | [https://zh.wikipedia.org/w/index.php?title=响铃&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%93%8D%E9%93%83&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 0000 1000 |   8 | [https://zh.wikipedia.org/wiki/退格鍵](https://zh.wikipedia.org/wiki/%E9%80%80%E6%A0%BC%E9%8D%B5)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 0000 1001 |   9 | [https://zh.wikipedia.org/wiki/制表键#定位符号](https://zh.wikipedia.org/wiki/%E5%88%B6%E8%A1%A8%E9%94%AE#%E5%AE%9A%E4%BD%8D%E7%AC%A6%E5%8F%B7)                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 0000 1010 |  10 | [https://zh.wikipedia.org/wiki/換行](https://zh.wikipedia.org/wiki/%E6%8F%9B%E8%A1%8C)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 0000 1011 |  11 | [https://zh.wikipedia.org/w/index.php?title=垂直定位符號&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%9E%82%E7%9B%B4%E5%AE%9A%E4%BD%8D%E7%AC%A6%E8%99%9F&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                            |
| 0000 1100 |  12 | [https://zh.wikipedia.org/w/index.php?title=换页键&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E6%8D%A2%E9%A1%B5%E9%94%AE&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                          |
| 0000 1101 |  13 | CR (字符)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 0000 1110 |  14 | 取消变换（Shift out）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 0000 1111 |  15 | [https://zh.wikipedia.org/wiki/启用變换（Shift](https://zh.wikipedia.org/wiki/%E5%90%AF%E7%94%A8%E8%AE%8A%E6%8D%A2%EF%BC%88Shift) in）                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 0001 0000 |  16 | [https://zh.wikipedia.org/w/index.php?title=跳出数据通讯&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E8%B7%B3%E5%87%BA%E6%95%B0%E6%8D%AE%E9%80%9A%E8%AE%AF&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                            |
| 0001 0001 |  17 | [https://zh.wikipedia.org/w/index.php?title=設備控制&action=edit&redlink=1一（https://zh.wikipedia.org/w/index.php?title=XON&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E8%A8%AD%E5%82%99%E6%8E%A7%E5%88%B6&action=edit&redlink=1%E4%B8%80%EF%BC%88https://zh.wikipedia.org/w/index.php?title=XON&action=edit&redlink=1) [https://zh.wikipedia.org/w/index.php?title=啟用軟體速度控制&action=edit&redlink=1）](https://zh.wikipedia.org/w/index.php?title=%E5%95%9F%E7%94%A8%E8%BB%9F%E9%AB%94%E9%80%9F%E5%BA%A6%E6%8E%A7%E5%88%B6&action=edit&redlink=1%EF%BC%89)   |
| 0001 0010 |  18 | [https://zh.wikipedia.org/w/index.php?title=設備控制&action=edit&redlink=1二](https://zh.wikipedia.org/w/index.php?title=%E8%A8%AD%E5%82%99%E6%8E%A7%E5%88%B6&action=edit&redlink=1%E4%BA%8C)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 0011 |  19 | [https://zh.wikipedia.org/w/index.php?title=設備控制&action=edit&redlink=1三（https://zh.wikipedia.org/w/index.php?title=XOFF&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E8%A8%AD%E5%82%99%E6%8E%A7%E5%88%B6&action=edit&redlink=1%E4%B8%89%EF%BC%88https://zh.wikipedia.org/w/index.php?title=XOFF&action=edit&redlink=1) [https://zh.wikipedia.org/w/index.php?title=停用軟體速度控制&action=edit&redlink=1）](https://zh.wikipedia.org/w/index.php?title=%E5%81%9C%E7%94%A8%E8%BB%9F%E9%AB%94%E9%80%9F%E5%BA%A6%E6%8E%A7%E5%88%B6&action=edit&redlink=1%EF%BC%89) |
| 0001 0100 |  20 | [https://zh.wikipedia.org/w/index.php?title=設備控制&action=edit&redlink=1四](https://zh.wikipedia.org/w/index.php?title=%E8%A8%AD%E5%82%99%E6%8E%A7%E5%88%B6&action=edit&redlink=1%E5%9B%9B)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 0101 |  21 | [https://zh.wikipedia.org/wiki/確認失敗回應](https://zh.wikipedia.org/wiki/%E7%A2%BA%E8%AA%8D%E5%A4%B1%E6%95%97%E5%9B%9E%E6%87%89)                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 0001 0110 |  22 | [https://zh.wikipedia.org/w/index.php?title=同步用暫停&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%90%8C%E6%AD%A5%E7%94%A8%E6%9A%AB%E5%81%9C&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 0111 |  23 | [https://zh.wikipedia.org/w/index.php?title=區塊傳輸结束&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%8D%80%E5%A1%8A%E5%82%B3%E8%BC%B8%E7%BB%93%E6%9D%9F&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                            |
| 0001 1000 |  24 | [https://zh.wikipedia.org/w/index.php?title=取消&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%8F%96%E6%B6%88&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 0001 1001 |  25 | 连线介质中断                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 0001 1010 |  26 | [https://zh.wikipedia.org/wiki/替代字符](https://zh.wikipedia.org/wiki/%E6%9B%BF%E4%BB%A3%E5%AD%97%E7%AC%A6)                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 1011 |  27 | [https://zh.wikipedia.org/wiki/退出键](https://zh.wikipedia.org/wiki/%E9%80%80%E5%87%BA%E9%94%AE)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 0001 1100 |  28 | [https://zh.wikipedia.org/w/index.php?title=文件分割符&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E6%96%87%E4%BB%B6%E5%88%86%E5%89%B2%E7%AC%A6&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 1101 |  29 | [https://zh.wikipedia.org/w/index.php?title=群組分隔符&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E7%BE%A4%E7%B5%84%E5%88%86%E9%9A%94%E7%AC%A6&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 1110 |  30 | [https://zh.wikipedia.org/w/index.php?title=记录分隔符&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E8%AE%B0%E5%BD%95%E5%88%86%E9%9A%94%E7%AC%A6&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0001 1111 |  31 | [https://zh.wikipedia.org/w/index.php?title=单元分隔符&action=edit&redlink=1](https://zh.wikipedia.org/w/index.php?title=%E5%8D%95%E5%85%83%E5%88%86%E9%9A%94%E7%AC%A6&action=edit&redlink=1)                                                                                                                                                                                                                                                                                                                                                                                      |
| 0111 1111 | 127 | [https://zh.wikipedia.org/wiki/Delete字符](https://zh.wikipedia.org/wiki/Delete%E5%AD%97%E7%AC%A6)                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |  
    
直接使用时不需要考虑二进制编码，只需要用常量字符圈定自然语言即可

> 其中常量字符用单引号：’y’, ’x’ 即可表示自然语言中的 x 和 y

控制字符也被简化为转义序列

> 比如 ’/n’ 被用来表示换行

## 变量类型总结

1. 整数类型（Integer Types）：
    - `int`：用于表示整数，通常占用4个字节。
    - `short`：用于表示短整数，通常占用2个字节。
    - `long`：用于表示长整数，通常占用4个字节。
    - `long long`：用于表示更长的整数，通常占用8个字节。
2. 浮点类型（Floating-Point Types）：
    - `float`：用于表示单精度浮点数，通常占用4个字节。
    - `double`：用于表示双精度浮点数，通常占用8个字节。
    - `long double`：用于表示更高精度的浮点数，占用字节数可以根据实现而变化。
3. 字符类型（Character Types）：
    - `char`：用于表示字符，通常占用1个字节。
    - `wchar_t`：用于表示宽字符，通常占用2或4个字节。
    - `char16_t`：用于表示16位Unicode字符，占用2个字节。
    - `char32_t`：用于表示32位Unicode字符，占用4个字节。
4. 布尔类型（Boolean Type）：
    - `bool`：用于表示布尔值，只能取`true`或`false`。
5. 枚举类型（Enumeration Types）：
    - `enum`：用于定义一组命名的整数常量。
6. 指针类型（Pointer Types）：
    - `type*`：用于表示指向类型为`type`的对象的指针。
7. 数组类型（Array Types）：
    - `type[]`或`type[size]`：用于表示具有相同类型的元素组成的数组。
8. 结构体类型（Structure Types）：
    - `struct`：用于定义包含多个不同类型成员的结构。
9. 类类型（Class Types）：
    - `class`：用于定义具有属性和方法的自定义类型。
10. 共用体类型（Union Types）：
    - `union`：用于定义一种特殊的数据类型，它可以在相同的内存位置存储不同的数据类型。

## 变量的声明

### 作用域

#### 什么是作用域

一般来说有三个地方可以定义变量：

- 在函数或一个代码块内部声明的变量，称为**局部变量**。
- 在函数参数的定义中声明的变量，称为**形式参数**。
- 在所有函数外部声明的变量，称为**全局变量**。

作用域是程序的一个区域，变量的作用域可以分为以下几种：

- **局部作用域**：在函数内部声明的变量具有局部作用域，它们只能在函数内部访问。局部变量在函数每次被调用时被创建，在函数执行完后被销毁。
- **全局作用域**：在所有函数和代码块之外声明的变量具有全局作用域，它们可以被程序中的任何函数访问。全局变量在程序开始时被创建，在程序结束时被销毁。
- **块作用域**：在代码块内部声明的变量具有块作用域，它们只能在代码块内部访问。块作用域变量在代码块每次被执行时被创建，在代码块执行完后被销毁。
- **类作用域**：在类内部声明的变量具有类作用域，它们可以被类的所有成员函数访问。类作用域变量的生命周期与类的生命周期相同。

<aside> 💡 如果在内部作用域中声明的变量与外部作用域中的变量同名，则内部作用域中的变量将覆盖外部作用域中的变量

</aside>

#### 局部变量

在函数或一个代码块内部声明的变量，称为局部变量。它们只能被函数内部或者代码块内部的语句使用。下面的实例使用了局部变量：

```cpp
#include <iostream>
using namespace std;
 
int main ()
{
  // 局部变量声明
  int a, b;
  int c;
 
  // 实际初始化
  a = 10;
  b = 20;
  c = a + b;
 
  cout << c;
 
  return 0;
}
```

特征是变量的声明写在代码块内部，也就是大括号内部

#### 全局变量

在所有函数外部定义的变量（通常是在程序的头部），称为全局变量。全局变量的值在程序的整个生命周期内都是有效的。

全局变量可以被任何函数访问。也就是说，全局变量一旦声明，在整个程序中都是可用的。下面的实例使用了全局变量和局部变量：

```cpp
#include <iostream>
using namespace std;
 
// 全局变量声明
int g;
 
int main ()
{
  // 局部变量声明
  int a, b;
 
  // 实际初始化
  a = 10;
  b = 20;
  g = a + b;
 
  cout << g;
 
  return 0;
}
```

在程序中，局部变量和全局变量的名称可以相同，但是在函数内，局部变量的值会覆盖全局变量的值。下面是一个局部变量和全局变量重名的实例：

```cpp
#include <iostream>
using namespace std;
 
// 全局变量声明
int g = 20;
 
int main ()
{
  // 局部变量声明
  int g = 10;
 
  cout << g;
 
  return 0;
}
```

> [!warning]
> 一定要赋予初值
> 切勿使用未被赋予初值的变量


### C++变量规范化案例

```cpp
#include <iostream> 

int main()
{
	int score = 1;
	int random;
	//此处random变量不像score一样被赋予了一个初始值，因此会报错

	std::cout << "Random: " << random;
	std::cin.get();

	score = 1 + score;

	std::cout << "New Score: " << score;
	std::cin.get();

	return 0;
}
```

如何避免这种情况？参照下面的代码：

```cpp
#include <iostream> 

int main()
{
	using namespace std;
	//全部使用std命名空间，下面不需要单独用::命名空间了
	count.setf(ios_base::floatfield, ios_base::fixed)
	//使用ios_base命名空间里的floatfield指令来显示浮点位, fixed命令来显示十进制位数
	float f_time = 1.0 / 3.0f;
	//增加小写f代表float，转化数据类型为浮点数
	double d_time = 1.0 / 3.0;
	long double ld_time = 1.0 / 3.0;
	//添加两个整数数据来做对比

	std::cout << "Float: " << f_time * 10e6 << endl; //endl自动换行
	std::cout << "Double: " << d_time * 10e6 << endl;
	std::cout << "Long Double: " << ld_time * 10e6 << endl;
	std::cin.get();

	return 0;
}

输出结果
Float: 3333333.432674 //7位有效数字之后不再有精度
Double: 3333333.333333
Long Double: 3333333.333333
```

### C++常量和变量的运用

```cpp
#include <iostream> 

int main()
{
	using namespace std;

	const float SPEED = 600.0f;

	float realSpeed = 0.0f; 
	//因为常量不可在运行过程中修改，一般只会调用其数值
  //比如此处定义一个realSpeed变量，并用SPEED常量乘以一个系数来赋值
	realSpeed = SPEED * 2.0f;

	cout << realSpeed;

	cin.get();

	return 0;
}
```

### C++变量修饰符

C++ 允许在 **char、int 和 double** 数据类型前放置修饰符。

修饰符是用于改变变量类型的行为的关键字，它更能满足各种情境的需求。

下面列出了数据类型修饰符：

- signed：表示变量可以存储负数。对于整型变量来说，signed 可以省略，因为整型变量默认为有符号类型。
    
- unsigned：表示变量不能存储负数。对于整型变量来说，unsigned 可以将变量范围扩大一倍。
    
- short：表示变量的范围比 int 更小。short int 可以缩写为 short。
    
- long：表示变量的范围比 int 更大。long int 可以缩写为 long。
    
- long long：表示变量的范围比 long 更大。C++11 中新增的数据类型修饰符。
    
- float：表示单精度浮点数。
    
- double：表示双精度浮点数。
    
- bool：表示布尔类型，只有 true 和 false 两个值。
    
- char：表示字符类型。
    
- wchar_t：表示宽字符类型，可以存储 Unicode 字符。
    

修饰符 **signed、unsigned、long 和 short** 可应用于整型，**signed** 和 **unsigned** 可应用于字符型，**long** 可应用于双精度型。

这些修饰符也可以组合使用，修饰符 **signed** 和 **unsigned** 也可以作为 **long** 或 **short** 修饰符的前缀。例如：**unsigned long int**。

> [!NOTE]
> C++ 允许使用速记符号来声明**无符号短整数**或**无符号长整数**。您可以不写 int，只写单词 **unsigned、short** 或 **long**，**int** 是隐含的。例如，下面的两个语句都声明了无符号整型变量。

```cpp
signed int num1 = -10; // 定义有符号整型变量 num1，初始值为 -10
unsigned int num2 = 20; // 定义无符号整型变量 num2，初始值为 20

short int num1 = 10; // 定义短整型变量 num1，初始值为 10
long int num2 = 100000; // 定义长整型变量 num2，初始值为 100000

long long int num1 = 10000000000; // 定义长长整型变量 num1，初始值为 10000000000

float num1 = 3.14f; // 定义单精度浮点数变量 num1，初始值为 3.14
double num2 = 2.71828; // 定义双精度浮点数变量 num2，初始值为 2.71828

bool flag = true; // 定义布尔类型变量 flag，初始值为 true

char ch1 = 'a'; // 定义字符类型变量 ch1，初始值为 'a'
wchar_t ch2 = L'你'; // 定义宽字符类型变量 ch2，初始值为 '你'
```

为了理解 C++ 解释有符号整数和无符号整数修饰符之间的差别，我们来运行一下下面这个短程序：

```cpp
#include <iostream>
using namespace std;
 
/* 
 * 这个程序演示了有符号整数和无符号整数之间的差别
*/
int main()
{
   short int i;           // 有符号短整数
   short unsigned int j;  // 无符号短整数
 
   j = 50000;
 
   i = j;
   cout << i << " " << j;
 
   return 0;
}
```

## 常量

### 常量的概念

程序运行中不会发生变化的量

符号常量可以给常量命名方便调用，命名后的常量被称为符号常量

常量具有只读的特性

符号常量名首字母一般大写来与变量名做区分
### 定义常量

在 C++ 中，有两种简单的定义常量的方式：

- 使用 **#define** 预处理器。
- 使用 **const** 关键字。

下面是使用 `#define` 预处理器定义常量的形式：

```cpp
#define identifier value
```

```cpp
#include <iostream>
using namespace std;
 
#define LENGTH 10   
#define WIDTH  5
#define NEWLINE '\n'
 
int main()
{
 
   int area;  
   
   area = LENGTH * WIDTH;
   cout << area;
   cout << NEWLINE;
   return 0;
}
```

使用 **const** 前缀声明指定类型的常量，如下所示：

```cpp
const type variable = value;
```

```cpp
#include <iostream>
using namespace std;
 
int main()
{
   const int  LENGTH = 10;
   const int  WIDTH  = 5;
   const char NEWLINE = '\n';
   int area;  
   
   area = LENGTH * WIDTH;
   cout << area;
   cout << NEWLINE;
   return 0;
}
```
