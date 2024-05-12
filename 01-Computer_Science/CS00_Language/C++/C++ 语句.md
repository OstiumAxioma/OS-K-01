---
date: 2024-01-17
tags:
  - 计算机/编程语言
  - 计算机/编程语言/Cpp
toolSet: 
knowledgeLink:
  - "[[Iteration and Recursion 迭代与递归]]"
---
## C++语句的基本概念

C++ 语句是控制操作对象的方式和顺序的程序元素

C++ 语句将按顺序执行，除非表达式语句、选择语句、迭代语句或跳转语句特意修改了顺序。

语句可以是以下类型之一：

> _`labeled-statement`_
> _`expression-statement`_
> _`compound-statement`_
> _`selection-statement`_
> _`iteration-statement`_
> _`jump-statement`_
> _`declaration-statement`_
> _`try-throw-catch`_

这些语句代表了以下几种常用类型

- [表达式语句](https://learn.microsoft.com/zh-cn/cpp/cpp/expression-statement?view=msvc-170)。 这些语句评估表达式的副作用或其返回值。
- [Null 语句](https://learn.microsoft.com/zh-cn/cpp/cpp/null-statement?view=msvc-170)。 可以在 C++ 语法需要语句但未采取任何措施的位置提供这些语句。
- [复合语句](https://learn.microsoft.com/zh-cn/cpp/cpp/compound-statements-blocks?view=msvc-170)。 这些语句是用大括号 ({ }) 括起来的语句组。 可在能够使用单个语句的任何位置使用它们。
- [选择语句](https://learn.microsoft.com/zh-cn/cpp/cpp/selection-statements-cpp?view=msvc-170)。 这些语句执行一个测试；如果测试的计算结果为 true（非零），则它们将执行代码的一部分。 如果测试的计算结果为 false，则它们可能执行代码的另一部分。
- [迭代语句](https://learn.microsoft.com/zh-cn/cpp/cpp/iteration-statements-cpp?view=msvc-170)。 这些语句用于重复执行代码块，直到满足指定的终止条件。
- [跳转语句](https://learn.microsoft.com/zh-cn/cpp/cpp/jump-statements-cpp?view=msvc-170)。 这些语句可以立即将控制权转移到函数中的其他位置或从函数中返回控制权。
- [声明语句](https://learn.microsoft.com/zh-cn/cpp/cpp/declarations-and-definitions-cpp?view=msvc-170)。 声明将一个名称引入程序中。

通过在块中的声明变量，您还可以对这些变量的范围和生存期进行精确的控制

### 语句与标签

有三种标记语句。 它们全都使用冒号 (**`:`**) 将某种标签与语句隔开。 **`case`** 和 **`default`** 标签特定于 case 语句

```cpp
#include <iostream>
using namespace std;

void test_label(int x) {

    if (x == 1){
        goto label1;
    }
    goto label2;

label1:
    cout << "in label1" << endl;
    return;

label2:
    cout << "in label2" << endl;
    return;
}

int main() {
    test_label(1);  // in label1
    test_label(2);  // in label2
}
```

## 条件语句

判断结构要求程序员指定一个或多个要评估或测试的条件，以及条件为真时要执行的语句（必需的）和条件为假时要执行的语句（可选的）。

下面是大多数编程语言中典型的判断结构的一般形式：

![[Pasted image 20240505233916.png|center]]

C++ 编程语言提供了以下类型的判断语句

| 语句                                                                      | 描述                                                            |
| ----------------------------------------------------------------------- | ------------------------------------------------------------- |
| [if 语句](https://www.runoob.com/cplusplus/cpp-if.html)                   | 一个 **if 语句** 由一个布尔表达式后跟一个或多个语句组成。                             |
| [if...else 语句](https://www.runoob.com/cplusplus/cpp-if-else.html)       | 一个 **if 语句** 后可跟一个可选的 **else 语句**，else 语句在布尔表达式为假时执行。         |
| [嵌套 if 语句](https://www.runoob.com/cplusplus/cpp-nested-if.html)         | 您可以在一个 **if** 或 **else if** 语句内使用另一个 **if** 或 **else if** 语句。 |
| [switch 语句](https://www.runoob.com/cplusplus/cpp-switch.html)           | 一个 **switch** 语句允许测试一个变量等于多个值时的情况。                            |
| [嵌套 switch 语句](https://www.runoob.com/cplusplus/cpp-nested-switch.html) | 您可以在一个 **switch** 语句内使用另一个 **switch** 语句。                     |

#### if语句

```cpp
if(boolean_expression)
{
   // 如果布尔表达式为真将执行的语句
}
```

如果布尔表达式为 true，则 if 语句内的代码块将被执行。如果布尔表达式为 false，则 if 语句结束后的第一组代码（闭括号后）将被执行。

> [!important] Important
> C 语言把任何非零和非空的值假定为 true，把零或 null 假定为 false。
#### else if语句
如果布尔表达式为 true，则执行 if 块内的代码。如果布尔表达式为 false，则执行 else 块内的代码

![[Pasted image 20240505234117.png|center]]

```cpp
if(boolean_expression)
{
   // 如果布尔表达式为真将执行的语句
}
else
{
   // 如果布尔表达式为假将执行的语句
}
```
#### if...else if...else 语句
一个 **if** 语句后可跟一个可选的 **else if...else** 语句，这可用于测试多种条件。

当使用 if...else if...else 语句时，以下几点需要注意：

- 一个 `if` 后可跟零个或一个 else，else 必须在所有 else if 之后。
- 一个 `if` 后可跟零个或多个 `else if`，`else if` 必须在 `else` 之前。
- 一旦某个 `else if` 匹配成功，其他的 `else if` 或 `else` 将不会被测试。

该语句可以简单理解为以下结构：

```cpp
if (条件1)
{语句1;}
else if (条件2)
{语句2;}
else (条件2为假)
{语句3;}
```

#### 条件语句案例
下面是一个用条件语句做的简单的玩家血量检测结构

```cpp
#include <iostream>
#include <string>

int main()
{
	using namespace std;

	float health = 100.0f;
	float damage = 20.0f;

	health = health - damage;

	if(health <= 0.0f)
	{
		cout << "You died!" << endl;
	}
	else if(health < 30.0f)
	{
		cout << "You're pretty hurt!" << endl;
	}
	else
	{
		cout << "Your HP is:" << health << endl;
	}

	return 0;
}
```

### 循环语句
有的时候，可能需要多次执行同一块代码。

一般情况下，循环语句是顺序执行的：函数中的第一个语句先执行，接着是第二个语句，依此类推。

![[Pasted image 20240505234247.png|center]]
#### 循环类型
C++ 编程语言提供了以下几种循环类型：

|循环类型|描述|
|---|---|
|[while 循环](https://www.runoob.com/cplusplus/cpp-while-loop.html)|当给定条件为真时，重复语句或语句组。它会在执行循环主体之前测试条件。|
|[for 循环](https://www.runoob.com/cplusplus/cpp-for-loop.html)|多次执行一个语句序列，简化管理循环变量的代码。|
|[do...while 循环](https://www.runoob.com/cplusplus/cpp-do-while-loop.html)|除了它是在循环主体结尾测试条件外，其他与 while 语句类似。|
|[嵌套循环](https://www.runoob.com/cplusplus/cpp-nested-loops.html)|您可以在 `while`、`for` 或 `do...while` 循环内使用一个或多个循环。|
#### while循环语句
while循环语句的格式如下：

```cpp
while(条件)
{语句;}
```

> [!tip] 判断条件
> 执行大括号中的语句前，检查一次条件是否满足

如果条件满足则继续执行，否则结束执行语句

下面是一个通过while语句实现的简单的密码判别功能案例

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
	int password = 1234;
	int input = 0;

	while (input != password)
	{
		cout << "Enter the correct password: " << endl;
		cin >> input;
	}

}
```
#### do…while循环语句
do…while循环语句的格式如下：

```cpp
do{语句;}
while(条件);
```

执行大括号中的语句后，检查一次条件是否满足

> [!tip] 判断条件
> 区分do…while循环与其它循环的关键：无论while里的条件是否满足，都会至少执行一遍do里的语句，执行后才检查条件是否满足

如果条件满足则继续执行，否则结束执行语句

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
	int password = 1234;
	int input = 0;

	do

	{
		cout << "Enter the correct password: " << endl;
		cin >> input;
	}

	while (input != password);

}
```
#### for循环语句

```cpp
for(初值;条件;每次变化量)
{语句;}
```

> [!important] 
> 初值在执行语句前就要完成赋值

> [!tip] 判断条件
> 在for语句中，检查的条件包括**初值+每次变化量的结果**。且执行前就会检查条件是否满足

如果条件满足则继续执行，否则结束执行语句

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
	for (int i = 0; i <= 3; i = i + 1) 
	{
		//会循环执行4次
		cout << "Fire" << endl;
	}

	return 0;
}
```

#### 循环控制语句
循环控制语句能更改执行的正常序列。当执行离开一个范围时，所有在该范围中创建的自动对象都会被销毁。

C++ 提供了下列的控制语句：

| 循环类型                                                                        | 描述                                                    |
| --------------------------------------------------------------------------- | ----------------------------------------------------- |
| [break 语句](https://www.runoob.com/cplusplus/cpp-break-statement.html)       | 终止 loop 或 switch 语句，程序流将继续执行紧接着 loop 或 switch 的下一条语句。 |
| [continue 语句](https://www.runoob.com/cplusplus/cpp-continue-statement.html) | 引起循环跳过主体的剩余部分，立即重新开始测试条件。                             |
| [goto 语句](https://www.runoob.com/cplusplus/cpp-goto-statement.html)         | 将控制转移到被标记的语句。但是不建议在程序中使用 goto 语句。                     |
