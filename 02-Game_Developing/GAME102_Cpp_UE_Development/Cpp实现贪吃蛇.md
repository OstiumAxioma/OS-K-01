---
Date: 2024-05-05
tags:
  - 游戏开发
  - 游戏开发/引擎
ToolSet:
  - "[[现代C++基础]]"
  - "[[C++ 结构体]]"
KnowledgeLink:
  - "[[Class and Object 面向对象基本元素-类与对象]]"
---
### 游戏开发的基本原则

1. 需要解决什么问题
2. 当前条件下，有哪些可行的方案
3. 针对方案需要动用哪些工具

### 确定游戏规则

1. 玩家属性：人数，关系，行为模式etc.
2. 游戏类型：视角，游戏方式，游戏目标etc.
3. 平台：硬件，机体性能，交互模式etc.

比如一个贪吃蛇游戏，按照这个逻辑建立原型：

![[Pasted image 20240506005815.png|center]]

1. 玩家属性
    1. 单人游玩
    2. 简单上手
2. 游戏类型
    1. 2D街机
    2. 金币收集+线性成长
    3. 存活+积分的游戏目标
    4. 游戏时长无限
3. 平台
    1. PC+数位显示屏
    2. 键盘操控WASD移动模式

确立游戏规则后，以玩家的视角线性的提出问题并回答：

1. 游戏是怎么开始的（游戏的初始化）
    1. 游戏区域的绘制
    2. 得分版
    3. 玩家和食物的出生点 //利用cout命令搭配字符块来绘制
2. 游戏是怎么检查玩家行为和给予反馈的
    1. 检测玩家和食物以及边界的碰撞关系 //用循环语句反复检查和重绘角色位置
    2. 确定游戏的结束条件
3. 玩家的操控手段
    1. 键盘控制器 //调用Windows内置键盘指令

### 搭建游戏框架

```cpp
#include <iostream>
using namespace std;

bool gameOver = false; //初始化时游戏不结束
void Initialization() //定义一个初始化用的函数
{
	//游戏初始化
}

void GameMode()
{
	//游戏规则
}

void PlayerController()
{
	//玩家控制器
}

void Tick()
{
	//模拟帧刷新，本案例不传递Delta time
}

void UserInterface()
{
	//用户界面，积分面板等
}

int main()
{
	//初始化游戏
	Initialization();

	//刷新游戏数据
	while (!gameOver)
	{
		//游戏未结束始终循环以下内容
		GameMode();
		PlayerController();
		Tick();
		UserInterface();
	}

	return 0
}
```

根据事前规划的游戏逻辑，用自定义函数搭建代码的基本架构

针对每一个游戏的模块，包括：

1. 游戏初始化
2. 游戏模式/规则
3. 控制系统
4. 计时系统
5. 用户界面

这几个点来创建函数块，并统一在`main`函数中调用

由于游戏的整个过程中都要不断检查除初始化以外的所有块，将初初始化以外的函数都放入`while`循环语句中

记住`#include <iostream>`和`using namespace std;`都是建立程序前基本必须的

### 代码的整理

使用一些编译器的指令可以帮助代码变得更加易读

比如`#pragma region`命令可以手动创建可收缩的代码块

```cpp
#pragma region 类型/函数
bool gameOver = false; //初始化时游戏不结束
#pragma	endregion
```

### 建立游戏环境

利用for循环语句建立游戏边框，分为三个部分：

1. 顶部完整行
2. 中间空格
    1. 左右两侧填充*号
    2. 中间填充空格
3. 底部完整行

```cpp
#pragma region VARs
bool gameOver = false; 
//长宽参数
const int HEIGHT = 20;
const int WIDTH = 40;

#pragma	endregion

void Initialization() 
{
	//顶部完整行
	for (int i = 0; i < WIDTH; i++)
	{
		cout << "*"; 
	}
	cout << endl;
	//中间空白处
	for (int i = 0; i < HEIGHT; i++)
	{
		
		for (int j = 0; j < WIDTH; j++) 
		{
			if (j == 0 or j == WIDTH - 1)
			{
				//两侧墙壁，也就是j=0和j=-1的位置
				cout << "*"; 
			}
			else
			{
				//中间除了墙壁以外的所有位置
				cout << " "; 
			}
		}
		cout << endl; 
	}

	//底部完整行
	for (int i = 0; i < WIDTH; i++)
	{
		cout << "*";
	}
	cout << endl;
}
```

### 定义玩家结构

调用Windows内置库获取屏幕位置信息

设置结构体来定义玩家属性

在initialization框架下设置结构体的初值

```cpp
#include <windows.h> //调用windows内置库

struct SnakeInfo
{
	int length;
	int x;
	int y;
	char head;
	char movingDirection;
	int score;
};

SnakeInfo snake; //实例化结构体SnakeInfo，使用snake来调用

//在Tick块中不断更新snake的x和y坐标
void Tick()
{
	Sleep(100); //休眠100ms

	SetSnakeLocation(snake.x, snake.y);
	cout << snake.head; //挪动光标位置到新坐标，还没有清除尾部坐标
}

```

### 建立玩家控制器

