#SOLID, GRASP和其他面向对象设计的基本原则
##学习面向对象的设计原则，并牢牢掌握SOLID和GRASP规则背后的思想

（by Muhammad Umair  ·  Feb. 13, 17 · Web Dev Zone）

今天开始编程，体验与Qlik合作带来的驱动数据应用开发的强大引擎。

从一些老生常谈的事情开始说起来吧，软件代码应该符合以下特质：

- 可维护性
- 可扩展性
- 模块化

每当你想要质问代码是否满足以上标准的时候，就会将自己置于困难之中。


If the software code remains easier to maintain, extend, and modularize over its lifetime then it means that the code has above average quality features.

如何一个软件代码维护起来更简单，可拓展，并且模块化在他的生命周期内，那我们认为这个代码是具有更高的平均质量特征。

I have written difficult to read, hard to extend, and rotten software code. I only knew after six months into the development when a change happened. Hence the development timeline is important in understanding quality factors.

如果我们写的代码难以阅读，难以拓展，代码中充满了坏味道。那每当改变的时候，我们会深深的陷入开发。因此，开发周期内所编写的东西对于整个项目质量是至关重要的。

Senior developers, who know about quality, don't have this problem. They feel proud when they see their code has the quality factors which junior developers only dream about. Hence senior developers and experts have distilled a set of principles down to junior developers which they can apply to write quality code and to show off in front of other developers. If you want to write down the principles and techniques that find helpful, then here is a guide to extract principles from your own experience.

高级开发工程师，他们知道什么是代码质量，并且不会有上述问题，他们感到自豪，因为可以写出初级开发者所不能及的高质量代码。因此高级开发者和专家们总结出一系列原则，以供初级开发者们学习，便于他们写出高质量的代码。如果你想要写一些自己在开发中的总结经验，可以在这里填写 [成为专家](https://simpleprogrammer.com/2016/12/28/software-design-patterns-hiding/)


In this post, I will cover SOLID principles. These principles are given by Uncle Bob (Robert C. Martin). I will also cover GRASP (General Responsibility Assignment Software Principles) published by Craig Larman and a few other basic object-oriented design principles. I have included examples from my personal experience, therefore you will not find any 'Animal' or 'Duck' examples.

在这篇博文中，我会阐述SOLID原则，这些原则由Robert C. Martin提出，我们也将阐述GRASP原则，而这一部分由Craig Larman和一些面对对象设计原则的基本构成，我将从我个人的经验中举出一些例子，所以你不会看到经常在书本上被滥用的Animal和Duck例子。

The code examples shown are closer to Java and C#, but they are helpful to any developer who knows the basics of object oriented programming.

示例代码比较接近Java和C#，但是适用于对面向对象编程基础的任何开发者。


Here is the complete list of principles covered in this post:
这里是完整的原则列表：

1. Single Responsibility Principle (SOLID) 单一责任性原则
2. High Cohesion (GRASP) 高内聚
3. Low Coupling (GRASP) 低耦合
4. Open Closed Principle (SOLID) 开闭原则
5. Liskov Substitution principle (SOLID)  里氏替换原则
6. Interface Segregation Principle (SOLID) 接口分离原则
7. Dependency Inversion Principle (SOLID) 依赖倒置原则
8. Program to an Interface, not to an Implementation 面向接口编程
9. Hollywood Principle 控制反转
10. Polymorphism (GRASP) 多态模式
11. Information Expert (GRASP) 信息专家模式
12. Creator (GRASP) 创造者模式
13. Pure Fabrication (GRASP) 纯虚构模式
14. Controller (GRASP) 控制器模式
15. Favor composition over inheritance
16. Indirection (GRASP) 中介模式


One mistake that I made early in my early career is that I tried to apply all the above principles at one time. That was a big mistake. I don’t want you to do that. I have designed a very simple process that allows you to apply basic design skills, and then build upon from there. I compile that process in a free report. 
在我早期职业生涯中我经常犯一个错误，总说试图一次性使用这些所有的原则。这是一个巨大的认知错误，我希望你引以为戒。



[点击这里下载免费报告](http://eepurl.com/ccscu1)

 
#单一功能原则
单一功能原则（SRP）定义：

**每个类应该只负责一种单一的功能**

一个类利用它的函数或者契约（以及函数相关的成员变量）来执行其功能。
我们来看下面这个类
 
	Class Simulation{
		Public LoadSimulationFile()
		Public Simulate()
		Public ConvertParams()
	}
这个类有两个功能。第一，装载仿真数据，第二，执行仿真算法（使用Simulate和ConvertParams方法）。
类使用一个或多个函数来执行功能。在上例中，加载仿真数据是一个功能，执行仿真算法是另一个功能。加载仿真数据需要一个函数（LoadSimulationFile），而剩下的两个函数用来实现算法的功能。
那么如何分辨自己的类有哪些功能呢？参考功能的定义短语为“改变的原因”。因此，寻找一个类改变的所有原因，如果有一个以上的理由需要改动这个类，那么这意味着这个类并没有遵守单一功能原则。
上面的示例中，这个类不应该包含LoadSimulationFile函数（或者装载仿真数据的功能）。如果我们创建一个单独的类来加载模拟数据，那么这个类就不再违反SRP原则了。

**一个类只能有一个功能**，那么在设计软件的时候我们如何去遵守这个严格的规则？
让我们考虑另一个与SRP密切相关的原则：高内聚。高内聚给了你一个主观的尺度，而不像SRP那样是一个客观指标。极低内聚意味着这个类执行了很多的功能，例如一个类负责了超过10个功能。低内聚意味着一个类执行大约5个功能，而中等内聚







