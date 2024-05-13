---
date: 2024-05-13
tags:
  - 计算机/图形学/渲染管线
  - 计算机/图形学/计算几何
toolSet:
  - "[[C 语言基础]]"
  - "[[Introduction to Dlang]]"
knowledgeLink: 
status: false
---
# What is SDL
SDL（英语：Simple DirectMedia Layer）是一套开放源代码的跨平台多媒体开发函式库，使用C语言写成。SDL提供了数种控制图像、声音、输出入的函数，让开发者只要用相同或是相似的代码就可以开发出跨多个平台（Linux、Windows、Mac OS X等）的应用软件。目前SDL多用于开发游戏、模拟器、媒体播放器等多媒体应用领域
## 结构与特色
虽然SDL时常被比较为‘跨平台的DirectX’，然而事实上SDL是定位成以精简的方式来完成基础的功能，它大幅度简化了控制图像、声音、输出入等工作所需撰写的代码。但更高阶的绘图功能或是音效功能则需搭配OpenGL和OpenAL等API来达成。另外它本身也没有方便建立图形用户界面的函数

虽然SDL本身是使用C语言写成，但是它几乎可以被所有的编程语言所使用，例如：C++、Perl、Python（借由pygame函式库）、Pascal等等，甚至是Euphoria、Pliant这类较不流行的编程语言也都可行。

![[Pasted image 20240513022016.png|center|]]

## 语法与子系统
SDL将功能分成下列数个子系统（subsystem）：

- Video（图像）—图像控制以及线程（thread）和事件管理（event）。
- Audio（声音）—声音控制
- Joystick（摇杆）—游戏摇杆控制
- CD-ROM（光盘驱动器）—光盘媒体控制
- Window Management（视窗管理）－与视窗程序设计集成
- Event（事件驱动）－处理事件驱动

以下是一支用C语言写成、非常简单的SDL示例：

```c
// Headers
#include "SDL.h"

// Main function
int main(int argc, char* argv[])
{
    // Initialize SDL
    if(SDL_Init(SDL_INIT_EVERYTHING) == -1)
        return(1);

    // Delay 2 seconds
    SDL_Delay(2000);

    // Quit SDL
    SDL_Quit();

    // Return
    return 0;
}
```

### SDL实现简单的实时循环
以下代码实现了一个窗口，并在`quit`命令发出前保持显示
```c
bool quit=false;
SDL_Event e;
while(quit){
	while(SDL_PollEvent(&e)!=0){
		//Handle keyboard, text input, etc.
	}
	render();
	SDL_RenderPresnet(renderer); //Update the screen
}
```
