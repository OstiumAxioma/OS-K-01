---
date: 2024-05-17
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet: []
knowledgeLink: 
status:
---
# Unity UI 技术概述
## Unity GUI 简介
UI 即 User Interface（用户界面）的简称。在许多软件中，采用狭义的概念，特指窗体、面板、按钮、文本框等人们熟悉的人机交互元素，及其组织与风格（也称皮肤）。Unity UI 系统采用上述狭义概念。

Unity 目前支持三套完全不同风格的 UI 系统：

- 4.0 及以前 - IMGUI（Immediate Mode GUI）及时模式图形界面。它是代码驱动的 UI 系统，没有图形化设计界面，只能在 OnGUI 阶段用 GUI 系列的类绘制各种 UI 元素，因此 UI元素只能浮在游戏界面之上。
- 5.0 及以后 - Unity GUI / UGUI 是面向对象的 UI 系统。所有 UI 元素都是游戏对象，友好的图形化设计界面， 可在场景渲染阶段渲染这些 UI 元素。
- 2018.3 及以后 - UXML（unity 扩展标记语言）。基于 IMGUI 的 AXML 包装，方便移动应用开发者入门。
## IMGUI系统
IMGUI 的存在符合游戏编程的传统，即使在今天它依然没有被官方宣判为遗留（将要淘汰的）系统（Legacy Systems）。在修改模型，渲染模型这样的经典游戏循环编程模式中，在渲染阶段之后，绘制 UI 界面无可挑剔（参考Execution Order of Event Functions）。这样的编程即避免了 UI 元素保持在屏幕最前端，又有最佳的执行效率，一切控制掌握在程序员手中，这对早期计算和存储资源贫乏的游戏设备来说，更是弥足珍贵。当然，早年 UI 交互手段就是绘制图片和文本，检测输入事件等基本任务。

> [!abstract]
> 按 Unity 官方说法，IMGUI 主要用于以下场景：
> 
> - 在游戏中创建调试显示工具
> - 为脚本组件创建自定义的 Inspector 面板。
> - 创建新的编辑器窗口和工具来扩展 Unity 环境。

> [!tip]
> 也就是说，IMGUI在Unity开发原型阶段的快速反应与调试中起到非常重要的作用，因为它的代码相对简单容易发现错误。

IMGUI系统通常不打算用于玩家可能使用并与之交互的普通游戏内用户界面。为此，应该使用 Unity 的基于 GameObject 的 UGUI 系统。

