#SOLID, GRASP和其他面向对象设计的基本原则
##学习面向对象的设计原则，并牢牢掌握SOLID和GRASP规则背后的思想

（by Muhammad Umair  ·  Feb. 13, 17 · Web Dev Zone，[原文链接](https://dzone.com/articles/solid-grasp-and-other-basic-principles-of-object-o) ）

今天开始编程，体验与Qlik合作带来的驱动数据应用开发的强大引擎。

从一些老生常谈的事情开始说起来吧，软件代码应该符合以下特质：

- 可维护性
- 可扩展性
- 模块化

每当你想要质问代码是否满足以上标准的时候，就会将自己置于困难之中。

如果要使软件代码在其生命周期内维护起来更简单，可拓展更强且模块化，那么我们认为这种代码必须具有高于平均代码质量的一些特征。

我曾经编写过难以阅读，难以拓展，并且充满了坏味道的代码。在经历了大约6个月的开发学习后，才终于有了一些改善。因此，开发过程中的经历对于理解项目代码质量来说是至关重要的。

高级开发工程师，他们理解什么是代码质量，因此不会有上述问题。他们感到自豪，因为可以写出初级开发者所不能及的高质量代码。因此高级开发者和专家们总结出一系列原则，以供初级开发者们学习，便于他们写出高质量的代码。

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
9. Hollywood Principle 好莱坞原则
10. Polymorphism (GRASP) 多态模式
11. Information Expert (GRASP) 信息专家模式
12. Creator (GRASP) 创造者模式
13. Pure Fabrication (GRASP) 纯虚构模式
14. Controller (GRASP) 控制器模式
15. Favor composition over inheritance 组合优于继承
16. Indirection (GRASP) 中介模式

在我早期职业生涯中我经常犯一个错误，总说试图一次性使用这些所有的原则。这是一个巨大的认知错误，我希望你引以为戒。
 
#Single Responsibility Principle（单一责任原则）
单一责任原则（SRP）定义：

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

#Open-Closed Principle（开闭原则）

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
在这个示例中，客户端读取（ds.Read()）来自于网络数据流。如果我想要扩展这个客户端的功能使之能够读取其他数据流的内容，例如PCI数据流，那么我需要添加另外继承自DataStream的子类，如下所示：

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

#Liskov Substitution Principle（里氏替换原则）

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

从面向对象分析的视角来看，有另一个不同的角度去解释LSP原则。总的来说，通过OOA，类和它们的层级结构将会是我们软件设计需要考虑的一个部分。

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

#Interface Segregation Principle（接口隔离原则）
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


# Dependency Inversion Principle（依赖反转）
<p>这条原则是对上面所讨论的原则的一个概述。
<p>在我们给出DIP的书面定义之前，请让我介绍一个与此紧密相连的一条原则，以帮助我们理解DIP。
<p>这条原则是：面向接口编程，而不是面向实现编程。

<p>考虑下面这个简单的例子：

```
Class PCIDevice{
    Void open(){}
    Void close(){}
}
Static void Main(){
    PCIDevice aDevice = new PCIDevice();
    aDevice.open();
    //do some work
    aDevice.close();
}
```
<p>上面的示例代码违背了面向接口编程，因为我们正在操作的引用是具体的类对象PCIDevice，下面的代码则遵循了这个原则：

```
Interface IDevice{
    Void open();
    Void close();
}
Class PCIDevice implements IDevice{
    Void open(){ // PCI device opening code }
    Void close(){ // PCI Device closing code }
}
Static void Main(){
    IDevice aDevice = new PCIDevice();
    aDevice.open();
    //do some work
    aDevice.close();
}
```
<p> 所以这条原则很容易去遵循。依赖反转与此类似，但需要我们做的更多。

<p>依赖反转：高级模块不应该依赖低级模块。二者应该依赖于抽象。

<p>你可以把"两者都应该依赖于抽象"简单的理解成面向接口编程。那么什么是高级模块和低级模块？

<p>为了这条原则的第一部分，我们需要理解高级模块和低级模块实际上是什么。看如下代码：

```
Class TransferManager{
public void TransferData(USBExternalDevice usbExternalDeviceObj,SSDDrive  ssdDriveObj){
            Byte[] dataBytes = usbExternalDeviceObj.readData();
           // work on dataBytes e.g compress, encrypt etc..
            ssdDriveObj.WrtieData(dataBytes);
        }
}
Class USBExternalDevice{
Public byte[] readData(){
        }
}
Class SSDDrive{
Public void WriteData(byte[] data){
}
}
```
<p>上面的代码有三个类，TransferManager代表高级模块，因为它在一个方法中用了其它两个类。因此其他两个类则是低级模块。

<p>上面的代码中，高级模块是直接使用低级模块的(没有任何抽象)，因此违背了依赖反转原则。

<p>违背了依赖反转这条原则会让你的软件系统变得难以更改。比如，如果你想增加其他的外部设备，你将不得不
改变高级模块。因此你的高级模块将会依赖于低级模块，依赖会让代码变得难以改变。

<p>如果你理解了上面的原则"面向接口编程"，那么就很容易解决这个问题:

```
Class USBExternalDevice implements IExternalDevice{
Public byte[] readData(){
}
}
Class SSDDrive implements IInternalDevice{
Public void WriteData(byte[] data){
}
}
Class TransferManager implements ITransferManager{
public void Transfer(IExternalDevice externalDeviceObj, IInternalDevice internalDeviceObj){
           Byte[] dataBytes = externalDeviceObj.readData();
           // work on dataBytes e.g compress, encrypt etc..
           internalDeviceObj.WrtieData(dataBytes);
        }
}
Interface IExternalDevice{
        Public byte[] readData();
}
Interfce IInternalDevice{
Public void WriteData(byte[] data);
}
Interface ITransferManager {
public void Transfer(IExternalDevice usbExternalDeviceObj,SSDDrive  IInternalDevice);
}
```
在上面的代码中，高级模块和低级模块都依赖于抽象，符合了依赖反转原则。

# Hollywood（好莱坞原则）
<p>这条原则和依赖反转原则类似：不要调用我们，我们会给你。

<p>这意味着高级组件可以以一种互不依赖的方式去支配低级组件。

<p>这条原则可以防止依赖恶化。依赖恶化发生在每个组件都依赖于其他各个组件。换句话说，依赖恶化是让依赖发生在各个方向(向上，横向，向下)。
Hollywood原则可以让我们时依赖只向一个方向。

<p>DIP和Hollywood之间的差异给了我们一条通用原则：无论是高级组件还是低级组件，都要依赖于抽象而不是具体的类。另一方面，Hollywood原则
强调了高级组件和低级组件应该以不产生依赖的方式交互。

# Polymorphism（多态）
<p>什么？多态也是设计原则？对的，多态是任何面向对象语言都要提供的基础特征，它可以让父类的引用指向子类。

<p>它同时也是GRASP的设计原则之一。这条原则为你在面向对象设计中提供了知道方针。


<p>这条原则严格限制了运行时类型信息的使用(RTTI)。在C#中，我们用如下方式实现RTTI：

```
if(aDevice.GetType() == typeof(USBDevice)){
//This type is of USBDEvice
}
```
<p>在java中，RTTI由getClass()或者instanceOf()完成：

```
if(aDevice.getClass() == USBDevice.class){
    // Implement USBDevice
     Byte[] data = USBDeviceObj.ReadUART32();
}

```
<p>如果你曾在你的项目中编写了类似的代码，那么现在是时候重构你的代码以符合多态原则，看如下例图:

<img src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/PolymorphismDiagram.jpg"/>

<p>在此我在接口中生成了read方法，然后委托他们的实现类去实现该方法，现在，我只用方法Read:

```
//RefactoreCode
IDevice aDevice = dm.getDeviceObject();
aDevice.Read();
```
getDeviceObject()的实现来自哪里？这是我们即将要讨论的创造者原则和信息专家原则

# Information Expert（信息专家原则）

<p>这是GRASP的一条简易原则，它在如何给予类权限方面给我们提供指导。它表明你应该在一个类有足够的信息去满足某项职能的时候才能赋予它该只能。考虑下面的类：

<img src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/InformationExpert.jpg"/>

<p>在我们的场景中，一个模式全速启动（每秒600次循环），而用户的显示器以降低的速度更新。那么，我将不得不指定一个职责以决定下一帧需不需要展示。

<p>哪一个类负责担任这项职责？我有两个选择:Simulation类或者SpeedControl类。

<p>既然SpeedControl类有关于当前顺序下已经被展示的帧，那么依据信息专家原则，SpeedControl负责该职责。


# Creator（创造者原则）
<p>创造者是GRASP中负责决定某类负责创造另一个类的实例。对象的创造是非常重要的，因此有原则的去决定创造者的角色是很有用的。

<p>根据Larman所言，类B在以下任何一个条件为真时，应该负担起对A的创造：

* B包含A
* B聚合A
* B含有初始化A所需有的信息
* B记录A
* B闭合式的使用A

<p>在我们多态的例子中，我使用了信息专家原则和创造者原则，使得DeviceManager类得到创建Device对象的职责。
这是因为DeviceManger有创建Device所需要的信息。

# Pure Fabrication（纯虚构模式）

<p>为了理解纯制造，首先你要理解面向对象分析（OOA）

<p>面向对象分析是一个通过问题领域去确定类的过程。举个例子，银行系统的领域模型应该包含诸如类Account, Branch, Cash, Check, Transaction
等等。这些领域类需要存储关于客户的信息。实现该原则的一个选择是把数据的存储委托给领域类。这个选择会降低领域类的粘性。
最后这个选择违背了SRP原则。

<p>另一个选择是引入其他不含任何领域概念的类。在银行系统中，我们可以引入被称为持久化提供者的类。这个类不带便任何领域实体。其目的是为了处理存储方法。因此
PersistenceProvider是符合纯制造原则的。

# Controller（控制器原则）
<p>当我开始开发软件系统时，我最常写的就是使用java swing组件，大部分逻辑都在监听器后面。

<p>然后我学习了有关领域类模型的知识。也由此将我的逻辑从监听器移到领域类。然而我直接从监听器里调用领域对象，这就使得GUI组件(监听器)和领域模型之间产生了依赖。
控制器设计模型有助于最小化这种依赖。

<p>控制器有两个目的，其一是封装系统操作。系统操作是指你的用户想要达到某个目的，例如购买某件商品给或者将某个东西加入购物车。
这种系统操作是通过调用系统对象的一个或者多个方法来完成的。控制器的第二个目的是为UI和领域模型提供一层。

<p>UI可以让用户进行系统操作。控制器是UI层之后的第一个处理系统操作请求的对象然后将职责委托给底层的领域对象。

<p>例如，下面是一个MAP类，代表着一个控制器：

<imag src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/Controller.jpg"/>

<p>我们在UI层将职责"移动光标"委托给控制器，紧接着由控制器去调用底层的领域对象去移动游标。

<p>通过使用控制器原则，会使得你在增加其他用户接口（如命令行接口或者web接口）变得更加灵活。

# Favor Composition Over Inheritance（组合优于继承原则）

<p>在面向对象编程中，基本上有两种扩展已有代码功能的方法，第一个是继承。

<p>第二个方法是组合，用编程术语来说，你可以通过获取对象的一个引用来扩展这个对象的功能。组合的一个很有用的特征是它的行为可以在运行时才设定。而在继承中，你只有在编译器才能确定它的行为。我们将通过多个例子来说明这一点。

<p>当我是一个新手，我通过集成来扩展行为，如下：

<img src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/FavorCompositionOverInheritance1.jpg"/>

<p>最初，我只知道将要处理一段数据输入流，而数据有两种类型。过了几个星期之后，我意识到数据的字节顺序需要被处理。
因此按照如下图例改变了类设计：
<img src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/FavorCompositionOverInheritance2.jpg"/>

<p>之后，一个新的变量被增加到需求中。这次我需要处理数据的极性，极性有两种：Stream A, StreamB，流有很多种字节顺序，如此一来，类就爆炸了。我将不得不去维护
一大批类。

<p>如果我使用组合来处理相同的问题，类设计如下：

<img src="http://www.objectorienteddesign.org/wp-content/uploads/2016/12/FavorCompositionOverInheritance3.jpg"/>

<p>我新增了一些类，然后在我的代码中使用它们的引用：

```
clientData.setPolarity(new PolarityOfTypeA); // or clientData.setPolarity(new PolarityOfTypeB)
clientData.FormatPolarity;
clientData.setEndianness(new LittleEndiannes());// setting the behavior at run-time
clientData.FormatStream();
```
<p>如此一来，我就可以根据的想要的行为来提供类的实例，这个特征使得类的总数下降了，最终保证了可维护性。因此组合优于继承将会减少不可维护问题的发生，并且让你能更加灵活的在运行时设定你的行为。


# Indirection（间接原则）

<p>这个原则回答了一个问题：你如何使对象以解耦的方式进行交互。答案是：将交互的职责交给中间对象，使得相关组件的耦合度降低。

<p>例如，一个软件应用在多个配置和选择下都可以工作。要将领域代码与配置分离，需要添加一个特定的类，如下面的列表所示：



```
Public Configuration{
  public int GetFrameLength(){
    // implementation
  }
  public string GetNextFileName(){
  }
 // Remaining configuration methods
}
```

<p>这样，如果任何领域对象需要读取某个配置，只要让它去请求Configuration对象即可。因此，主要代码与配置代码完成了解耦。

<p>如果你已经阅读了纯构造原则，那么配置类就是该原则的一个实例。但是间接原则的目的是完成解耦，而纯构造原则的目的是为了维持领域模型的简介。

<p>许多软件设计模式诸如适配器模式，外观模式以及观察者模式都是间接原则的更专业化的体现。


