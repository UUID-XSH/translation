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
<p>上述代码创建了一个WelcomeService的实例。这意味着他们紧紧的耦合在一起。举个例子，</p>