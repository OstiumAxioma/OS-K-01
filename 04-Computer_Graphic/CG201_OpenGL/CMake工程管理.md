---
date: 2024-02-18
tags:
  - 计算机/图形学/渲染管线
toolSet:
  - "[[OpenGL渲染管线]]"
  - "[[现代C++基础]]"
knowledgeLink: 
status: true
---
## CMake工程前置文件
### 静态链接vs动态链接

**静态链接库（.lib）**：包含代码与数据的文件，可在程序**编译期间**被链接入程序
稳定、固定版本
![[lib.png|center|300]]
**动态链接库（.dll）**：包含代码与数据的文件，可在程序**执行期间**被动态加载
利于程序的升级
![[dll.png|center|300]]
### 配置流程

配置流程概述
1. 提前下载好visual studio 2022和Cmake。
2. 下载glfw的**source**文件，用Cmake编译该文件，这一步会生成`glfw.lib`文件。
3. 来到glad网站进行在线下载，输入配置，下载glad的<mark class="hltr-red">zip文件</mark>。
4. 在visual studio**创建空白Cmake项目**，在项目里创建三个文件夹：src、lib、include。将`glad.c`复制进src文件夹，将`glfw.lib`复制进lib文件夹，将glad和glfw中的头文件复制进include文件。
5. 配置CMakelists文件，添加测试代码，然后为CMakelists重新生成缓存，编译调试

## CMake工程配置流程

### 文件结构和`CMakeLists.txt`
1. `CMakeLists.txt` 整个CMake工程的描述文件
		
```Cpp
//需求的最低cmake程序版本
cmake_minimum_required(VERSION 3.12)

//本工程的名字
project(OpenGL)

//本工程支持的C++版本
set(CMAKE_CXX_STANDARD 17)

//本工程主程序文件及输出程序名称
add_executable(glStudy "main.cpp")
```

2. Visual Studio → 文件 → 打开 → CMake → `CMakeLists.txt`
3. 创建完成的文件结构如下：
	1. `CMakeLists.txt`整个CMake工程的描述文件
	2. `main.cpp`程序主文件
	3. `.vs`文件夹：visual studio建立的工程文件夹<font color="#c0504d">（工程分享不需要包括此文件夹）</font>
	4. `out`文件夹：工程编译链接的结果输出文件夹 <font color="#c0504d">（工程分享不需要包括此文件夹）</font>
		1. `-out/build/x64-debug`为最终可执行程序.exe的位置

### 多C++文件编译

> [!question]
> 需求：当我们在工程里加入了新的cpp文件，主函数需要调用其中函数的时候，需要将其纳入编译与链接

```Cpp
//需求的最低cmake程序版本
cmake minimum required(VERSION 3.12)
//本工程的名字
project(OpenGL)
//本工程支持的C++版本
set(CMAKE CXX STANDARD 17)
//搜索当前目录所有的cpp，加入SRCS(sources)变量中
aux_source_directory(.SRCS)
//本工程所有cpp文件编译链接，生成exe($ 释放文件)
add_executable(glStudy ${SRCS})
```

### 多文件夹编译

> [!question]
> 需求：当代码分布在不同的文件夹下的时候，需要将其他文件夹下的cpp打包为lib库静态链接，从而纳入链接范畴

```Cpp
//遍历并递归将本文件夹下所有cpp放到FUNCS中
file(GLOB_RECURSE FUNCS ./ *.cpp)

//将FUNCS中所有cpp编译为funcs这个lib库
add_library(funcs ${FUNCS})
```
<sup>此段代码写入<font color="#9bbb59">库</font>CMakeLists</sup>
![[FileStructure.png|center|200]]<sup><center>主CMakeLists和库CMakeLists的关系，库在此处是funcs文件夹</center></sup>
```Cpp
//需求的最低cmake程序版本
cmake_minimum_required(VERSION 3.12)

//本工程的名字
project(OpenGL)

//本工程支持的C++版本
set(CMAKE_CXX_STANDARD 17)

//将funcs文件夹纳入到编译系统
add_subdirectory(funcs)

//搜索所有的cpp,加入SRCS变量中
aux_source_directory(. SRCS)

//本工程所有cpp文件编译链接,生成exe
add_executable(glStudy ${SRCS})

//将funcs.lib链接入softRender
target_link_libraries(glStudy funcs)

```
<sup>写入主CMakeLists</sup>
### 资源拷贝

> [!question] 
> 需求：当我们工程中出现资源文件(图片、模型、音频、视频、动态链接库等),都需要拷贝到编译链接完成的exe所在目录下面,才能够被程序正确读取,所以需要拷贝功能

```Cpp
//把需要拷贝的资源路径都放到ASSETS里
file(GLOB ASSETS "./assets" "thirdParty/assimp-vc143-mtd.dll")

//把ASSETS指代的目录集合的内容,都拷贝到可执行文件目录下
file(COPY ${ASSETS} DESTINATION ${CMAKE_BINARY_DIR})
```
<sup>写入主CMakeLists</sup>

此过程在文件执行过程中[[CMake工程管理#静态链接vs动态链接|动态链接库]]链接库]].dll

![[SourceCopy.png|center|300]]
<center><sup>动态链接库的存放位置</sup></center>
> [!tip]
> 主CMakeList.txt中保存文件即可完成拷贝，此后每次更新assets都需要执行保存来更新
### 多目标编译

> [!tip]
> 需求：当我们在一个工程里,有多个main程序的时候,希望可以每次选择一个执行

![[MultiTarget.png|center|300]]
<center><sup>多个main文件或主程序文件</sup></center>
```Cpp
//搜索所有的cpp,加入SRCS变量中
aux_source_directory(. SRCS)

//本工程主程序文件及输出程序名称
add_executable(glStudy "main.cpp")
add_executable(main2 "main2.cpp")

//将funcs.lib链接入softRender
target_link_libraries(main funcs)
```
