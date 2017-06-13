 # Logging Sins in Your Java Applications

Everyone should know good logging practices. Here are nine pitfalls to avoid when logging, ranging from handling sensitive information to not logging excessively.
 
每个人都应该掌握记日志的最佳实践。本文介绍了在记日志过程中应该避免的九个陷阱，从处理敏感信息到不要过度的去打日志。

Logging runtime information in your Java application is critically useful for understanding the behavior of any app, especially in cases when you encounter unexpected scenarios, errors or just need to track certain application events.

在Java应用程序中记录运行时信息对于了解任何应用程序的行为非常有用，特别是在遇到异常情况，错误或只需跟踪某些应用程序事件的情况下。

In a real-world production environment, you usually don’t have the luxury of debugging. And so, logging files can be the only thing you have to go off of when attempting to diagnose an issue that’s not easy to reproduce.

在现实世界的生产环境中，您通常没有足够的条件去调试。 因此，当尝试诊断不容易复现的问题时，日志文件可能是你唯一的救命稻草。

Done properly, log files can also save you a lot of time by providing clues into the cause of the problem, and into the state of the system at the time it happened. Also, logging can be useful for auditing purposes, gathering statistics, extracting business intelligence and a variety of other tasks.

理想的日志文件可以为问题提供线索，从而为你解决大量的时间，并使系统进入及时进入期望状态。

Overall, logging is certainly a foundational practice that provides significant benefits during the lifetime of the application – so it can be tempting to start recording as much log data as you can.

总之，日志记录的确是一个基础实践，在应用的整个生命周期提供了显著的优势 - 所以你可以尝试着开始去尽可能多的记日志。

However, improper use of logging can have significant drawbacks as well.

然而，不当的日志记录同样会导致重大缺陷。

In the following sections, we’ll take a look at some of the most common and most detrimental practices that you can run into when making use of logging in an application.

在下文中，我们将介绍关于在应用中记日志时的一些最常见和最有害的做法。

All examples and configuration are using the popular log4j 2 library. Logback is another great option, also well-supported by Stackify.

所有的示例和配置都使用比较流行的log4j 2库，Logback是另外一个很好的选择，也被Stackify很好的支持。

9 Java Logging Problems and How to Avoid Them

## 9个java日志问题以及怎么去避免它们

## 1. Logging Sensitive Information 记录敏感信息

To start with, probably the most damaging logging practice brought on by the “log as much as possible just in case” approach is displaying sensitive information in the logs.

首先，"尽可能的去打日志"这个行为带来的最大危害是在日志文件中展示敏感信息。

Most applications handle data that should remain private, such as user credentials or financial information. The danger of having this type of information logged into a plain text file is clear – log files will very likely be processed by multiple, unsecured systems.

大多数应用都会处理隐私数据，诸如用户的身份和财务信息，因此把这些信息记录在纯文本文件中的危害显而易见-日志文件很可能会被多个、不安全的系统处理。

What’s more, logging some categories of data, such as financial information, is also heavily regulated and can have serious legal implications.

此外，记录诸如财务这种类型的信息也会得到严格监管，因为可能会造成严重的法律影响。

The best way to avoid this is simply to make sure you never log this kind of sensitive information.

解决这些问题的最好办法是确保你从来不会将此类敏感信息记录到日志文件里。

There are alternatives, such as encrypting the log files, but that generally makes these files a lot less usable overall, which is not ideal.

当然还有一些其他选择，比如加密日志文件，但是这并不是理想方案，因为会降低日志文件的使用率。

Before we move on, here’s a more comprehensive list of the types of information that you need to be very careful logging.

