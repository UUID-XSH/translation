# Junit document

JUnit是由 Erich Gamma 和 Kent Beck 编写的一个回归测试框架（regression testing framework）。Junit测试是程序员测试，即所谓白盒测试，因为程序员知道被测试的软件如何（How）完成功能和完成什么样（What）的功能。 

## Junit的特点

- JUnit是用于编写和运行测试的开源框架
- 提供了注释，以确定测试方法
- 提供断言测试预期结果
- JUnit优雅简洁
- JUnit测试可以自动运行，并提供即时反馈
- Junit显示测试进度，如果测试通过为绿色，测试失败则会变成红色

JUnit是Java中最有名的单元测试框架。然而，它仅适合于纯粹的单元测试，对于集成测试应该使用例如TestNG等来代替。通常对于每一项测试点需要一个正测试一个负测试。

### 1.添加maven依赖

    <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
	</dependency>



### 2.Test Runner
<p>使用注解:

	@RunWith(SpringJUnit4ClassRunner.class)




### 3.加载测试配置

	@SpringApplicationConfiguration(classes = Application.class)
<p>其中``Application.class``为自定义的配置类.




### 4.Assertions:断言
<p>我们在确定被测试代码的正确性时，总是会在特定的时刻做出一些假设，比如代码

	public int sum(int a,int b){
		 int sum = a + b;
		 return sum;
	}
	 
当我们给定输入a=1,b=2时，当且仅当sum=3时我们才认为该逻辑是正确的.那么我们在测试代码中添加如下断言：

	public void sumTest(){
	    int a = 1;
	    int b = 2;
	    int sum = sum(a,b)
	    Assert.assertTrue(sum == 3);
	}
<p>Junit为primitive类型和Objects提供了一系列重载的断言方法,它们都在类``org.junit.Assert``中,以下是该类常用方法和说明:

	//condition为真,则断言成功;反之断言失败,并抛出AssertionError
	static public void assertTrue(boolean condition)</code>

	//同上,message为断言失败时,AssertionError的标识信息
	static public void assertTrue(String message, boolean condition)</code>

	//condition为假,则断言成功;反之断言失败,并抛出AssertionError
	static public void assertFalse(boolean condition)</code>

	//同上,message为断言失败时,AssertionError的标识信息
	static public void assertFalse(String message, boolean condition)</code>

	//若期望值(expected)与实际值(actual)相等,断言成功,反之断言失败,并抛出AssertionError
	private static boolean isEquals(Object expected, Object actual)</code>

	//同上,message为断言失败时,AssertionError的标识信息
	static public void assertEquals(String message, Object expected,Object actual)

	//断言两个对象不相等,如果相等,则抛出AssertionError
	static public void assertNotEquals(Object unexpected, Object actual)</code>

	//同上,message为断言失败时,AssertionError的标识信息
	static public void assertNotEquals(String message, Object unexpected,Object actual)

	//断言object为null,若object不为null,则抛出AssertionError
	 static public void assertNull(Object object)

	//同上,message为断言失败时,AssertionError的标识信息
	static public void assertNull(String message, Object object)
<p>更多方法请参考`org.junit.Assert`




### 5.Exception testing:测试异常

有时候，抛出异常是一个方法正确工作的一部分。比如创建一个``ArrayList``但是没有初始化，此时调用``get``方法会抛出``IndexOutOfBoundsException``异常，那么如何测试这个异常呢？

<p>考虑如下代码:

	new ArrayList<String>.get(0);
我们在Junit中有三种方法去验证该异常的发生.

* 使用`@Test`的`expected`参数，通过annotation实现
* 使用`Try/Catch`
* 使用`ExpectedException Rule`

<p>示例如下：

	public class SomeTest {

		/**
		 * 方法1
		 */
		@Test(expected = IndexOutOfBoundsException.class)
		public void empty() {
			new ArrayList<String>().get(0);
		}

		/**
		 * 方法2
		 */
		@Test
		public void testExceptionMessage() {
			try {
				new ArrayList<String>().get(0);
				fail();
			} catch (IndexOutOfBoundsException e) {
				Assert.assertNotNull(e);
				Assert.assertTrue("Index: 0, Size: 0".equals(e.getMessage()));
			}
		}

		/**
		 * 方法3
		 */
		@Rule
		public ExpectedException thrown = ExpectedException.none();

		@Test
		public void chuckTest() {
			Thing thing = new Thing();
			thrown.expect(Exception.class);
			thrown.expectMessage("some message");
			thrown.expectMessage(CoreMatchers.containsString("some"));    //模糊匹配
			thing.chuck();
		}
	
		private class Thing {
			public void chuck() {
				throw new IllegalArgumentException("some message");
			}
		}

	}
	
