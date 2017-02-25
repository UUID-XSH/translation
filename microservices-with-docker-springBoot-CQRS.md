# Microservices with Docker, Spring Boot and Axon CQRS/ES
# 用Docker，Spring Boot ／ Cloud和Axon CQRS／ES来构建微服务

The pace of change in software architecture has rapidly advanced in the last few years. New approaches like DevOps, Microservices and Containerisation have become hot topics with adoption growing rapidly. In this post, I want to introduce you to a microservice project that I’ve been working on which combines two of the stand out architectural advances of the last few years: command and query responsibility separation (CQRS) and containerisation.

软件架构变化的步伐在过去几年快速演进。新的方法，如DevOps，微服务和容器化已经成为热点话题也被逐渐广泛采用。在这篇文章中，作者会介绍一个自己实践的微服务项目，包含了两个在架构层面上比较突出的点：命令和查询职责分离（CQRS）与容器化。

In this first installment, I’m going to show you just how easy it it to distribute and run a  multi-server microservice application using containers.

在第一部分，作者会演示如何轻松用容器分发和运行一个多服务的微服务应用。

In order to do this I’ve used Docker to create a suite of containers containing all the microservices required to run the demo. At the time of writing there are seven microservices  in this suite; they are:-

为了做到这一点，我使用Docker创建了一套包含所有运行演示所需的微服务容器集群。在写本文的时候，Demo包含了七个微服务：-

