#SOLID, GRASP和其他面向对象设计的基本原则
##学习面向对象的设计原则，并牢牢掌握SOLID和GRASP规则背后的思想

（by Muhammad Umair  ·  Feb. 13, 17 · Web Dev Zone）

今天开始编程，体验与Qlik合作带来的驱动数据应用开发的强大引擎。

从一些老生常谈的事情开始说起来吧，软件代码应该符合以下特质：

- 可维护性
- 可扩展性
- 模块化

每当你想要质问代码是否满足以上标准的时候，就会将自己置于困难之中。

如果要使软件代码在其生命周期内维护起来更简单，可拓展更强且模块化，那么我们认为这种代码必须具有高于平均代码质量的一些特征。

我曾经编写过难以阅读，难以拓展，并且充满了坏味道的代码。在经历了大约6个月的开发学习后，才终于有了一些改善。因此，开发过程中的经历对于理解项目代码质量来说是至关重要的。

高级开发工程师，他们理解什么是代码质量，因此不会有上述问题。他们感到自豪，因为可以写出初级开发者所不能及的高质量代码。因此高级开发者和专家们总结出一系列原则，以供初级开发者们学习，便于他们写出高质量的代码。如果你想要写一些自己在开发中的总结经验，可以在这里填写 [成为专家](https://simpleprogrammer.com/2016/12/28/software-design-patterns-hiding/)

在本篇文章中，我将阐述SOLID原则，这些原则由Robert C. Martin提出，同时我也将阐述GRASP原则，而这一部分由Craig Larman和一些面向对象的设计原则构成，我将用我个人的经验所得来举例，因此你不会看到经常在书本上被滥用的Animal和Duck的例子。

示例代码比较接近Java和C#，但是适用于有面向对象编程基础的任何开发者。

以下是完整的原则列表：

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

在我早期职业生涯中我经常犯一个错误，总说试图一次性使用这些所有的原则。这是一个巨大的认知错误，我希望你引以为戒。

[点击这里下载免费报告](http://eepurl.com/ccscu1)
 
#Single Responsibility Principle 单一功能原则
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
让我们考虑另一个与SRP密切相关的原则：高内聚。高内聚给了你一个主观的尺度，而不像SRP那样是一个客观指标。极低内聚意味着这个类执行了很多的功能，例如一个类负责了超过10个功能。低内聚意味着一个类执行大约5个功能，而中等内聚大约执行3个功能，而高内聚则一个类只有单一功能。因此，设计的经验法则是努力实现高内聚。

另一个需要在这里讨论的原则是低耦合。这个原则表明一个类应该独立完成特定的功能，使得类之间保持低依赖性。再次审视上面的示例类，在应用SRP和高内聚规则之后，我们决定创建一个独立的类来处理模拟数据文件。这样，我们就创建了两个互相依赖的类。
看起来采用高内聚似乎和低耦合原则相抵触了。因为原则是最小化耦合，并不是使耦合为零，因此这种程度的耦合是可以接受的。对于创建一个通过对象之间协作完成任务的面向对象的程序设计来说，一定程度的耦合是正常的。

另一方面，考虑一个链接数据库的GUI类，通过HTTP协议链接远程客户端并处理屏幕布局。这个GUI类依赖了太多的类，很明显违反了低耦合原则。如果不包含所有的相关类则该类不能被重用，任何对数据库组件的改变都将改变这个GUI类。

#Open-Closed Principle 开闭原则

开闭原则的描述为：

**一个软件模块（类或者方法）应该对拓展开放而对修改关闭**

换句话说，你无法对已经编写的工程代码进行修改，但是你能够为工程添加新的代码。

有两种方法可以实现开闭原则，即继承或组合。

下面的示例采用继承来实现开闭原则：

```
	Class DataStream{
		Public byte[] Read()
	}
	Class NetworkDataStream:DataStream{
		Public byte[] Read(){
		//Read from the network
	 }
	}
	
	Class Client {
		Public void ReadData(DataStream ds){
		ds.Read();
	 }
	}
```
在这个示例中，客户端读取数据（ds.Read()）来自于网络数据流。如果我想要扩展这个客户端的功能使之能够读取其他数据流的内容，例如PCI数据流，那么我需要添加另外继承自DataStream的子类，如下所示：

```	
	Class PCIDataStream:DataStream{
		Publc byte[] Read(){
		//Read data from PCI
	 }
	}
```
在这种情况下，客户端代码的运行没有任何错误。客户端认识基类，因此可以传递DataStream两个子类中的任何一个的对象，这样，客户端可以在未知子类的情况下读取数据。这是在不修改任何现有代码的情况下实现的。

我们还可以使用组合来实现这个原则，并且还有其他方法和设计模式来应用该原则，这些方法中的一部分将在本文中进行讨论。

然而，你必须将这个原则应用于每一段代码吗？当然不，因为大部分的代码其实是不怎么变动的，你只需要战略性的将这个原则应用到那些你预计将来会有变动的代码片上即可。

#Liskov Substitution Principle 里氏替换原则

里氏替换原则的描述为：

**子类应当可以替换父类并出现在父类能够出现的任何地方**

这个定义还可以理解为对一个客户端来说需要有足够的抽象（接口或抽象类）。

为了详细说明，让我们考虑一个例子，如下：

```
	Public Interface IDevice{
		Void Open();
		Void Read();
		Void Close();
	}
```
这个例子是数据采集装置的抽象。数据采集装置按其接口类型不同而区分，它能够使用USB接口，网络接口（TCP 或者 UDP），PIC接口或者另外的计算机接口。然而，客户端设备不需要知道与其链接的是何种数据采集装置。为了在不改变客户端代码的情况下适应新的采集装置，这就需要程序员给接口提供极大的灵活性。

当只有两个具体的类实现iDevice接口的情况如下所示：

```
	public class PCIDevice:IDevice {
		public void Open(){
		// Device specific opening logic
		}
	public void Read(){
		// Reading logic specific to this device
		}
	public void Close(){
		// Device specific closing logic.
		}
	}
	
	public class NetWorkDevice:IDevice{
		public void Open(){
		// Device specific opening logic
		}
	public void Read(){
		// Reading logic specific to this device
		}
	public void Close(){
		// Device specific closing logic.
		}
	}
```
这三个方法（打开、读取和关闭）对于处理设备传入的数据已经足够了。然后，假设需要添加基于USB接口的另一个数据采集设备。

USB设备的问题在于，当你打开连接时，来自先前连接的数据仍保留在缓冲区中。因此，在对USB设备的第一次读取调用时，返回来自上一个会话的数据。有针对性的采集行为会破坏这些数据。幸运的是，基于USB的设备驱动程序提供刷新功能，预先清除了基于USB的采集设备中的缓冲区。那么如何在代码中实现这个功能，并使代码的改动最小？

一个简单但是草率的解决方案是更新代码，通过标识识别是否调用USB设备，如下：

```
	public class USBDevice:IDevice{
		public void Open(){
			// Device specific opening logic
		}
		public void Read(){
    		// Reading logic specific to this device<br> 
		}	
		public void Close(){
			// Device specific closing logic.
		}
		public void Refresh(){
			// specific only to USB interface Device
		}
	}

	//Client code..
	Public void Acquire(IDevice aDevice){
        aDevice.Open();
        // Identify if the object passed here is USBDevice class Object.    
		if(aDevice.GetType() == typeof(USBDevice)){
		USBDevice aUsbDevice = (USBDevice) aDevice;
		aUsbDevice.Refresh();
		}
        // remaining code….
	}
```
在这个解决方案中，客户端代码直接使用具体类以及接口（或抽象类），这意味着抽象程度不足以使客户端执行其功能。

另一种陈述方式是基类无法满足需求（刷新操作），但是子类可以，实际上，子类有该项行为。因此，派生类和基类不兼容且子类不能被代替。所以，该解决方案违反了里氏替换原则。

下面这个示例中，客户端依赖于更多的实体（iDevices 和 USB Devices），一个实体的任何一点改变都将影响其他实体。因此，违反LSP原则将导致类之间的互相依赖。

下面我列出一种遵循LSP原则的解决方案：

```
	Public Interface IDevice{
		Void Open();
		Void Refresh();
		Void Read();
		Void Close();
	}
```
现在客户端如下：

```
	Public void Acquire(IDevice aDevice){
		aDevice.open();
		aDevice.refresh();
		aDevice.acquire()
		//Remaining code..
	}
```	
现在客户端不再依赖于设备的具体实现了，因此，这个方案中，我们的接口（设备）对于客户端来说是完备的了。

从面向对象分析的视角来看，有另一个不同的角度去解释LSP原则。[这里获取免费的面向对象分析的介绍](http://www.objectorienteddesign.org/)。总的来说，通过OOA，类和它们的层级结构将会是我们软件设计需要考虑的一个部分。

当我们考虑类和层级结构的时候我们可能会设计一些违反LSP规则的类。

让我们思考一个古典的例子，即长方形和正方形。一开始看起来正方形是长方形的特例，于是一个乐观的程序设计师将绘制出下面的层级继承关系：

```
	Public class Rectangle{
		Public void SetWidth(int width){}
		Public void SetHeight(int height){}
	}
	Public Class Square:Rectangle{
		//
	}
```
接下来你会发现你不能使用这个正方形的对象去代替长方形的对象了。因为正方形继承自长方形，所以它也继承了设置长度和宽度的方法。于是一个正方形的客户端能够随意改变自己的长和宽为不同的大小，但是实际上正方形的长宽应该是相同的，因此我们软件的这个正常行为就失败了。

这个问题只能根据不同的使用场景和条件具体分析类来避免。因此如果你孤立的设计一个类很可能在实际运行中将会出错。就像我们的正方形和长方形那样，一开始认为很完美的关系设计，在不同的条件下，这种关系设计最终被认定并不符合我们软件正常运行的要求。

#Interface Segregation Principle 接口隔离原则
接口隔离原则（ISP）描述为：

**客户端不应该被强迫依赖他们不使用的接口**

还是考虑前一个例子：

```
	Public Interface IDevice{
		Void Open();
		Void Read();
		Void Close();
	}
```
这个接口有三个实现类，USB设备、网络设备和PCID设备。这个接口对于网络和PCID设备来说够用，但是USB设备还需要另一个函数（Refresh()）才能运行正常。

和USB设备一样，也许还有另外的设备也需要这个函数来支持工作。为此，接口被更新如下：

```
	Public Interface IDevice{
		Void Open();
		Void Refresh();
		Void Read();
		Void Close();
	}
```
那么问题来了，任何一个实现该接口的类都需要去实现Refresh函数。

例如为了满足以上的设计，必须对网络设备和PCID设备添加下面的代码：

```
	public  void Refresh(){
		// Yes nothing here… just a useless blank function
	}
```
因此，这个设备基类被视为一个胖接口（过多的函数）。这种设计违反了接口隔离原则，因为它造成了客户端不必要的依赖。

有很多方法可以解决这个问题，不过我将在预定义的面向对象的解决方案范畴内处理这个问题。

我们知道在open操作之后就会直接调用refresh函数，因此，我改变逻辑将设备客户端的refresh函数迁移至具体的实现类内部。在本例中我将调用refresh的逻辑移动到USB设备的具体实现类中：

```
	Public Interface IDevice{
		Void Open();
		Void Read();
		Void Close();
	}
	Public class USBDevice:IDevice{
		Public void Open{
			// open the device here…
			// refresh the device
			this.Refresh();
		 }
		Private void Refresh(){
			// make the USb Device Refresh
		 }
	}
```
通过这个方式，我减少了基类中的函数数目，让它变得更轻了。

