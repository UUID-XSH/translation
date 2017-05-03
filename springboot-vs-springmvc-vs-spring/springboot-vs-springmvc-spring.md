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
   
<p>考虑一下Spring JMS and Spring JDBC两个模块。引入这些模块带来额外的功能了吗？没有。我们完全可以使用J2EE or Java EE去取代它们。那么我们为什么要引入？因为它带来了简单的抽象。这些抽象的目标是为了减少
样本代码和重复代码以及提高解耦度，增加单元可测试性。举个例子，相比于传统的JDBC或者JMS，使用JDBCTemplate或JMSTemplate可以使代码量变得更少。</p>

<h5>Problem 2: Good Integration With Other Frameworks</h5>
<h5>问题2：与其他框架更好的整合</h5>

<p>The great thing about Spring Framework is that it does not try to solve problems that are already solved. All that it does is to provide a great integration with frameworks which provide great solutions.
   Hibernate for ORM
   iBatis for Object Mapping
   JUnit and Mockito for Unit Testing</p>
   
<p>Spring框架的伟大之处在于它并不试图去解决已经被解决的问题。它做的所有的一切都是为了提供一个良好的环境去和其它提供解决方案的框架集成在一起。</p>

<h4>What Is the Core Problem That Spring MVC Framework Solves?</h4>
<h4>什么事Spring MVC解决的核心问题？</h4>

<p>Spring MVC Framework provides decoupled way of developing web applications. With simple concepts like Dispatcher Servlet, ModelAndView and View Resolver, it makes it easy to develop web applications.</p>
<p>Sring MVC提供了一种解耦的方法去开发web应用。通过一些诸如Dispatcher Servlet、ModelAndView以及View Resolver的简单概念去使得开发web应用变得更加简单。</p>

<h4>Why Do We Need Spring Boot?</h4>
<h4>为什么我们需要Spring Boot</h4>

<p>Spring based applications have a lot of configuration.When we use Spring MVC, we need to configure component scan, dispatcher servlet, a view resolver, web jars(for delivering static content) among other things.</p>
<p>基于Spring的应用会有很多配置。当我们使用Spring MVC时，我们需要配置组建扫描，servlet调度，视图解析，用于传递静态内容的web jar包以及其他。</p>

```
<bean
      class="org.springframework.web.servlet.view.InternalResourceViewResolver">
       <property name="prefix">
          <value>/WEB-INF/views/</value>
      </property>
      <property name="suffix">
          <value>.jsp</value>
      </property>
</bean>
<mvc:resources mapping="/webjars/**" location="/webjars/"/>
```

<p>The code snippet below shows the typical configuration of a dispatcher servlet in a web application.</p>
<p>下面的代码片段则示范了一个典型的在一个web应用中配置servlet分发器的过程。</p>

```
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/todo-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

<p>When we use Hibernate/JPA, we would need to configure a datasource, an entity manager factory, a transaction manager among a host of other things.</p>
<p>当我们使用Hibernate或者JPA，我们需要配置一个数据源，一个实体管理工厂，一个事物管理器以及很多其它事情。</p>

```
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
    destroy-method="close">
    <property name="driverClass" value="${db.driver}" />
    <property name="jdbcUrl" value="${db.url}" />
    <property name="user" value="${db.username}" />
    <property name="password" value="${db.password}" />
</bean>
<jdbc:initialize-database data-source="dataSource">
    <jdbc:script location="classpath:config/schema.sql" />
    <jdbc:script location="classpath:config/data.sql" />
</jdbc:initialize-database>
<bean
    class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
    id="entityManagerFactory">
    <property name="persistenceUnitName" value="hsql_pu" />
    <property name="dataSource" ref="dataSource" />
</bean>
<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionMana
    <property name="entityManagerFactory" ref="entityManagerFactory" />
    <property name="dataSource" ref="dataSource" />