在我们继续之前，这里有一个更全面的信息类型列表(https://www.owasp.org/index.php/Logging_Cheat_Sheet#Data_to_exclude)，您需要非常小心地记录这些信息。

### Logging Plain User Input 明文记录用户输入

Another common security issue in Java applications is JVM Log Forging.

在JAVA应用中，另外一个常见的安全问题是JVM日志伪造。

Simply put, log forging can happen when data from an external source like user input or another untrusted source is written directly to the logs. A malicious attacker can enter input that simulates a log entry such as “\n\nweb – 2017-04-12 17:47:08,957 [main] INFO Amount reversed successfully” which can result in corrupted log data.

简单来说，当外部来源（如用户输入或其他不可信源）的数据直接写入日志时，可能会发生日志伪造。 恶意攻击者可以输入模拟日志条目的输入，例如“\ n \ nweb - 2017-04-12 17：47：08,957 [main] INFO Amount successfully successfully successfully”，这可能导致日志数据损坏。

There are various ways to handle this kind of vulnerability:

有多种方法来处理这种漏洞

don’t log any user input – not always possible, since the user data can be critical for getting to the root cause of some problems

不记录任何用户输入 - 并不总是可行，因为用户数据可能对于出现某些问题的根本原因至关重要

use validation before logging – this solution can impact performance, as well as forego logging important information

在记录之前使用验证 - 此解决方案可能会影响性能，以及放弃记录重要信息

log to a database – more secure but costly regarding performance, and can introduce another vulnerability – SQL injection

将日志记入登录数据库 - 性能更安全但更昂贵，并可能引入另一个漏洞 - SQL注入

use a tool like the Enterprise Security API from OWASP

使用来自OWASP的Enterprise Security API等工具

Using ESAPI is definitely a good way to go; this open-source security library from OWASP can encode data before writing it to the logs:

使用ESAPI绝对是一个很好的方法; OWASP的这个开源安全库可以在将数据写入日志之前进行编码：

message = message.replace( '\n' ,  '_' ).replace( '\r' , '_' )
  .replace( '\t' , '_' );
message = ESAPI.encoder().encodeForHTML( message );

## 2. Excessive Logging 滥记

Another practice to be avoided is logging too much information. This can happen in an attempt to capture all potentially relevant data.

要避免的另一个做法是记录太多的信息。这可能发生在试图捕获所有潜在的相关数据。

One possible and very real issue with this approach is decreased performance. However, with the evolution of logging libraries, you now have the tools to make this less of a concern.

这种方法的一个可能和非常实际的问题是性能下降。 然而，随着日志库的发展，您现在可以使用这些工具来减少这个问题。

As an example of improved performance, the 2.x version of log4j uses asynchronous logging, which means executing I/O operations in a separate thread.

作为改进性能的例子，log4j的2.x版本使用异步日志记录，这意味着在单独的线程中执行I / O操作。

Too many log messages can also lead to difficulty in reading a log file and identifying the relevant information when a problem does occur.

当发生问题时，太多的日志信息也可能使得读取日志文件和识别相关信息会很困难。

One way to reduce the number of log lines of code is by logging important information across cross-cutting concerns in the system.

减少代码日志行数量的一种方法是将记录系统交叉处的重要信息。

For example, if you want to log the start and end of particular methods, you can add an Aspect that will do this for every method that has a specified custom annotation:

例如，如果要记录特定方法的开始和结束，您可以为具有指定自定义的每注解的方法添加一个Aspect：

```
@Aspect
public class MyLogger {
    private static final Logger logger = LogManager.getLogger(MyLogger.class);
    @Around("execution(* *(..)) && @annotation(LogMethod)")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        logger.info("Starting method execution: " + joinPoint.getSignature().getName() + 
          " in class:"+joinPoint.getSignature().getDeclaringTypeName());
        Object result = joinPoint.proceed();
        logger.info("Exiting method execution: " + joinPoint.getSignature().getName() + 
          " in class:"+joinPoint.getSignature().getDeclaringTypeName());
        return result;
    }
}
```

With the help of the custom aspect, we can now be very selective and pick the exact areas of the application where we actually need that information in the logs. And, as a result, we can significantly decrease the overall logging footprint of the system.

在自定义切面的帮助下，我们现在可以非常有选择性针对应用的确定区域记录下我们需要的信息。进而我们可以显著的减少系统日主输入的总体数量。

## 3. Cryptic Log Messages 加密日志消息

When parsing log files, encountering a line that doesn’t provide sufficient information can be frustrating. A common pitfall is the lack of specificity or context in log messages.

当解析日志文件时，遇到不能提供足够信息的行可能会令人沮丧。 常见的缺陷是日志消息中缺乏特异性或上下文。

To illustrate the problem, let’s have a look at a log message lacking specificity:

为了说明问题，我们来看看一个缺乏特异性的日志消息：

```
Operation failed.
```
Instead, you can add more specific and identifiable information:

相反，您可以添加更具体和可识别的信息：

```
File upload picture.jpg failed.
```

Always keep in mind that your logs will most certainly be read by a different developer or system administrator, and they need to understand what has happened in the application.

请牢记，您的日志肯定会被不同的开发人员或系统管理员阅读，他们需要了解应用程序中发生了什么。

A good way to add context in log messages is by including the timestamp, log level, thread name and fully qualified class name of the event. This way, you can more easily identify when and where specific events occur at runtime.

在日志消息中添加上下文的一个好方法是通过包括事件的时间戳，日志级别，线程名和完全限定的类名。 这样，您可以更轻松地识别特定事件在运行时发生的时间和位置。

To add this information when using log4j 2, you can configure a Pattern Layout with the options %d for the date, %p for log level, %t for thread name and %c for class name:

要在使用log4j 2时添加此信息，您可以配置模式布局，其中包含日期的％d，日志级别为％p，线程名称为％t，类名为％c：

```
<PatternLayout>
    <Pattern>%d [%t] %p %c - %m%n</Pattern>
</PatternLayout>
```

A log message using the above layout will look like this:

使用上述布局的日志消息将如下所示：

```
2017-05-11 22:51:43,223 [main] INFO com.stackify.service.MyService - User info updated
```
### 4. Using a Single Log File 使用单个日志文件

The downside of only using one log file for the application is, that this will, over time, become quite large and difficult to work with.

仅对应用程序使用一个日志文件的缺点是，随着时间的推移，这将会变得相当大，难以处理。

A good practice for quickly finding relevant information is to create a new log file each day, with the date as part of the file name.

快速查找相关信息的良好做法是每天创建一个新的日志文件，日期作为文件名的一部分。

Let’s take a look at an example of how to create a log file with the name equal to the current date if using the log4j2 library:

我们来看看如何使用log4j2库，如何创建名称等于当前日期的日志文件的示例：

```
SimpleLayout layout = new SimpleLayout();
FileAppender appender = new FileAppender(layout, LocalDate.now().toString(), false);
logger.addAppender(appender);
```

The same library also provides the option to configure a Rolling File Appender that will create new log files at certain time intervals:

同一个库还提供了配置滚动文件附加程序的选项，它将以特定的时间间隔创建新的日志文件：

```
<RollingFile name="RollingFile" fileName="logs/app.log"
  filePattern="logs/${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
    <PatternLayout>
        <Pattern>%d [%t] %p %c - %m%n</Pattern>
    </PatternLayout>
    <Policies>
        <TimeBasedTriggeringPolicy />
        <SizeBasedTriggeringPolicy size="250 MB"/>
    </Policies>
    <DefaultRolloverStrategy max="20"/>
</RollingFile>
```

The configuration above will result in a one or more files created for each day up to 250 MB per file with the current date as the file name, placed in folders with names of the form year-month.

上述配置将导致每个文件创建一个或多个文件，每个文件最多250 MB，当前日期为文件名，放置在名称为年月份的文件夹中。

### 5. Choosing Incorrect Log Levels 选择不正确的日志级别

Choosing an inadequate log level will lead to either missing significant events or being flooded with a lot of less important data.

选择不正确的日志级别将导致丢失重大事件或被大量不太重要的数据淹没。

Simply put, choosing the right log level for the different logs in your system is one of the core things you need to get right to have a good experience understanding your logs.

简单来说，为您的系统中的不同日志选择正确的日志级别是您理解您的日志所需要的核心事项之一。

Most logging frameworks have a set of levels similar to FATAL, ERROR, WARN, INFO, DEBUG, TRACE, ordered from highest to lowest.

大多数日志记录框架具有类似于FATAL，ERROR，WARN，INFO，DEBUG，TRACE的级别，从最高到最低排序。


Available log levels 可用日志级别

Let’s take a look at each of these and the type of log messages they should contain based on severity:
我们来看看这些以及基于严重性应该包含的日志消息的类型：

FATAL should be reserved for errors that cause the application to crash or fail to start (ex: JVM out of memory) 应该保留用于导致应用程序崩溃或无法启动的错误（例如：JVM内存不足）
ERROR should contain technical issues that need to be resolved for proper functioning of the system (ex: couldn’t connect to database) 应包含需要解决的系统正常运行的技术问题（例如：无法连接到数据库）
WARN is best used for temporary problems or unexpected behavior that does not significantly hamper the functioning of the application (ex:  failed user login) 最适合用于临时问题或意外行为，这并不会严重妨碍应用程序的运行（例如：用户登录失败）
INFO should contain messages that describe what is happening in the application (ex: user registered, order placed)  应包含描述应用程序中发生的情况的消息（例如：用户注册，订单放置）
DEBUG is intended for messages that could be useful in debugging an issue (ex: method execution started) 旨在用于在调试问题中有用的消息（例如：方法执行开始）
TRACE is similar to DEBUG but contains more detailed events (ex: data model updated) 类似于DEBUG，但包含更详细的事件（例如：更新数据模型）

Controlling Log Levels 控制日志级别

The log level of a message is set when it is written:
消息的日志级别在写入时设置：

```
logger.info("Order ID:" + order.getId() + " placed.");
```

Logging APIs usually allow you to set the level up to which you want to see messages. What this means is that, if you set the log level for the application or certain classes to INFO, for example, you will only see messages at the levels FATAL, ERROR, WARN and INFO, while DEBUG and TRACE messages will not be included.

日志API通常允许您设置要查看消息的级别。 这意味着，如果您将应用程序或某些类的日志级别设置为INFO，例如，您将只会看到级别为FATAL，ERROR，WARN和INFO的消息，而不会包括DEBUG和TRACE消息。

This is helpful as you would usually show DEBUG or lower messages in development, but not in production.

这是有用的，因为通常会在开发中显示DEBUG或更低的消息，生产中反之。

Here is how you can set the log level for a class, package or entire application in log4j 2, using a log4j2.properties file:

以下是使用log4j2.properties文件在log4j 2中设置类，包或整个应用程序的日志级别：

```
loggers=classLogger,packageLogger
logger.classLogger.name=com.stackify.service.MyService
logger.classLogger.level=info
logger.packageLogger.name=com.stackify.config
logger.packageLogger.level=debug
rootLogger.level=debug
```

To display the log level in the message, you can add the %p option in the log4j2PatternLayout.

要在消息中显示日志级别，可以在log4j2PatternLayout中添加％p选项。

Before we move on, keep in mind that, if none of the existing levels are appropriate for your application needs, you have the possibility of defining a custom log level as well.

在我们继续介绍之前，请记住，如果现有的级别没有适合您的应用程序需求，您也可以定义自定义日志级别。

### 6. Tracking A Single Operation Across Multiple Systems and Logs 跨系统跟踪记录单个操作

In distributed systems with multiple, independently deployed services that work together to process incoming requests, tracking a single request across all of these systems can be difficult.

在具有共同处理传入请求的多个独立部署的服务的分布式系统中，跨越所有这些系统跟踪单个请求可能是困难的。

A single request will very likely hit multiple of these services, and if a problem does occur, we’ll need to corroborate all of the individual logs of these systems to get the full picture of what happened.

单个请求很可能会影响到这些服务中的多个，如果出现问题，我们需要确认这些系统的所有日志，以获得所发生的全部信息。

To address this kind of architecture, we now have a new generation of logging helper tools in the ecosystem, such as Zipkin and Spring Cloud Sleuth.

为了解决这种架构带来的问题，我们现在在生态系统中拥有新一代的日志助手工具，如Zipkin和Spring Cloud Sleuth。

Zipkin traces requests across microservice architectures to help you identify which application is causing the issue. It also comes with a helpful UI where you can filter traces based on application, length of the trace or timestamp.

Zipkin跟踪微服务体系结构中的请求，以帮助您识别哪个应用程序导致问题。 它还带有一个有用的UI，您可以根据应用程序、跟踪的长度或时间戳进行过滤。

And the Spring Cloud Sleuth project works by adding a unique 64-bit ID to each trace; a web request, for instance, can constitute a trace. This way, the request can be identified across multiple services.

Spring Cloud Sleuth项目通过在每个跟踪中添加唯一的64位ID来实现; 例如，Web请求可以构成一个跟踪。 这样，可以在多个服务之间识别请求。

These tools address the limitations of the core libraries and you navigate the new realities of the more distributed style of architectures.

这些工具解决了核心库的局限性，您可以浏览更多分布式体系结构的新现实。

### 7. Not Logging with JSON 不使用JSON进行日志记录

While logging in a plaintext format is very common, the advent of log storage and data analysis systems has shifted that towards JSON.

虽然明文格式在日志文件中非常常见，日志存储和数据分析系统的出现已经转向了JSON。

JSON as the primary application log format has the advantage of being just as readable as plain text, while also being a lot easier to parse by automated processing tools.

作为主应用程序日志格式的JSON具有与纯文本一样可读的优点，同时也易于通过自动处理工具进行解析。

For example, Log4j 2 offers the JSONLayout for this exact purpose:

例如，Log4j 2为此确切目的提供了JSONLayout：

```
<JSONLayout complete="true" compact="false"/>
```

This will produce a well-formed JSON document:

这将产生一个格式良好的JSON文档：

```
[
    {
        "logger":"com.stackify.service.MyService",
        "timestamp":"1376681196470",
        "level":"INFO",
        "threadId":1,
        "thread":"main",
        "threadPriority":1,
        "message":"Order ID:1234 placed."
    },
    ...
]

```


As JSON, the log data will be semantically richer when it’s processed by a log management system such as Retrace – which will immediately enable its powerful structured/semantic logging capabilities.

作为JSON，日志数据在诸如Retrace之类的日志管理系统处理时将会语义上更丰富，这将使强大的结构化/语义记录功能成为可能。

### 8. Logging Impact on Performance 记录对性能的影响


Finally, let’s consider an issue which is unavoidable when adding logging to an application: the impact on performance.

最后，让我们考虑一下在应用中记录日志所导致的一个不可避免的问题：对性能的影响。

A small drop in performance is to be expected. However, it’s important to track this so that you can minimize it and not slow down the system.

虽然预期性能下降很小。 但是，跟踪这一点很重要，以便您可以将其最小化，而不会使系统减速。

Some performance-related aspects to consider when choosing a logging API are:

选择日志记录API时要考虑的一些与性能有关的方面有：

File I/O operations using a buffer – this is critical as file I/O is an expensive operation

使用缓冲区的文件I / O操作 - 这是至关重要的，因为文件I / O是一项昂贵的操作

Asynchronous logging – this should be considered so that logging doesn’t block other application processes

异步日志记录 - 这应该被考虑，以便日志记录不阻止其他应用程序进程

Logging response time – the time it takes to write a log entry and return

记录响应时间 - 编写日志条目并返回所需的时间

Number of threads used for logging  用于记录的线程数

Log level filtering – this is done to verify if the log level corresponding to a message is enabled, and can be done by traversing the hierarchy or having the Logger point directly to the Logger configuration; the latter approach is preferable regarding performance

日志级别过滤 - 这是为了验证是否启用与消息对应的日志级别，并且可以通过遍历层次结构或使Logger直接指向Logger配置来完成; 后一种方法对于性能是优选的

Of course, if you need to keep the choice open and the system flexible, you can always use a higher level abstraction such as slf4j.

当然，如果您需要保持选择的开放性和系统灵活性，您可以随时使用更高级别的抽象，如slf4j。

Before we move one, here are just a few steps you can take to improve logging performance of your system:

在我们继续之前，您可以采取以下几个步骤来提高系统的日志记录性能：

tune the log level of the application for verbose packages  调整应用程序的日志级别以进行详细包
avoid logging source location information at runtime, as looking up the current thread, file, a method is a costly operation
避免在运行时记录源位置信息，因为查找当前线程，文件，方法是一项昂贵的操作
avoid logging errors with long stack traces 避免使用长堆栈跟踪记录错误
check if a specific log level is enabled before writing a message with that level – this way the message won’t be constructed if it’s not needed
在写入具有该级别的消息之前检查是否启用了特定的日志级别 - 这样，如果不需要，消息将不被构造
review the logs before moving to production to check if any logging can be removed
在移动到生产之前检查日志，以检查是否可以删除任何日志

### 9. Honorable Mentions 荣誉奖


Before we wrap up, let’s have a look at one final practice that you should avoid – and that is using standard out instead of logging.

在我们总结之前，让我们来看看你应该避免的一个最后的做法 - 那就是使用标准输出而不是日志记录。

While System.out() can be a quick way go start very early on in the development cycle, it’s definitely not a good practice to follow after that point.

虽然System.out（）可以是一个很快的方式，尤其是在开发初期。但是过后这绝对不是一个好习惯。

Besides the fact that you lose all the powerful features of a dedicated logging API, this primary downside here is simply the fact that the logging data won’t be persisted anywhere.

除了失去专用日志记录API的所有强大功能之外，这个方法的主要缺点是日志数据在任何地方都不会得到保留。

Finally, another honorable mention is a practice that can make reading and analyzing log data a lot easier – standardized log messages. Simply put, similar events should have similar messages in the log.

最后，另一个值得一提的是，可以使阅读和分析日志数据更容易 - 标准化日志消息。简单来说，类似的事件应该在日志中有类似的消息。

If you need to search for all instances of that particular event or extract meaningful insights out of your log data, standard log messages are quite important.

如果您需要搜索该特定事件的所有实例或从日志数据中提取有意义的信息，则标准日志消息是非常重要的。

For example, if an upload operation fails – having these different messages in the log would be confusing:

例如，如果上传操作失败 - 在日志中使用这些不同的消息会令人困惑：

```
Could not upload file picture.jpg
File upload picture.jpg failed.
```

Instead, whenever the file upload fails, you should consistently use one of these messages to log the failure.

相反，只要文件上传失败，您应该始终使用其中一个消息来记录该失败。


### Conclusion结论

The use of logging has become ubiquitous in application development, due to the highly useful and actionable insights it brings into the runtime of the system.

在应用程序开发中使用日志记录已经变得无所不在，因为为系统的运行时带来了非常有用和可行的能力。

However, to get the most out of your log data, it’s important to go beyond the basics, develop a culture of logging and understand the finer points of operating with this data at scale and in production.

然而，为了充分利用日志数据，重要的是超越基础，开发出记录日志的文化，并了解在规模和生产中使用这些数据的更精细的操作点。