```cpp
#include<conio.h>//调用键盘输入

char lastTouch = 'w'; //定义一个移动参数并设置初值

void PlayerController()
{
	//玩家控制器
	// keyboard hit键盘按键事件
	if (_kbhit())
	{
		//定义局部变量touch，字符类型(因为键盘输入的就是字符类型变量)，负责接收键盘输入响应
		char touch = _getch();

		//通过读取键盘输入并赋值给snake.movingDirection这个结构体属性来定义角色移动（需要搭配后续改变蛇坐标的代码块）
		//分情况讨论，角色向上移动
		if (touch == 'w' and snake.movingDirection != 's')
		{
			snake.movingDirection = 'w';
			lastTouch = 'w';
		}
		//分情况讨论，角色向下移动
		else if (touch == 's' and snake.movingDirection != 'w')
		{
			snake.movingDirection = 's';
			lastTouch = 's';
		}
		//分情况讨论，角色向左移动
		else if (touch == 'a' and snake.movingDirection != 'd')
		{
			snake.movingDirection = 'a';
			lastTouch = 'a';
		}
		//分情况讨论，角色向右移动
		else if (touch == 'd' and snake.movingDirection != 'a')
		{
			snake.movingDirection = 'd';
			lastTouch = 'd';
		}
		//其他情况保持原先的方向（读取原输入值）
		else
		{
			snake.movingDirection = lastTouch;
		}
	}

	//定义蛇本身如何移动
	//向上
	if (snake.movingDirection == 'w') snake.y--;
	//向下
	else if (snake.movingDirection == 's') snake.y++;
	//向左
	else if (snake.movingDirection == 'a') snake.x--;
	//向右
	else if (snake.movingDirection == 'd') snake.x++;
	else return;
	
}
```

### 定义游戏结束条件

```cpp
void GameMode()
{
	//游戏结束条件：触碰边界
	//通过判断蛇头部的位置来决定是否触碰边界
	if ((snake.x == 0 or snake.x == WIDTH - 1) or
		(snake.y == 0 or snake.y == HEIGHT + 1));
	{
		//全局变量gameover布尔值为正来结束游戏
		gameOver = true;
		//将光标设置在右下角
		SetSnakeLocation(30, 22);
		cout << "Game Over" << endl;
	}
}
```

### 消除贪吃蛇尾部

由于贪吃蛇的身体总长度会随着食物发生变化，不能光消除固定位置的身体char数值，而是要利用动态数组来定位需要消除的char，且该数组需要可变

动态数组可以逐帧交换数据

而此处，可以使用向量（vector）来达到一石二鸟的效果

```cpp

#include<vector>

#pragma region VARs
bool gameOver = false; 
const int HEIGHT = 20;
const int WIDTH = 40;
char lastTouch = 'w';
int prevBodyX = 0; //身体上一帧的信息
int prevBodyY = 0;
int prevHeadX = 0; //头部上一帧的信息
int prevHeadY = 0;
vector<int> tempX(1);//定义一组一维数组，至少初始化一个数值，这里是为了维持每帧头部位置的变换（一帧移动一个像素所以头部也要每帧前移一个像素）
vector<int> tempY(1);

SnakeInfo snake; //实例化结构体SnakeInfo，使用snake来调用

#pragma	endregion

void Tick()
{
	Sleep(100); //休眠100ms

	//由于蛇尾位置一直在变，需要按照当前蛇的长度循环搜索蛇全身的位置信息直到最后一位（也就是后方为空格的一位）
	for (int i = 0; i < snake.length; i++)
	{
		//遍历蛇的整个身体找到尾
		prevBodyX = tempX[i];
		prevBodyY = tempY[i];

		tempX[i] = prevHeadX;
		tempY[i] = prevHeadY;
		//值交换，把欲清楚的prevBody位置信息传回prevHead
		**//假如跳出for循环，prevHead的值会被后面的逻辑重新赋值prevHeadX=tempX[0]
		//假如继续for循环，执行下面的逻辑把prevHead的信息继续交换给后面的身体部分**
		//判断是否跳出for循环的标准是snake.length也就是说直到遍历整个蛇的长度以前都不会跳出循环
		prevHeadX = prevBodyX;
		prevHeadY = prevBodyY;
	}

	//清除上一帧的符号，注意在刷新这一帧之前进行
	//找到且仅清楚蛇尾最后一位
	SetSnakeLocation(prevBodyX, prevBodyY);
	cout << " "; //刷新为空格

	SetSnakeLocation(snake.x, snake.y);
	cout << snake.head; //挪动光标位置到新坐标，还没有清除尾部坐标
	//值交换，刷新后立刻把头部位置存入prevHead变量
	prevHeadX = tempX[0];
	prevHeadY = tempY[0];
	//把结构体数值存入变量
	tempX[0] = snake.x;
	tempY[0] = snake.y;
}
```

### 生成食物增加蛇长度

利用上一部分引入的矢量数据，我们可以得到数组。通过`push_back`指令就能在数组的末端添加元素

```cpp
void SpawnFood(FoodInfo& food) //&代表引用传递而非值传递
{
	srand(static_cast<unsigned int>(time(0))); //影响rand()值

	//随机数结果除以边长-2取余数获取范围内随机值
	food.foodLocationX = rand() % (WIDTH - 2) + 1;
	food.foodLocationY = rand() % (HEIGHT - 2) + 1;
	//在屏幕上显示这两个数值
	SetObjectLocation(food.foodLocationX, food.foodLocationY);
	cout << food.foodSymbol;
}

void GameMode()
{	
	if ((snake.x == 0 or snake.x == WIDTH - 1) or (snake.y == 0 or snake.y == HEIGHT + 1))
	{
		gameOver = true;

		SetObjectLocation(30, 22);
		cout << "游戏结束";
	}
	else if (snake.x == foodActual.foodLocationX and snake.y == foodActual.foodLocationY)
	{
		snake.length += 1;
		snake.score += 10;
		SpawnFood(foodActual);
		tempX.push_back(int());//push_back从末位添加元素
		tempY.push_back(int());
	}
}
```