方法一中``@Test(expected = IllegalArgumentException.class)``表示验证这个测试方法将抛出``IllegalArgumentException``异常，如果没有抛出的话，则测试失败。

方法二利用`Try/Catch`块对异常进行捕获，然后分析验证异常类型来进行测试。

方法三功能更为强大，标准的JUnit的`org.junit.Test`注解提供了一个expected属性，可以用它指定一个`Throwble`类型，如果方法调用中抛出了这个异常，这条测试用例就算通过了。如果想验证异常的具体信息，那就需要使用`ExpectedException`来实现。在上述示例中，我们期望抛出的异常里包含指定的信息“some message”，和仅匹配类型相比，采用`ExpectedException`进行验证更安全。假设有一个ExceptionThrower:

	class ExceptionsThrower {
    void throwRuntimeException(int i) {
        if (i <= 0) {
            throw new RuntimeException("Illegal argument: i must be <= 0");
        }
        throw new RuntimeException("Runtime exception occurred");
    	}
	}
可以看到，抛出的两个异常都是`RuntimeException`，因此如果不检查异常具体信息的话，无法百分百确定方法抛出的异常到底是哪个，进而造成测试的遗漏。检查异常信息能确保程序抛出的是哪个异常，单就这点来说，采用异常规则进行测试就能秒杀第一种方法中的expected属性了。

如果想验证的异常信息非常复杂，`ExpectedException`允许传入一个Hamcrest匹配器（matcher)给`expectMessage`方法，即:

	thrown.expectMessage(startsWith("some")); 

当然还可以指定自己的匹配器来进行消息验证:

	thrown.expectMessage(CoreMatchers.containsString("some")); 
`CoreMatchers`为自定义的异常匹配器。

### 6.测试超时
``timeout``属性，用来测试性能，即测试一个方法能不能在规定时间内完成。
假设sort方法采用冒泡排序，随机生成一个长度为50000的数组然后测试排序所用时间。timeout的值为2000，单位为毫秒，即超出2秒将视为测试不通过。

	@Test(timeout = 2000)
    public void testSort() throws Exception {
        int[] arr = new int[50000]; //数组长度为50000
        int arrLength = arr.length;
        //随机生成数组元素
        Random r = new Random();
        for (int i = 0; i < arrLength; i++) {
            arr[i] = r.nextInt(arrLength);
        }

        new Math().sort(arr);
    }
运行结果测试不通过，且提示TestTimedOutException。
<img src="http://fmn.rrimg.com/fmn073/20160913/1020/original_2yvo_2c72000a421d1e84.jpg" width = "800" height = "150">
将sort方法改为快速排序，再运行一下测试类`MathTest`，测试通过，说明代码性能获得了提升。


### 7.Ignoring a Test:忽略测试
出于某种原因,你可能希望忽略某些测试,你可以使用注解`@Ignore`,被忽略的测试方法仍然会被Test runners记录下来:

    public class SomeTest {

	/**
	 * 方法1
	 */
	@Test(expected = IndexOutOfBoundsException.class)
	public void empty() {
		new ArrayList<String>().get(0);
	}

	/**
	 * 方法2
	 */
	@Ignore
	@Test
	public void testExceptionMessage() {
		try {
			new ArrayList<String>().get(0);
		} catch (IndexOutOfBoundsException anIndexOutOfBoundsException) {
			Assert.assertNotNull(anIndexOutOfBoundsException);
			Assert.assertTrue("Index: 0, Size: 0".equals(anIndexOutOfBoundsException.getMessage()));
		}
	}

	/**
	 * 方法3
	 */
	@Rule
	public ExpectedException thrown = ExpectedException.none();

	@Test
	public void chuckTest() {
		Thing thing = new Thing();
		thrown.expect(Exception.class);
		thrown.expectMessage("some message");
		thrown.expectMessage(CoreMatchers.containsString("some"));    //模糊匹配
		thing.chuck();

	}


	private class Thing {
		public void chuck() {
			throw new IllegalArgumentException("some message");
			}
		}
	}
