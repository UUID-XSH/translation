# Mockito document
## 1.什么是Mock对象？
<p>示例：
<div align=center>
<img src="http://dl.iteye.com/upload/attachment/0067/5882/02030f95-ba9a-3104-b0f1-d7d8f02029fd.png" >
</div>
如果我们要对Class A进行测试，那么就需要先把整个依赖树构建出来，也就是BCDE的实例，一种替代方案就是使用mocks。
<div align=center>
<img src="http://dl.iteye.com/upload/attachment/0067/5884/2fab8997-d489-396e-9365-2ae1fe94b6c2.png" >
</div>
从图中可以清晰的看出：

- mock对象就是在调试期间用来作为真实对象的替代品
- mock测试就是在测试过程中，对那些不容易构建的对象用一个虚拟对象来代替的测试方法

接下来我们认识一下mock框架---Mockito。
## 2.mockito实例
<p>
### 添加maven依赖
<p>在pom文件中增加``spring-boot-starter-test``依赖，这个starter包括了spring-test依赖以及其他测试框架，例如``JUnit``和``Mockito``等等。

      	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
### 有两种方法创建Mock对象：
<p>

1 使用注解：在被测对象上加注解``@InjectMocks``，需要Mock的对象上加注解``@mock``，并在``@Before``注解的setUp()中进行初始化；

    public abstract class MockitoTest {
        @InjectMocks
	    private CreditLoanController creditLoanController;

	    @Mock
	    DataImportForCredit dataImportForCredit;
	    
    	@Before
    	public void setup() throws Exception {
        // 初始化测试用例类中由Mockito的注解标注的所有模拟对象
        MockitoAnnotations.initMocks(this);   	
         }
    }

2 直接Mock对象：
   
    public abstract class MockitoTest {      

	    private DataImportForCredit dataImportForCredit;
	    
    	@Before
    	public void setup() throws Exception {
        dataImportForCredit = mock(DataImportForCredit.class); 	
        ...
         }
    }

### 验证程序行为
<p>示例mock了一个List,因为大家熟悉它的接口，实际使用中请不要mock一个List类型,使用真实的实例来替代。（注意，不能对final，Anonymous ，primitive类进行mock。）

     // 静态导入会使代码更简洁
	 import static org.mockito.Mockito.*;

	 // 创建mock对象
	 List mockedList = mock(List.class);

	 // 使用mock对象
	 mockedList.add("one");
	 mockedList.clear();

     //verification 验证是否调用了add()和clear()方法
     verify(mockedList).add("one");
     verify(mockedList).clear();

其中，``verify``用于验证程序中某些方法的执行次数，默认为times(1)省略不写，可以设置次数num：

     Mockito.verify(mockedList, Mockito.times(num)).add(Mockito.any());          

验证方法从未调用：

	// 使用never()进行验证,never相当于times(0)
	 verify(mockedList, never()).add("never happened");
验证方法最少和最多调用次数：

	// 使用atLeast()/atMost()
	 verify(mockedList, atLeastOnce()).add("three times");
	 verify(mockedList, atLeast(2)).add("five times");
	 verify(mockedList, atMost(5)).add("three times");

### 如何做一些测试桩 (Stub)

- 默认情况下，所有的函数都有返回值。mock函数默认返回的是``null``；
- 测试桩函数可以被覆写，最近的一次覆写有效；
- 一旦测试桩函数被调用，该函数将会返回测试者设置的固定值；

示例:

	LinkedList mockedList = mock(LinkedList.class);
	// 测试桩
	Mockito.stub(mockedList.get(0)).toReturn("first");
	// 输出“first”
	System.out.println(mockedList.get(0));

通过``toThrow``设置某个方法抛出异常：

    Mockito.stub(mockedList.get(0)).toThrow(new Exception());

为返回值为void的方法设置通过``stub``抛出异常：

	doThrow(new RuntimeException()).when(mockedList).clear();

	// 调用这句代码会抛出异常
	mockedList.clear();	
为连续调用做测试桩:

	Mockito.stub(titanAsyncJob.isProcessing()).toReturn(false).toReturn(true);
	// 第一次调用:res1为false
	boolean res1 = titanAsyncJob.isProcessing();
	// 第二次调用:res2为true
	boolean res2 = titanAsyncJob.isProcessing();
### 参数匹配器 (matchers)
<p>参数匹配器使验证和测试桩变得更灵活,使用``equals()``与``anyX()``的匹配器会使得测试代码更简洁:

	verify(mock).someMethod(anyInt(), anyString(), eq("third argument"));
	// 正确,因为anyInt()代表任意整数，anyString()代表任意字符串，eq()是一个参数匹配器

	verify(mock).someMethod(anyInt(), anyString(), "third argument");
	// 错误,因为所有参数必须由匹配器提供，而参数"third argument"并非匹配器提供，会抛出异常
	 
注意，不能在验证或者测试桩函数之外使用``anyObject()``, ``eq()``函数。

### 验证方法执行顺序

	 // 验证mock对象的函数执行顺序
	 List singleMock = mock(List.class);

	 singleMock.add("was added first");
	 singleMock.add("was added second");

	 // 为该mock对象创建一个inOrder对象
     InOrder inOrder = inOrder(singleMock);

	 // 验证add函数首先执行的是add("was added first"),然后是add("was added second")
	 inOrder.verify(singleMock).add("was added first");   
	 inOrder.verify(singleMock).add("was added second");