</bean>
<tx:annotation-driven transaction-manager="transactionManager"/>
```

<h5>Problem #1: Spring Boot Auto Configuration: Can We Think Different?</h5>
<h5>问题1：Spring Boot自动配置：我们可以另辟蹊径吗？</h5>

<p>Spring Boot brings a new thought process around this.
Can we bring more intelligence into this? When a spring mvc jar is added into an application, can we auto configure some beans automatically?
How about auto-configuring a Data Source if Hibernate jar is on the classpath?
How about auto-configuring a Dispatcher Servlet if Spring MVC jar is on the classpath?
There would be provisions to override the default auto configuration.
Spring Boot looks at a) Frameworks available on the CLASSPATH b) Existing configuration for the application. Based on these, Spring Boot provides basic configuration needed to configure the application with these frameworks. This is called
Auto Configuration .</p>

<p>Spring Boot为我们带来了一个新的途径去思考该问题。
我们能让此变得更加智能吗？当一个spring mvc的jar包添加到应用中，我们可以让其自动配置一下bean吗？
当Hibernate的jar包被添加到类路径下，就自动配置数据源怎么样？
当Spring MVC的jar包被添加到类路径下，就自动配置Servlet分发器怎么样？
同样也可以有覆写默认自动配置的规定。
Spring boot关注以下两点：a）框架在CLASSPATH可用 b)存在对应用可用的配置。基于此，Spring boot在这些框架中为应用提供基础的配置。这被叫做自动配置。
</p>


<h5>Problem #2: Spring Boot Starter Projects: Built Around Well- Known Patterns</h5>
<h5>问题2：Spring Boot Starter 项目：基于公认的模式构建</h5>
<p>Let’s say we want to develop a web application.
First of all, we would need to identify the frameworks we want to use, which versions of frameworks to use and how to connect them together.
All web application have similar needs. Listed below are some of the dependencies we use in our Spring MVC Course. These include Spring MVC, Jackson Databind (for data binding), Hibernate- Validator (for server side validation using Java Validation API) and Log4j (for logging). When creating this course, we had to choose the compatible versions of all these frameworks.</p>


<p>假设现在我们要开发一个web应用。首先，我们需要确定要使用的框架，框架的版本以及如何把它们连接在一起。
所有的web应用都有类似的需求。下面列举了一些在我们的Spring MVC课程中常用的依赖。包括Spring MVC，Jackson Databind(用于数据绑定），Hibernate- Validator（用于服务端使用Java Validation API验证）以及 Log4j（用于日志）。当我们开设此课程时，我们不得不选择所有框架的兼容版本。</p>

```
 <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>4.2.2.RELEASE</version>
 </dependency>

<dependency>
<groupId>org.hibernate</groupId>
<artifactId>hibernate-validator</artifactId>
<version>5.0.2.Final</version>
</dependency>

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.3</version>
</dependency>