结果如下:
<img src="http://fmn.rrfmn.com/fmn079/20160913/0945/original_0daj_777800028d1d1e83.jpg" width = "700" height = "300">


### 8.执行顺序
<p>默认情况下,一个测试类中的测试方法是以一个确定的但不可预测的顺序执行``（MethodSorters.DEFAULT）``,编写良好的测试代码之间并不需要假定任何执行顺序.但是我们仍然可以通过注解`@FixMethodOrder`来指定顺序:

	@RunWith(SpringJUnit4ClassRunner.class)
	@SpringApplicationConfiguration(classes = Application.class)
	@FixMethodOrder(MethodSorters.NAME_ASCENDING)
	public class SumTest {
		@Test
		public void C(){
			System.out.println("C");
		}

		@Test
		public void B(){
			System.out.println("B");
		}
		
		@Test
		public void A(){
			System.out.println("A");
		}
	}
<p>上述代码将按字典顺序执行,结果为:

	A
	B
	C

可以使用注解Annotation来定义测试规则:

- ``@Test``：把一个方法标记为测试方法
- ``@Before``：每一个测试方法执行前自动调用一次
- ``@After``：每一个测试方法执行完自动调用一次
- ``@BeforeClass``：所有测试方法执行前执行一次，在测试类还没有实例化就已经被加载，所以用static修饰
- ``@AfterClass``：所有测试方法执行完执行一次，在测试类还没有实例化就已经被加载，所以用static修饰

新建一个测试类AnnotationTest:

	public class AnnotationTest {
	
		public AnnotationTest() {
			System.out.println("构造方法");
		}

		@BeforeClass
		public static void setUpBeforeClass() {
			System.out.println("BeforeClass");
		}

		@AfterClass
		public static void tearDownAfterClass() {
			System.out.println("AfterClass");
		}

		@Before
		public void setUp() {
			System.out.println("Before");
		}

		@After
		public void tearDown() {
			System.out.println("After");
		}

		@Test
		public void test1() {
			System.out.println("test1");
		}

		@Test
		public void test2() {
			System.out.println("test2");
		}

	}
运行结果为:

	BeforeClass
	构造方法
	Before
	test1
	After
	构造方法
	Before
	test2
	After
	AfterClass
``@BeforeClass``和``@AfterClass``在类被实例化前（构造方法执行前）就被调用了，而且只执行一次，通常用来初始化和关闭资源。``@Before``和``@After``和在每个``@Test``执行前后都会被执行一次。``@Test``标记一个方法为测试方法.JUnit为了保证每个测试方法都是单元测试，是独立的互不影响,所以每个测试方法执行前都会重新实例化测试类，所以构造方法被执行了两次。

### 9.Transaction事务管理
在进行单元测试时,有一些方法需要对数据库进行操作.但是这个操作只是用来进行测试,并不想真正的对数据库中的真实数据进行修改,这就需要测试完成后将数据rollback到方法执行前的状态,这时候单元测试就需要借助spring transaction的事务管理来完成.
Spring Boot使用事务非常简单，首先使用注解``@EnableTransactionManagement``开启事务支持后，然后在访问数据库的Service方法上添加注解``@Transactional``便可，如果注解在类上，则整个类的所有方法都默认支持事务。

<p>示例:

	@RunWith(SpringJUnit4ClassRunner.class)
	@SpringApplicationConfiguration(classes = Application.class)
	@WebAppConfiguration
	@Transactional
	public class SomeServiceTest {
	
	@Autowired
	SomeService someService;

	@Before
	public void prepare() {
		...
	}

	@Test
	public void saveListTest() {
		List<String> stringList = new ArrayList<>();
		stringList.add("some thing");
		List<String> result = someService.saveList(stringList);
		}
	}
上例中调用``saveList``方法将stringList数据存入数据库,测试完成后自动回滚到``SomeServiceTest``方法执行前的状态。