![Architecture](https://github.com/benwilcock/microservice-sampler/raw/master/slides/CQRS-Architecture-02.png "Architecture")

The source code for this demo is available on Github and demonstrates how to implement and integrate several of the features required for ‘cloud native’ Java including:-

这个演示的源代码在[Github](https://github.com/benwilcock/microservice-sampler).该项目同时演示了如何实施和集成多个'云原生（Java）应用’需要的特性，包括：

* Microservices with Java and Spring Boot;
* Build, Ship and Run anywhere using Docker containers;
* Command and Query Responsibility Separation (CQRS) and Event Sourcing (ES) using the Axon Framework v2, MongoDB and RabbitMQ;
* Centralised configuration（统一配置管理）, service registration（服务注册） and API Gateway（API网关） using Spring Cloud;

## How it works
## 工作原理

The microservice sample project introduced here revolves around a fictitious `Product` master data application similar to that which you’d find in most retail or manufacturing companies. Products can be added, stored, searched and retrieved from this master data using a simple RESTful service API. As changes happen, notifications are sent to interested parties using messaging.

这里介绍的微服务示例项目围绕一个虚构的‘产品’主数据应用，它会和大多数零售或制造企业得的产品应用相关。使用简单的RESTful服务API，产品可以被添加，存储，搜索和获取。当变化发生时，通知会通过消息的方式发送给相关服务。

The Product Data application is built using the CQRS architectural style. In CQRS commands like `ADD` are physically separated from queries like `VIEW (where id=1)`. Indeed in this particular example the Product domain’s codebase has been quite literally split into two separate components – a command-side microservice and a query-side microservice.

产品数据应用使用CQRS构建。在CQRS命令中如`ADD`在物理上与查询命令例如`VIEW（其中id = 1）`分离。事实上，在这个特殊的例子中，产品域的代码库被毫不夸张地分成两个独立的元件————命令侧微服务和查询侧微服务。

Like most 12 factor apps, each microservices has a single responsibility; features its own datastore; and can be deployed and scaled independently of the other. This is CQRS and microservices in their most literal interpretation. Neither CQRS or microservices have to be implemented in this way, but for the purpose of this demonstration I’ve chosen to create a very clear separation of the read and write concerns.

就像通常的12-Factor应用一样，每个微服务都有自己独立的职责，独立的数据存储，并且可以独立的进行部署和扩展，这是CQRS和微服务最直观的字面解释。CQRS或者微服务并非一定要如此实现，在这里我选择将读写逻辑关注点分离只是为了更好的示范。

The logical architecture looks like this:

整体的逻辑结构如下图：

![CQRS Architecture Overview](http://fmn.rrfmn.com/fmn070/20170225/1200/large_v4Va_ab56000a34421e7f.jpg)

Both the command-side and the query-side microservices have been developed using the Spring Boot framework. All communication between the command and query microservices is purely `event-driven`. The events are passed between the microservice components using RabbitMQ messaging. Messaging provides a scalable means of passing events between processes, microservices, legacy systems and other parties in a loosely coupled fashion.

命令侧和查询侧的微服务都是采用Spring Boot框架搭建的。命令和查询部分之间的所有通信都是单纯通过`event-driven（事件驱动）`，事件在微服务组件之间的传递采用RabbitMQ消息。消息传递提供了以松散耦合的方式在进程，微服务，传统系统和其他部分之间传递事件的可扩展手段。

>Notice how neither of the services shares it’s database with the other. This is important because of the high degree of autonomy it affords each service, which in turn helps the individual services to scale independently of the others in the system. For more on CQRS architecture, check out my Slideshare on CQRS Microservices which the slide above is taken from.

>请注意，任何服务都不会与其他服务共享数据库。这是非常重要的，因为它使得每个服务具有高度的自主性，这反过来保证了个体服务能够独立于系统中的其他服务进行扩展。有关CQRS架构的更多信息，请参阅上面幻灯片中Slideshare上的CQRS微服务。

The high level of autonomy and isolation present in the CQRS architectural patterns presents us with an interesting problem – how should we distribute and run components that are so loosely coupled? In my view, containerisation provides the best mechanism and with Docker being so widely used, it’s format has become the defacto standard for container images, with most popular cloud platforms offering direct support. It’s also very easy to use, which definitely helps.

CQRS架构模式中存在的高度自主和隔离给我们带来了一个有趣的问题————我们应该如何分配和运行如此松散耦合的组件？在我看来，对于这个问题容器化提供了最好的机制，并且随着Docker被越来越广泛的应用，它的格式已经成为容器镜像的标准，并且那些最流行的云平台也提供了直接的支持，再加上它确实非常方便使用，因此必定有所帮助。

The Command-side Microservice

命令侧微服务

Commands are “actions which change state“. The command-side microservice contains all the domain logic and business rules. Commands are used to add new Products, or to change their state. The execution of these commands on a particular Product results in `Events` being generated which are persisted by the Axon framework into MongoDB and propagated out to other processes (as many processes as you like) via RabbitMQ messaging.

所谓命令就是“改变状态的动作”。命令侧微服务包含所有逻辑部分和业务规则。命令用于添加新内容或更改其状态。在特定内容上执行这些命令将导致生成“事件”，这些事件由Axon框架持久保存到MongoDB中，并通过RabbitMQ消息传播到其他进程（尽可能多的进程）。

In event-sourcing, events are the sole record of state for the system. They are used by the system to describe and re-build the current state of any entity on demand (by replaying it’s past events one at a time until all previous events have been re-applied). This sounds slow, but actually because events are simple, it’s really fast and can be tuned further using rollups called ‘snapshots’.

在事件源中，事件是系统唯一的状态记录，系统使用它们来描述和重建任何实体的当前状态（依次重放它过去的每个事件，直到所有事件被重新应用一遍）。这听起来很慢，但实际上因为事件很简单因此执行的很快，并且可以使用`snapshots（快照）`进一步实现回滚。

In Domain Driven Design (DDD) the entity is often referred to as an `Aggregate` or an `AggregateRoot.`

在域驱动设计（DDD）中，实体通常被称为`Aggregate（聚合）`或`AggregateRoot（聚合轮询）`。

The Query-side Microservice

查询侧微服务

The query-side microservice acts as an event-listener and a view. It listens for the `Events` being emitted by the command-side and processes them into whatever shape makes the most sense (for example a tabular view).

In this particular example, the query-side simply builds and maintains a ‘materialised view’ or ‘projection’ which holds the latest state of the individual Products (in terms of their id and their description and whether they are saleable or not). The query-side can be replicated many times for scalability and the messages held by the RabbitMQ queues can be made to be durable, so they can even temporarily store messages on behalf of the query-side if it goes down.

The command-side and the query-side both have REST API’s which can be used to access their capabilities.

For more information, see the Axon documentation which describes how Axon brings CQRS and Event Sourcing to your Java apps as well as lots of detail on how it’s configured and used.

Running the Demo

Running the demo code is easy, but you’ll need to have the following software installed on your machine first. For reference I’m using Ubuntu 16.04 as my OS, but I have also tested the app on the new Docker for Windows Beta successfully.

Docker (I’m using v1.8.2)
Docker-compose (I’m using v1.7.1)
If you have both of these, you can run the demo by following the process outlined below.

If you have either MongoDB or RabbitMQ already, please shut down those services before continuing in order to avoid port clashes.
Step 1: Get the Docker-compose configuration file

In a new empty folder, at the terminal execute the following command to download the latest docker-compose configuration file for this demo.

1
$ wget https://raw.githubusercontent.com/benwilcock/microservice-sampler/master/docker-compose.yml
Try not to change the file’s name – Docker defaults to looking for a file called ‘docker-compose.yml’. If you do change the name, use the -f switch in the following step.
Step 2: Start the Microservices

Because we’re using docker-compose, starting the microservices is now simply a case of executing the following command.

1
$ docker-compose up
You’ll see lots of downloading and logging output in the terminal window as the docker images are downloaded and run.

There are seven docker images in total, they are mongodb, rabbitmq, config-service, discovery-service, gateway-service, product-cmd-side, & product-qry-side.
If you want to see which docker instances are running (and also get their local IP address), open a separate terminal window and execute the following command:-

1
$ docker ps
Once the instances are up and running (this can take some time at first) you can have a look around immediately using your browser. You should be able to access:-

The Rabbit Management Console on port `15672`
The Eureka Discovery Server Console on port `8761`
The Configuration Server mappings on port `8888`
The API Gateway Routes on port ‘8080’
Step 3: Working with Products

So far so good. Now we want to test the addition of products.

In this manual system test we’ll issue an `add` command to the command-side REST API.

When the command-side has processed the command a ‘ProductAddedEvent‘ is raised, stored in MongoDB, and forwarded to the query-side via RabbitMQ. The query-side then processes this event and adds a record for the product to it’s materialised-view (actually a H2 in-memory database for this simple demo). Once the event has been processed we can use the query-side microservice to lookup information regarding the new product that’s been added. As you perform these tasks, you should observe some logging output in the docker-compose terminal window.
Step 3.1: Add A New Product

To perform test this we first need to open a second terminal window from where we can issue some CURL commands without stopping the docker composed instances we have running in the first window.

For the purposes of this test, we’ll add an MP3 product to our product catalogue with the name ‘Everything is Awesome’. To do this we can use the command-side REST API and issue it with a POST request as follows…

1
$ curl -X POST -v --header "Content-Type: application/json" --header "Accept: */*" "http://localhost:8080/commands/products/add/01?name=Everything%20Is%20Awesome"
If you don’t have ‘CURL’ available to you, you can use your favourite REST API testing tool (e.g. Postman, SoapUI, RESTeasy, etc).

If you’re using the public beta of Docker for Mac or Windows (highly recommended), you will need to swap ‘localhost’ for the IP address shown when you ran docker ps at the terminal window.
You should see something similar to the following response.

1
2
3
4
5
6
7
8
9
10
11
12
* Trying 127.0.0.1...
* Connected to localhost (127.0.0.1) port 8080(#0)
> POST /commands/products/add/01?name=Everything%20Is%20Awesome HTTP/1.1
> Host: localhost:9000
> User-Agent: curl/7.47.0
> Content-Type: application/json
> Accept: */*$ http://localhost:8080/commands/products/01
< HTTP/1.1 201 Created
< Date: Thu, 02 Jun 2016 13:37:07 GMTThis
< X-Application-Context: product-command-side:9000
< Content-Length: 0
< Server: Jetty(9.2.16.v20160414)
The response code should be `HTTP/1.1 201 Created.` This means that the MP3 product “Everything is Awesome” has been added to the command-side event-sourced repository successfully.

Step 3.2: Query for the new Product

Now lets check that we can view the product that we just added. To do this we issue a simple ‘GET’ request.

1
$ curl http://localhost:8080/queries/products/1
You should see the following output. This shows that the query-side microservice has a record for our newly added MP3 product. The product is listed as non-saleable (saleable = false).

1
2
3
4
5
6
7
8
9
10
11
12
{
  name: "Everything Is Awesome",
  saleable: false,
  _links: {
    self: {
    href: "http://localhost:8080/queries/products/1"
    },
  product: {
    href: "http://localhost:8080/queries/products/1"
    }
  }
}
That’s it! Go ahead and repeat the test to add some more products if you like, just be careful not to try to reuse the same product ID when you POST or you’ll see an error.

If you’re familiar with MongoDB you can inspect the database to see all the events that you’ve created. Similarly if you know your way around the RabbitMQ Management Console you can see the messages as they flow between the command-side and query-side microservices.

About the Author

Ben Wilcock is a freelance Software Architect and Tech Lead with a passion for microservices, cloud and mobile applications. Ben has helped several FTSE 100 companies become more responsive, innovate, and agile. Ben is also a respected technology blogger who’s articles have featured in Java Code Geeks, InfoQ, Android Weekly and more. You can contact him on LinkedIn, Twitter and Github.