---
date: 2024-01-17
tags:
  - 计算机/编程语言
  - 计算机/编程语言/Cpp
toolSet: 
knowledgeLink: []
---
## 什么是函数

函数是一组一起执行一个任务的语句。每个 C++ 程序都至少有一个函数，即主函数 `main()` ，所有简单的程序都可以定义其他额外的函数。

您可以把代码划分到不同的函数中。如何划分代码到不同的函数中是由您来决定的，但在逻辑上，划分通常是根据每个函数执行一个特定的任务来进行的。

函数**声明**告诉编译器函数的名称、返回类型和参数。函数**定义**提供了函数的实际主体。

C++ 标准库提供了大量的程序可以调用的内置函数。例如，函数 `strcat()` 用来连接两个字符串，函数 **`memcpy()`** 用来复制内存到另一个位置。

函数还有很多叫法，比如方法、子例程或程序，等等。

## 函数的定义

### 函数定义构成

在 C++ 中，函数由一个函数头和一个函数主体组成。下面列出一个函数的所有组成部分：

- **返回类型**return_type：一个函数可以返回一个值。`return_type` 是函数返回的值的数据类型。有些函数执行所需的操作而不返回值，在这种情况下，`return_type` 是关键字 `void`。
- **函数名称**function_name：这是函数的实际名称。函数名和参数列表一起构成了函数签名。
- **参数**parameter list：参数就像是占位符。当函数被调用时，您向参数传递一个值，这个值被称为实际参数。参数列表包括函数参数的类型、顺序、数量。参数是可选的，也就是说，函数可能不包含参数。
- **函数体**body of the function：函数主体包含一组定义函数执行任务的语句（也就是运算的逻辑）

```cpp
return_type function_name( parameter list )
{
   body of the function
}
```

下面是自定义一个简单函数并调用的实例：

```cpp
#include <iostream>
#include <string>

using namespace std;

//自定义函数
int ExampleFunction (int x)
{
	return x+1;
}

int main()
{
	//定义一个局部变量z并调用函数
	int z = ExampleFunction(5);
	cout << z << endl;

	return 0;
}
```

如果返回类型return_type为 `void` 则函数执行所需的操作不会返回值，此时在函数外部不能设置变量，而是简化掉设置变量的过程直接使用函数本身：

```cpp
#include <iostream>
#include <string>

using namespace std;

//自定义函数返回类型为void
void ExampleFunction (int x)
{
	//局部变量在函数内定义
	int z = x+1;
	cout << z << endl;
}

int main()
{
	//不定义局部变量直接调用函数
	ExampleFunction(5);

	return 0;
}
```

### 函数传参
#### **形参和实参**

如果把函数比喻成一台机器，那么参数就是原材料，返回值就是最终产品；从一定程度上讲，函数的作用就是根据不同的参数产生不同的返回值

C语言函数的参数会出现在两个地方，分别是函数定义处和函数调用处，这两个地方的参数是有区别的。

**形参（形式参数）**

在函数定义中出现的参数可以看做是一个占位符，它没有数据，只能等到函数被调用时接收传递进来的数据，所以称为<font color="#ff0000">形式参数</font>，简称<font color="#ff0000">形参</font>

**实参（实际参数）**

函数被调用时给出的参数包含了实实在在的数据，会被函数内部的代码使用，所以称为<font color="#ff0000">实际参数</font>，简称<font color="#ff0000">实参</font>。

> [!tip]
> 形参和实参的功能是传递数据，发生函数调用时，实参的值会传递给形参

```cpp
返回值类型 function(参数类型 形参) 
{
	操作形参;
}

main() 
{
	参数类型 实参;
	function(形参=实参);
}
```

#### 形参和实参的区别和联系

1. 形参变量只有在函数被调用时才会分配内存，调用结束后，立刻释放内存，所以形参变量只有在函数内部有效，不能在函数外部使用。
2. 实参可以是常量、变量、表达式、函数等，无论实参是何种类型的数据，在进行函数调用时，它们都必须有确定的值，以便把这些值传送给形参，所以应该提前用赋值、输入等办法使实参获得确定值。
3. 实参和形参在数量上、类型上、顺序上必须严格一致，否则会发生“类型不匹配”的错误。当然，如果能够进行自动类型转换，或者进行了强制类型转换，那么实参类型也可以不同于形参类型。
4. 函数调用中发生的数据传递是单向的，只能把实参的值传递给形参，而不能把形参的值反向地传递给实参；换句话说，一旦完成数据的传递，实参和形参就再也没有瓜葛了，所以，在函数调用过程中，形参的值发生改变并不会影响实参。
5. 形参和实参虽然可以同名，但它们之间是相互独立的，互不影响，因为实参在函数外部有效，而形参在函数内部有效

