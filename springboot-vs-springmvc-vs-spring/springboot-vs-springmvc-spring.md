<h3> Spring Boot vs. Spring MVC vs. Spring: How Do They Compare?</h3>


<p>In this article, you will receive overviews of Spring, Spring MVC, and Spring Boot, learn what problems they solve, and where they're best applied. The most important thing that you will learn is that Spring, Spring MVC, and Spring Boot are not competing for the same space. They solve different problems and they solve them very well.</p>

<p>在这篇文章中，你将对Spring，SpringMVC以及SpringBoot有一个大概的预览，认识到它们解决的问题，以及合适它们的使用场景。你将认识到的最重要的事情是Spring，SpringMVC和SpringBoot这三者不是为了竞争同一领域。它们是为了解决不同的问题，并且表现出色。

<h4>What Is the Core Problem That Spring Framework Solves?</h4>
<h4>Spring Framework 解决的核心问题是什么？</h4>

<p>Think long and hard. What’s the problem Spring Framework solves?</p>
<p>请认真的思考一下，什么是Spring Framework要解决的问题？</p>


<p>The most important feature of Spring Framework is Dependency Injection. At the core of all Spring Modules is Dependency Injection or IOC Inversion of Control.</p>
<p>Spring Framework的最重要的特征是依赖注入。所有Srping模块的核心是依赖注入或者IOC控制反转。</p>

<p>Why is this important? Because, when DI or IOC is used properly, we can develop loosely coupled applications. And loosely coupled applications can be easily unit tested.
   Let’s consider a simple example.
<p>为什么重要？因为如果我们合理的使用了依赖注入和控制反转，我们就有能力开发出松散解耦的应用，从而很容易的进行单元测试。


<h4>Example Without Dependency Injection</h4>
<h4>不用依赖注入的示例</h4>

<p>Consider the example below: WelcomeController depends on WelcomeService to get the welcome message. What is it doing to get an instance of WelcomeService?</p>
<p>思考一下下面的示例：WelcomeController依赖于WelcomeService以获得欢迎信息。那么它将如何获取一个WelcomeService的实例呢？</p>

```
WelcomeService service = new WelcomeService();
```

<p>It’s creating an instance of it. And that means they are tightly coupled. For example: If I create a mock for WelcomeService in a unit test for WelcomeController, how do I make WelcomeController use the mock? Not easy!</p>
<p>上述代码创建了一个WelcomeService的实例。这意味着他们紧紧的耦合在一起。举个例子，如果我在一个单元测试中为WelcomeController创造一个WelcomeService的mock对象，那我应该如何使WelcomeController使用它？做到这点很不容易。</p>

```
@RestController
public class WelcomeController {
private WelcomeService service = new WelcomeService();
@RequestMapping("/welcome")
public String welcome() {
    return service.retrieveWelcomeMessage();
}
}
```

<h4>Same Example with Dependency Injection</h4>
<h4>使用依赖注入的相同案例</h4>

<p>The world looks much simpler with dependency injection. You let the Spring Framework do the hard work. We just use two simple annotations: @Component and @Autowired.</p>
<p>世界因为依赖注入而变得更加简单。于是你把困难交给了spring框架。我们只需要使用两个简单的注解：@Component和@Autowired</p>

<p>Using @Component , we tell Spring Framework: Hey there, this is a bean that you need to manage.
Using @Autowired , we tell Spring Framework: Hey find the correct match for this specific type
and autowire it in.</p>

<p>通过使用@Component，我们告诉Spring框架：这里是一个bean，你需要去管理。
使用@Autowired，我们告诉spring框架：去帮我找到匹配的类型并注入进来。</p>


<p>In the example below, Spring framework would create a bean for WelcomeService and autowire it into WelcomeController.</p>
<p>在下面的例子中，spring框架将会为WelcomeService创建一个bean病把它注入到WelcomeController中。</p>

<p>In a unit test, I can ask the Spring framework to auto-wire the mock of WelcomeService into WelcomeController. (Spring Boot makes things easy to do this with @MockBean. But, that’s a different story altogether!)</p>
<p>在一个单元测试中，我可以请求spring框架将一个WelcomeService的mock对象注入到WelcomeController中。（SpringBoot通过使用@MockBean让这件事情变得更加简单。但是，二者放在一起则是一个另外的故事！）

``` 
@Component
public class WelcomeService {
    //Bla Bla Bla
}

@RestController
public class WelcomeController {

@Autowired
private WelcomeService service;

@RequestMapping("/welcome")
public String welcome() {
    return service.retrieveWelcomeMessage();
}
}
```

<h4>What Else Does Spring Framework Solve?</h4>
<h4>Spring框架还解决了什么？</h4>

<h5>Problem 1: Duplication/Plumbing Code</h5>
<h5>问题1：重复/管道代码</h5>

<p>Does Spring Framework stop with Dependency Injection? No. It builds on the core concept of Dependency Injection with a number of Spring Modules
Spring JDBC 
Spring MVC 
Spring AOP 
Spring ORM 
Spring JMS 
Spring Test</p>

<p>Spring框架止步于依赖注入了吗？不。它构建了一系列以依赖注入为核心的spring模块。</p>

<p>Consider Spring JMS and Spring JDBC for a moment.
   Do these modules bring in any new functionality? No. We can do all this with J2EE or Java EE. So, what do these bring in? They bring in simple abstractions. The aim of these abstractions is to
   Reduce Boilerplate Code/Reduce Duplication Promote Decoupling/Increase Unit Testability
   For example, you need much less code to use a JDBCTemplate or a JMSTemplate compared to a traditional JDBC or JMS.</p>
   
<p></p>