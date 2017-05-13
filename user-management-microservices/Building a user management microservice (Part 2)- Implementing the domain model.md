# Building a user management microservice (Part 2): Implementing the domain model

# 建立一个用户管理的微服务（第2部分）：实现领域模型

[原文链接](https://springuni.com/user-management-microservice-part-2/)

In my previous post I defined the requirements of a user management microservice and designed the initial domain model of it. Getting lots of positive energy from the community and many valuable comments on Reddit ensured me, that it’s worth going on with the project. In this second part, I’ll detail how the domain model got implemented and what decisions were made behind the code.

在以前的帖子中，作者定义了用户管理微服务的要求，并设计了它的初始领域模型。在得到许多来自社区和Reddit上许多有价值的正能量的评论，这个项目值得继续推进。在在第二部分，作者将详细介绍如何实现领域模型，在代码之外做了哪些决定。

![2-1](2-1.png)


## Using Domain-Driven Design
## 使用领域驱动设计

In the first part, I mentioned that Domain-Driven Design principles will be used, which implies that the model cannot depend on any framework or infrastructure class. I had created applications so many times with cluttering the domain model with framework specific annotations (eg. JPA or Hibernate), that it felt completely alien to work with bare Java POJO’s again. The only library the domain model uses is Lombok in order to reduce the verbosity normal getters and setters would have caused.

在第一部分中，作者提到了将使用领域驱动设计原则，这意味着，该模型可以不依赖于任何框架或基础设施类。在多次应用实现过程中，作者把领域模型和框架的具体注释（如JPA或Hibernate）混在一起，就如同和Java POJO一起工作（贫血模型）。在设计领域模型中，唯一使用的库是Lombok，用于减少定义的getter和setter方法以避免冗余。

When designing a model with DDD, the first step is the characterization of classes. In Eric Evan’s book  Part II. focuses on the Building Blocks of Model-Driven Design . With that in mind, we classify our model classes into the following categories.

当设计DDD的模型，第一步是对类进行分类。在埃里克·埃文斯书中的第二部分专注于模型驱动设计的构建模块。考虑到这一点，我们我们的模型类分为以下几类。

## Entities

## 实体类

Entities have got a clear identify and lifecycle which needs to be managed. From that perspective a User is certainly an entity.

实体有明确的标识和生命周期需要被管理。从这个角度来看，用户肯定是一个实体。

ConfirmationToken [4] however is a borderline case, because logically it doesn’t exist without the context of a user, on the other hand, it can be identified by the token’s value and it has got its own lifecycle.

ConfirmationToken [4]就是一个边缘的例子，因为在没有用户上下文的情况下，逻辑上它就不存在，而另一方面，它可以通过令牌的值来标识并且它有自己的生命周期。

The same approach can be applied to Session, which could also be a value object, due to its immutable nature, yet it still has an identity and a lifecycle (sessions expire).

同样的方法也适用于Session，这也可能是一个值对象，由于其不可改变的性质，但它仍然有一个ID和一个生命周期（会话过期）。

## Value Objects
## 值对象

In contrast to entities, value objects don’t have a clear identity, that is, they are just grouping a set of attributes and if these attributes are the same as the attributes of another value object of the same type, then we can treat them the same.

相对于实体类，值对象没有一个明确的ID，那就是，他们只是将一系列属性组合，并且，如果这些属性和另外一个相同类型的值对象的属性相同，那么我们就可以认为这两个值对象是相同的。

When designing the domain model, value objects provided a convenient way to describe set of attributes, which carry a certain piece of information. AddressData, AuditData, ContactData and Password became value objects for this reason.

当设计领域模型，值对象提供了一种方便的方式来描述携带有一定的信息片段属性的集合。AddressData，AuditData，ContactData和Password因此可以认为是值对象。

Although it would have been impractical to implement all of them immutable, as some attributes of them may be changed changed individually, Password was a good candidate for that. When we create an instance of Password, it is created only once with its salt and hash. Upon changing the password, an entirely new instance is created with a new salt and hash.

虽然将所有这些属性实现为不可改变的是不切实际的，他们的某些属性可以单独被修改，Password是一个很好的例子。当我们创建Passwrod的实例，它的盐和哈希创建只有一次。在改变密码时，一个全新的实例与新的盐和散列将会被创建。

## Aggregates
## 聚合

Aggregates represent a set of objects which are bound together and accessed through a so called root aggregate.

聚合代表一组结合在一起，并通过访问所谓的聚合根的对象。

We’ve got two aggregates here user and session. The former contains all the entities and value objects related to users and latter contains only a single entity Session.

这儿有两个聚合对象：用户和会话。前者包含了所有与用户相关的实体和值对象，而后者只包含一个单一的实体Session。

Obviously, the aggregate root of user is the User entity. Through an instance of a User entity we manage confirmation tokens, user events and the user’s password.

显然，用户聚合根是用户实体。通过一个实例用户实体，我们可以管理确认令牌，用户事件和用户的密码。


Aggregate Session became a standalone entity – in spite of being tied to a user’s context – partly due to its disposable nature and partly because we don’t know who the user is when we look a session up. Sessions are created once and they either expire or they get removed on demand.


聚合Session成为一个独立的实体-尽管被捆绑到一个用户的上下文-部分原因是由于其一次性性质，部分是因为当我们查找一个会话时我们不知道用户是谁。Session被创建之后，要么过期，要么按需删除。

## Domain Events
## 领域事件

Domain events are emitted when such an event occurs which needs to be handled by another component of the system.

当需要由系统的另外组件处理的事件发生时，领域事件就会被触发。

The user management app has got a single domain event, which is UserEvent and it may refer to one of the following events.

用户管理应用程序有一个领域事件，这是UserEvent，它有以下类型：

* DELETED
* EMAIL_CHANGED
* EMAIL_CHANGE_REQUESTED
* EMAIL_CONFIRMED
* PASSWORD_CHANGED
* PASSWORD_RESET_CONFIRMED
* PASSWORD_RESET_REQUESTED
* SCREEN_NAME_CHANGED
* SIGNIN_SUCCEEDED
* SIGNIN_FAILED
* SIGNUP_REQUESTED

## Services
## 服务

Services carry the business logic which operate on a set of domain model’s classes. In this application UserService manages the life-cycle of Users and also emits the appropriate UserEvents. SessionService is for creating and destroying user sessions.

服务包含了能够操作一组领域模型的类的业务逻辑。在本应用中，UserService管理用户的生命周期，并发出合适的UserEvent。SessionService是用于创建和销毁用户会话。

## Repositories

## 存储库

Repositories are meant to represent a conceptual collection of objects of a entity, however sometimes they’re just used as data access objects. There are two approaches for implementing repositories. One way is to list all the possible data access methods in an abstract repository class or super-interface, as Spring Data does for example, or to create specialized repository interfaces for the task.

存储库旨在代表一个实体对象的概念集合，但是有时他们只是作为数据访问对象。有两种实现方法，一种方法是列出所有的抽象存储库类或超接口可能的数据访问方法，例如Spring Data，或者创建专门存储库接口。

For the user management app, I went for the second approach. UserRepository and SessionRepository list only those methods which are absolutely necessary to deal with their entities.

对于用户管理应用程序，作者选择了第二种方法。UserRepository和SessionRepository只列出那些绝对必要的处理他们实体的放法。

## Project structure

You might have already noticed, that there’s a GitHub repo springuni-particles which contains the some pieces of the user management app, but it doesn’t contain an executable version of the application itself.

你可能已经注意到，这有一个GitHub伤的一个库：springuni，它包含用户管理应用程序的一些部分，但它不包含应用程序本身的可执行版本。

![2-2](2-2.png)

The reason, why I wouldn’t provide a single repo with just dropping Spring Boot in with a few @Enable* annotations, is reusability. Most of the projects I came across seamed to be modular at a first glance, however their are just large monoliths without a clear separation of concerns. When you tried to reuse a module of a such a project, you quickly realize, that it depends on many other modules and/or too many external libraries.

究其原因，我为什么不提供单一只包含Spring Boot少量@Enable*注解的库，是为了可重用性。大多数我碰到的项目第一眼看起来是可以模块化的，但实际上他们只是没有良好分解职责的巨大单体应用。当你试图重用这样一个项目的模块，你很快意识到，它依赖于许多其他模块和/或过多的外部库。


springuni-particles (it could have been also called springuni-modules) provides only reusable pieces of modules for certain well defined functionalities. User and session management are examples for that.

springuni-particles（它可能已被也称为springuni模块）提供了多个模块的可重复使用的只为片某些明确定义的功能。用户和会话管理是很好的例子。

## Modules
## 模块

springuni-auth-model contains all the domain model classes and that business logic which is needed to manage users’ lifecycle and it’s completely framework agnostic. Its repositories and can be implemented by using any data storage mechanism which is the best fit for the actual task at hand. As well as, PasswordChecker and PasswordEncryptor can be used to implement any strong password hashing technique.

springuni-auth-model包含了所有的领域模型类和用于管理用户生命周期的业务逻辑，它是完全框架无关的。它的存储库，并且可以使用任何数据存储机制，对于手头的实际任务最符合。还有，PasswordChecker和PasswordEncryptor可基于任何强大的密码散列技术实现。

springuni-commons is just the dumping ground of common utilities. It’s tempting to pull well-known 3rd party libs (eg. Apache Commons Lang, Guava, etc.) in, which extend the standard library of the JDK. On the other hand, I found myself many times just using only a few classes of these very extensive libraries. I particularly like Apache Commons Lang’s StringUtils and Apache Common Collection’s CollectionUtils classes, however, I would rather provide a highly specialized StringUtils and CollectionUtils classes for the current project instead of adding external dependencies.

springuni-commons包含了通用的工具库。有很多著名的第三方库（如Apache Commons Lang，Guava等），这外延了JDK的标准库。在另一方面，我发现自己很多时候仅仅只用这些非常可扩展库的少量类。我特别喜欢的Apache Commons Lang中的StringUtils的和Apache共同集合的  CollectionUtils类，但是，我宁愿为当前项目提供一个高度定制化的StringUtils和CollectionUtils，这样就不需要添加外部依赖。

sprinuni-crm-model is here to define the common value objects for handling contact data, like address, country etc. Although the advocates of microservice architecture would vote against using shared libraries, yet I think that this particular point might need to revised from time-to-time for the task at hand. I was involved in a few CRM integration projects recently and having to re-implement the domain model for almost the same thing at various bounded contexts (ie. User, Customer, Contact) over and over again was tedious. That said, I think that’s worth having a small common library for the domain model of contact data.

sprinuni-crm-model定义了通用的值对象，用于处理联系人数据，如地址，国家等。虽然微服务架构的倡导者将投票反对使用共享库，但我认为这个特定点可能需要不时修订手头的任务。我最近参与了一些CRM集成项目，不得不重新实现了几乎同样的领域模型在不同的限界上下文（即用户，客户，联系人），这样一遍又一遍地是乏味的。也就是说，我认为使用联系人数据领域模型的小型通用库是值得尝试的。


## Next in this series
## 这个系列的下一部分

Building a user management microservice (Part 3): Implementing and testing repositories 