<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
```

<p>Here’s what the Spring Boot documentations says about starters.
   Starters are a set of convenient dependency descriptors that
   you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for database access, just include the spring-boot-starter-data-jpa dependency in your project,and you are good to go.</p>
   
<p>下面是Spring boot的文档对于starters的描述。
Starters是一个可以置于你的应用中的便捷的依赖描述器。你可以通过它得到包含Spring和相关技术的一站式服务，而不用寻找样本代码或者复制粘贴依赖描述。比如，如果你想使用Spring和JPA用于数据库访问，只要在你的项目中添加spring-boot-starter-data-jpa依赖便可以继续。</p>

<p>Let’s consider an example starter: Spring Boot Starter Web.
   If you want to develop a web application or an application to expose restful services, Spring Boot Start Web is the starter to pick. Let's create a quick project with Spring Boot Starter Web using Spring Initializr.</p>   
<p>
让我们来考察一下一个示例starter: Spring Boot Starter Web
如果你想要开发一个web应用，或者一个对外提供restful服务的应用，Spring Boot Start Web 则是一个好的开始。让我们使用 Spring Initializr来快速的创建一个 Spring Boot Starter Web项目。
</p>

<h5>Dependency for Spring Boot Starter Web</h5>
<h5>Spring Boot Starter Web的依赖</h5>

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

<p>The following screenshot shows the different dependencies that are added into our application 
Dependencies can be classified into: 
Spring: core, beans, context, aop
Web MVC: (Spring MVC)
Jackson: for JSON Binding
Validation: Hibernate Validator, Validation API Embedded Servlet Container: Tomcat
Logging: logback, slf4j
Any typical web application would use all these dependencies. Spring Boot Starter Web comes pre- packaged with these. As a developer, I would not need to worry about either these dependencies or their compatible versions.</p>

<p>下面的截图展示了添加到应用里的不同依赖。
依赖可以被分类为：
Spring: core, beans, context, aop
Web MVC: (Spring MVC)
Jackson: for JSON Binding
Validation: Hibernate Validator, Validation API Embedded Servlet Container: Tomcat
Logging: logback, slf4j
任何典型的web应用都会用到这些依赖。SpringBoot Starter Web则预先将这些打包好。作为一个开发者，我将不再担心这些依赖以及它们的兼容版本。
</p>


<h5>Spring Boot Starter Project Options</h5>
<h5>Spring Boot Starter 项目的选项</h5>

<p>As we see from Spring Boot Starter Web, starter projects help us in quickly getting started with developing specific types of applications.
spring-boot-starter-web-services: SOAP Web Services
spring-boot-starter-web: Web and RESTful applications
spring-boot-starter-test: Unit testing and Integration Testing 
spring-boot-starter-jdbc: Traditional JDBC
spring-boot-starter-hateoas: Add HATEOAS features to your services 
spring-boot-starter-security: Authentication and Authorization using Spring Security 
spring-boot-starter-data-jpa: Spring Data JPA with Hibernate 
spring-boot-starter-cache: Enabling Spring Framework’s caching support 
spring-boot-starter-data-rest: Expose Simple REST Services using Spring Data REST</p>

<p>正如我们在Spring Boot Starter Web中看到的那样，starter项目帮助我们快速的开始开发不同类型的应用
spring-boot-starter-web-services: SOAP Web 服务
spring-boot-starter-web: Web and RESTful 应用
spring-boot-starter-test: 单元测试和集成测试
spring-boot-starter-jdbc:  事务型JDBC
spring-boot-starter-hateoas: 将你的服务添加HATEOAS特性
spring-boot-starter-security: 使用Spring Security认证和授权 
spring-boot-starter-data-jpa: 使用Hibernate
spring-boot-starter-cache: 让Spring框架支持缓存
spring-boot-starter-data-rest:  使用Spring Data REST暴露简单的REST服务
</p>

<h5>Other Goals of Spring Boot</h5>
<h5>Spring Boot的其他目标</h5>

<p>There are a few starters for technical stuff as well
spring-boot-starter-actuator: To use advanced features like monitoring and tracing to your application out of the box
spring-boot-starter-undertow, spring-boot-starter-jetty, spring-boot-starter-tomcat: To pick your specific choice of Embedded Servlet Container
spring-boot-starter-logging: For Logging using logback 
spring-boot-starter-log4j2: Logging using Log4j2

Spring Boot aims to enable production ready applications in quick time.
Actuator: Enables Advanced Monitoring and Tracing of applications.
Embedded Server Integrations: Since the server is integrated into the application, I would need to have a separate application server installed on the server.
Default Error Handling</p>

<p>这里有一些starter，同样用于技术资料
spring-boot-starter-actuator: 用于高级特性，诸如监控，以及追踪你的应用
spring-boot-starter-undertow, spring-boot-starter-jetty, spring-boot-starter-tomcat:用于选择特定的内置Servlet容器
spring-boot-starter-logging: 使用logback进行日志处理 
spring-boot-starter-log4j2: 使用Log4j2进行日志处理</p>

<p>Spring Boot致力于快速的使应用进入生产状态。
执行器:使用高级监控和应用追踪
嵌入式服务器集成：由于服务是集成在应用中，我需要在服务器上安装一个不同的应用。
默认错误处理
</p>
