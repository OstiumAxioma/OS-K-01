---
date: 2024-05-13
tags:
  - 计算机/编程语言/Dlang
toolSet:
  - "[[C 语言基础]]"
knowledgeLink: 
status: true
---
# What is D
D语言是一种编程语言，具备多范型，例如面向对象、指令式。由沃尔特·布莱特和安德烈·亚历山德雷斯库所开发，起源自C++，深受C++的影响，然而其不是C++的变种，而是重新设计来自C++的部分特性，并受到其它编程语言观念的影响，如Java、C#以及Eiffel。2007年1月2日发布1.0稳定版本。2007年1月17日发布2.0版本。

D is a general-purpose systems programming language with a C-like syntax that compiles to native code. It is statically typed and supports both automatic (garbage collected) and manual memory management. D programs are structured as modules that can be compiled separately and linked with external libraries to create native libraries or executables.

D的设计来自实际的C++用法的经验教训，而不是从理论的角度。D沿用了很多C/C++观念，同时摒弃了一些概念，因此D并不完全兼容C/C++代码。D实现了C++的功能，实现了契约式设计（design by contract）、单元测试、真正的模块性、自动化存储器管理（垃圾回收）、头等数组（first class array）、关联数组、动态数组、数组切片、嵌套函数（嵌套函数）、内部类别、闭包的限制形式、匿名函数、编译时期函数执行、惰性计算以及革新的模板语法。D保有C++的性能以进行低端程序设计，并加入完整的内联汇编器支持。C++的多重继承改以Java 单继承与接口混合的风格取代。D的声明、语句和表达式语法几乎和C++一样。

> [!NOTE]
> 内联汇编器（inline assembler）象征了D和Java、C#等应用程序语言的不同。内联汇编器让程序员输入机器特定的汇编语言码，如同标准D代码—通常由系统程序员使用的技术，以访问处理器的低端功能，直接以硬件下的界面执行程序，如操作系统以及驱动程序。

D is not source compatible with C and C++ source code in general. However, any code that is legal in both C and D should behave in the same way.

Like C++, D has closures, anonymous functions, compile-time function execution, ranges, built-in container iteration concepts, and type inference. Unlike C++, D also implements design by contract, modules, garbage collection, first class arrays, array slicing, nested functions and lazy evaluation. D uses Java-style single inheritance with interfaces and mixins rather than C++-style multiple inheritance. On the other hand, D's declaration, statement and expression syntax closely matches that of C++.

D is a systems programming language. Like C++, D supports low-level programming including inline assembler, which typifies the differences between D and application languages like Java and C#. Inline assembler lets programmers enter machine-specific assembly code within standard D code, a method used by system programmers to access the low-level features of the processor needed to run programs that interface directly with the underlying hardware, such as operating systems and device drivers, as well as writing high-performance code (i.e. using vector extensions, SIMD) that is hard to generate by the compiler automatically.

D supports function overloading and operator overloading. Symbols (functions, variables, classes) can be declared in any order - forward declarations are not required.

In D, text character strings are arrays of characters, and arrays in D are bounds-checked.[12] D has first class types for complex and imaginary numbers.

## D Programming paradigms
D supports five main programming paradigms:

- concurrent (actor model) 并发（演员模型）
- object-oriented 面向对象
- imperative 指令式
- functional 函数式
- metaprogramming 元编程

## D Recycle
D同时支持手动与自动的垃圾回收机制。其自动回收是通过存储器管理完成的

存储器通常以垃圾回收管理，不过当这些对象超出作用域时，可立即结束指定的对象。还是可以使用重载运算符`new`和`delete`，以及简单的直接调用C的`malloc`函数和`free`函数以进行显示的存储器管理。垃圾回收可禁用个别的对象或事件，以健全整个程序，如果在存储器管理上有更多的控制，则更为理想。当垃圾回收在程序中有所不足时，手册还提供许多如何实现不同的高度优化存储器管理方案的示例。

## D and C family
D支持C的应用程序二进制接口（ABI），以及C的基本和衍伸类型，就能直接访问现有的C代码以及程序库。C的标准函式库也是D标准的一部分。除非你使用非常清楚的命名空间，它可以稍微散乱的访问，因为它散布遍及于D模块—不过纯粹的D标准函式库也通常够用，除非要与C代码接合。

并未完整支持C++的ABI，尽管D可以访问写给C ABI的C++代码，且可访问C++COM（组件对象模型）代码。D语法分析器了解外部（C++）调用约定，以链接C++对象，不过它只实现在D 2.0。

# Using D
## D Compiler
D has three major compiler: 
- <mark class="hltr-orange">DMD</mark>
	- DMD is an open-source compiler
	- Uses D's module system for fast compiling
- GDC
	- GCC based D compiler frontend
	- Good GDB support
- <mark class="hltr-pink">LDC</mark> - LLVM based D Compiler
	- Allows you to get LLVM optimizations and target many architectures