显然，如果不做复杂的界面，如下代码简单易用的代码是程序员喜欢的：

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class IMGUITest : MonoBehaviour {

	void OnGUI (){
		// Make a background box
		GUI.Box(new Rect(10,10,100,90), "Loader Menu");

		// Make the first button. If it is pressed, Application.Loadlevel (1) will be executed
		if(GUI.Button(new Rect(20,40,80,20), "Level 1")) {
			Application.LoadLevel(1);
		}

		// Make the second button.
		if(GUI.Button(new Rect(20,70,80,20), "Level 2")) {
			Application.LoadLevel(2);
		}
	}
}
```
## UGUI 的产生与优势
随着游戏开发的普及，为了让设计师也能参与参与程序开发，从简单的地图编辑器、菜单编辑器等工具应运而生。设计师甚至不需要程序员帮助，使用这些工具就可直接创造游戏元素，乃至产生游戏程序。除此之外，现代游戏 UI 必须要满足以下要求：

- 跨设备执行，自动适应不同分辨率
- UI 元素与游戏场景融为一体的交互
- 支持复杂的布局
- 多摄像机支持

以至于即使优秀的程序员在现代 UI 面前，传统代码驱动的 UI 面临开发效率低下，难以调试等问题。 对于 Unity 平台， 当 NGUI: Next-Gen UI kit 的出现使得不依赖 Unity Pro 功能，使用所见即所得（WYSIWYG）设计工具，集成了 tweening 运动管理系统， 可以制作多数 2D 游戏，直接威胁了 Unity 的生意。 Unity 最终高薪聘了 NGUI 的主设计师，最终又分道扬镳，于是就有了 UGUI，青出于蓝而胜于蓝！

> [!abstract]
> UGUI 的优势：
> 
> - 所见即所得（WYSIWYG）设计工具
> - 支持多模式、多摄像机渲染
> - 面向对象的编程

# UGUI 基础
## Canvas画布
画布（Canvas）是UGUI所有组件的容器，任何场景中想要使用UGUI组件就必须先创建画布。画布是绘图区域, 同时是 ui 元素的容器。 容器中 ui 元素及其子 UI 元素都将绘制在其上。 拥有Canvas组件的游戏对象都有一个画布，它空间中的子对象，如果是 UI 元素将渲染在画布上。

画布区域在场景视图中显示为矩形。这使得定位UI元素变得非常容易，无需随时显示游戏运行视图。

1. UI元素的显示顺序
	画布中的UI元素按照它们在层次结构中出现的顺序绘制。如果两个UI元素重叠，后面的元素将出现在较早的元素之上。因此，最后一个孩子显示在最上面。
	
	要更改哪些元素出现在其他元素的顶部，通过拖动它们在 Hierarchy 视图中的位置。这顺序也可以通过 Transform 组件的方法在脚本中控制。 如：SetAsFirstSibling，SetAsLastSibling 和SetSiblingIndex。

2. 渲染模式
	画布组件有渲染模式（Render Mode）设置，可用于使其在屏幕空间（Screen Space）或世界空间（World Space）中渲染。
	
	屏幕空间 - 叠加（Screen Space - Overlay）
	
	将UI元素放置在场景顶部渲染的屏幕，画布会自动更改大小匹配屏幕。<mark class="hltr-red">Canvas 默认中心点为屏幕中心</mark>！

	1. **屏幕空间 - 相机（Screen Space - Camera）**
		画布放在制定的渲染摄像机前，如 100 的位置，画布会自动匹配为屏幕分辨率。Canvas 默认中心点为屏幕中心！ 
	
	2. **世界空间（World Space）**
		画布行为与场景中的其他任何对象一样，UI元素将放置在其他对象的前面或后面渲染。**画布大小和位置任意设置**，这对于意在成为世界一部分的用户界面非常有用。
## UI 布局基础
每个UI元素都被表示为一个矩形，为了相对于 Canvas 和 其他 UI 元素实现定位，Unity 在 Transform 基础上定义了 RectTransform （矩形变换） 支持矩形元素在 2/3D 场景中变换。
### 矩形变换

矩形变换像常规变换一样具有位置，旋转和比例，但它也具有宽度和高度表示矩形的尺寸。
![[Pasted image 20240517191103.png|center]]

**Pivot/旋转点/轴心**
旋转点在场景视图中显示为蓝色圆圈。用规范化坐标表示位置，如 （0.5,0.5）表示矩形的中心。
修改 Rotation 属性，矩形围绕此点旋转。
![[Pasted image 20240517191145.png|center]]
**Anchors/锚点**
锚点在场景视图中显示为四个小三角形手柄（四叶花）。每个叶子位置对应矩形的四个顶点。当描点随父对象变换时，矩形的顶点与对应的锚点相对位置必须保持不变。
	
### 矩形工具

Unity 在界面上提供了工具以方便修改 UI 元素的变换参数。
![[Pasted image 20240517191412.png|center]]
提供了移动、旋转、比例、矩形四个工具。该图选择了矩形工具的工具栏按钮。在选择矩形工具时，设置为“旋转”和“局部”视图，通常便于操作
![[Pasted image 20240517194440.png|center]]
### 锚点预设

在 Inspector 面板中，可以在 Rect Transform 组件的左上角找到 Anchor Preset 按钮。点击该按钮会弹出 Anchor Presets 下拉菜单。
![[Pasted image 20240517194510.png|center]]
## UI 组件与元素
UI 部件都是用 Script 开发的自定义组件。包括在 UI、Layout 和 Rendering 等分类中。
### 可视化组件

可视化组件，在组件 UI 分类中，包括：

- Text：显示的文本的文本区域。可以设置字体，字体样式，字体大小以及文本是否具有丰富的文本功能。
- Image：显示图片的区域。可以设置GUI精灵、色彩。
- Raw Image：原始图像采用纹理，进行 UV 矩形贴图。
- Mask：Mask不是一个可见的UI控件。遮罩将子元素限制（即“掩蔽”）为父元素的形状。如果孩子比父控件大，那么只有适合父节点遮罩的部分是可见的。
- Effects：应用各种简单的效果，例如简单的投影或轮廓。
### UI 交互元素

UI 交互元素是 GameObject，它拥有 UI 交互组件、UI可视化组件、及相关组件的组合，以及一些 UI 子元素构成，以方便用户在设计场景中创建交互界面。

例如：Button 元素，除了 UI 必须拥有的 RectTransform 和 Canvas renderer 外，还有 Image 和 Button 组件，以及一个 Text 子元素。

- Button
- Toggle
- Toggle Group
- Slider
- Scrollbar
- Dropdown
- Input Field
- Scroll Rect (Scroll View)

# EventSystem
按官方手册，EventSystem 在一个场景中 **有且仅能有一个**，它将游戏操纵杆、键盘、鼠标、触摸屏、触摸屏与 UI 对象交互事件，如 Click，MouseMOver，DragEnter 等等，传递给游戏对象对应的行为组件。

> [!tip]
> 由于UI常常兼顾管理游戏机制的任务，所以UI和EventSystem通常高度绑定。同时，需要整个游戏共享的全局变量也通常在EventSystem中声明。

## Static Variables 静态变量

> [!important]
> 如果想要声明全局可用的变量，在Unity Script中我们通常使用静态变量修饰符

C#静态变量使用static 修饰符进行声明，在类被实例化时创建，通过类进行访问不带有 static 修饰符声明的变量称做非静态变量，在对象被实例化时创建，通过对象进行访问一个类的所有实例的同一C#静态变量都是同一个值，同一个类的不同实例的同一非静态变量可以是不同的值。静态函数的实现里不能使用非静态成员，如非静态变量、非静态函数等。 使用 static 修饰符声明属于类型本身而不是属于特定对象的静态成员static修饰符可用于类、字段、方法、属性、运算符、事件和构造函数，但不能用于索引器、析构函数或类以外的类型

> [!Definition]
> 在全局变量前，加上关键字 static 该变量就被定义成为了一个静态全局变量。
> 也就是使用`public static` 

使用全局静态变量，可以让其他类访问该值时，避免实例化。

## Static Function 静态方法
相对应的，方法一样有静态的修饰符。静态方法是一种特殊的**成员方法**，它不属于类的某一个具体的实例，而是属于类本身。所以对静态方法不需要首先创建一个类的实例，而是采用`类名.静态方法`的格式 。

由于static方法是类中的一个成员方法,属于整个类,即不用创建任何对象也可以直接调用。因此他也有避免实例化，不需要创建任何对象也可以调用的优点。但相应的，因为不需要实例化，我们也无法自动销毁某个静态方法。

Unity中有许多共有库的方法都是静态方法，比如`Input.GetAxis()(), Input.GetKey ()(), Input.GetButton ()`等

# SceneManager 场景管理器
有事件管理器，同样的就有场景管理器。SceneManager是在UnityEngine.SceneManagement命名空间下的类，主要有建立场景，加载场景，即运行时的场景管理。Unity 使用过程中关卡加载和卸载是大多数三维引擎都要提供的基本功能。因为关卡切换在游戏中非常常用。自从Unity5.3版本，Unity 的关卡切换就添加了新的SceneManager的类来处理。

<mark class="hltr-red">SceneManager</mark>
Class in `UnityEngine.SceneManagement`
描述：运行时的场景管理。
静态变量sceneCount: 当前加载的场景的总数。
前加载的场景的数量将被返回。

要使用SceneManager的話，最上方要先引入`UnityEngine.SceneManagement`名稱空間

![[Pasted image 20240517204024.png|center]]

只有在项目编译设置中添加了的场景才会被场景管理器读取到，只存放在本地文件夹的场景是不能直接读取的

![[Pasted image 20240517204145.png|center]]

讀取的方法有兩個，一個是使用場景名稱，另一個是使用場景的index。使用名称的话**如果修改了場景名稱，方法內的名稱也需要修改**。

而使用场景Index的话，是根据Building Setting中的Index来决定的。