### 参数的传递

**值传递：**

形参是实参的拷贝，改变形参的值并不会影响外部实参的值。从被调用函数的角度来说，值传递是单向的（实参->形参），参数的值只能传入，不能传出。

> [!important] 使用情景
> 当函数内部需要修改参数，并且不希望这个改变影响调用者时，采用值传递。

**引用传递：**

形参相当于是实参的“<mark class="hltr-red">别名</mark>”，对形参的操作其实就是<mark class="hltr-red">对实参的操作</mark>，在引用传递过程中，被调函数的形式参数虽然也作为局部变量在栈中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址。被调函数对形参的任何操作都被处理成间接寻址，即通过栈中存放的地址访问主调函数中的实参变量。

> [!important] 使用情景
> 被调函数对形参做的任何操作都影响了主调函数中的实参变量。

**指针传递：**

形参为指向实参地址的指针，当对形参的指向操作时，就相当于对实参<mark class="hltr-red">本身</mark>进行的操作

## 函数原型/前置声明

一般使用函数前需要完全定义函数名称，形式参数，返回类型和函数体

这种方法并不适用于大型项目的管理

### 函数原型

函数原型相当于一个简化过的函数声明，函数体位置不在函数定义中

可以放在独立的文件内，需要时再调用（比如头文件header.h）

由于其位置在程序执行的前部，所以也叫前置声明

### 函数原型的声明方式

函数原型的声明需要一下几个成分：

1. 函数名
2. 形式参数数量
3. 顺序和类型
4. 返回值类型

函数原型并不是没有函数体而是被拆分开来

下面是函数原型和传统函数调用的对比

### 传统调用

```cpp
#include <iostream>

using namespace std;

int CustomAdd(int a, int b)
{
	return a + b;
}

int main()
{
	int x = 0;
	
	x = CustomAdd(2, 3);

	cout << x << endl;
}
```
*调用函数的常规方式，定义函数体并在main函数中调用*
### 函数原型

```cpp
#include <iostream>

using namespace std;

int CustomAdd(int, int);

int main()
{
	int x = 0;
	
	x = CustomAdd(2, 3);

	cout << x << endl;
}

```
*函数原型的调用简化了函数体*

```cpp
int CustomAdd(int a, int b)
{
	return a + b;
}
```
*函数体（函数的实现implementation）被放在程序的后半部分，或者是其他的cpp文件中，而函数原型/前置声明被放在header文件中统一管理*

### 编译器中创建函数声明对应的函数体

在visual studio中，创建函数声明时序列栏会出现扳手图标🪛，点击后会自动在代码末尾创建空的函数定义（函数体）的框架方便编辑

![[Pasted image 20240505235957.png|center]]
## 函数重载
函数重载是指编译器中出现两个函数名称相同，但是形参数量、类型、顺序都不同的函数的情况

由于函数名称相同，编译器将使用另外一套逻辑来判断应该调用哪个函数。此时编译器会根据调用该函数的实际输入心事参数选择应该实现(implement)哪个函数的函数体

这种一个函数多种形式的状态也被称为[[Polymorphism 多态]]，这是一个面向对象编程中的重要概念

重载函数不考虑返回值类型，因此一般不另外定义返回值，如果返回值的类型和以前定义的函数重复了就会报错。下面是一个例子：

```cpp
void test(int a);
void test(double a);

int test(int a); //虽然定义了函数返回值int与void不一样，但是重载函数并不会考虑返回值
//因此在编译器看来，函数int test(int a)和函数void test(int a)完全没有任何区别，因此会报错
```

![[Pasted image 20240506000155.png|center]]
*当调用一个有重载的函数时，编译器会给出该函数的具体声明需要哪些参数*

在下面这个例子中，可以看到两个sphereOverlap函数都在main函数中被同时调用，且编译器可以根据其形参的区别分辨出具体要使用哪个函数体来进行输出

```cpp
#include <iostream>
#include <string>

using namespace std;

bool sphereOverlap(int start, int end, double radius, string actorTag);
void sphereOverlap(string location, double radius, string actorTag);

int main()
{
	sphereOverlap(1, 2, 10.0f, "player");
	sphereOverlap("1", 8.0f, "player");
	return 0;
}

bool sphereOverlap(int start, int end, double radius, string actorTag)
{
	int distance = end - start;

	if (actorTag == "player")
	{
		cout << "Cause Damage!" << endl;
		return true;
	}
	return false;
}

void sphereOverlap(string location, double radius, string actorTag)
{
	double distance = radius + stod(location);//string转为double

	cout << distance << endl;

	if (actorTag == "player" && distance < 10.0f)
	{
		cout << "Cause Damage 2!" << endl;
	}
}
```
