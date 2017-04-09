# Preparing for Continuous Delivery

# 为持续交付做准备

### Building Your DevOps Pipeline  建立您的DevOps管道

## 目录：

- 关于持续交付
- 目标
- 预备步骤
- 实现CD管道
- CD最佳实践及其他

# 1.关于持续交付

Continuous delivery is a set of patterns and best practices that can help software teams dramatically improve the pace and quality of their software delivery.

持续交付是一组能够帮助软件开发团队极大的提高其软件交付的速度和质量的模式和最佳实践组成。

Instead of infrequently carrying out relatively big releases, teams practicing continuous delivery instead aspire to deliver smaller batches of change into production, but much more frequently than usual — weekly, daily, or potentially multiple releases per day.

不同于低频率发布相对较大的版本，实施持续交付的团队希望比通常更频繁将更小批量的变更投入生产， 例如每周，每天或同一天都可能发布多个版本。

This style of software delivery can bring many benefits, as evidenced by market leaders such as Facebook, LinkedIn, and Twitter, who release software very frequently and iteratively and achieve success as a result. To get there, however, requires work and potentially significant changes to your development and delivery processes.

这种软件交付方式可以带来许多的好处，正如Facebook，LinkedIn和Twitter等市场领导者所证明的那样，他们更频繁的以迭代方式发布软件，并取得了成功。 然而，要达到那样的目的，需要对您的开发和交付方式进行一些潜在的重大变革。

This Refcard explains these requirements in more detail, giving guidance, advice, and best practices to development and operations teams looking to move from traditional release cycles towards continuous delivery.

本文详细地解释了这些要求，为准备从传统发布方式转向持续交付方式的开发和运维团队提供指导、建议和最佳实践案例。

# 2.目标

Continuous delivery should help you to:

持续交付将在以下方面为您提供帮助：

• Deliver software faster and more frequently, getting valuable new features into production as early as possible;

尽可能快地交付软件，尽可能早地将有价值的新功能运用于生产

• Increase software quality, system uptime and stability;

提高软件质量，系统正常运行时间和稳定性;

• Reduce release risk and avoid failed deployments into both test
and production environments;

降低发布风险，避免在测试和生产环境同时部署失败;

• Reduce waste and increase efficiency in the development and delivery process;

减少浪费，提高开发和交付过程的效率;

• Keep your software in a production-ready state such that you can deploy whenever you need.

使您的软件始终处于生产就绪状态，以便您可以随时部署。

# 3.预备步骤

To get there, however, you may need to put the following into place:

为了达到目的，您首先需要有以下基础：

• Development practices such as automated testing;

自动化测试等开发实践;

• Software architectures and component designs that facilitate more frequent releases without impact to users, including feature flags;

软件结构和组件设计，可帮助做更频繁的发布，而不影响用户，包括功能标志；

• Tooling such as source code management, continuous integration, configuration management and application release automation software;

工具如源代码管理，持续集成，配置管理和应用发布自动化软件；

• Automation and scripting to enable you to repeatedly build, package, test, deploy, and monitor your software with limited human intervention;

自动化和脚本化，使您能够以有限的人为干预重复构建，打包，测试，部署和监控软件；

• Organizational, cultural, and business process changes to support continuous delivery.

组织，文化和业务流程的变化，以支持持续交付。

On hearing about continuous delivery, some people’s first concern is that it implies that quality standards will slip or that the team will need to take shortcuts in order to achieve these frequent releases of software.

听到持续交付这个词，有些人的第一个担忧就是这是否意味着软件质量标准将会下滑，否则团队需要走捷径才能实现软件的频繁发布。

The truth is quite the opposite. The practices and systems you put
in place to support continuous delivery will almost certainly raise quality and give you additional safety nets if things do go wrong with a software release.

事实恰恰相反，为了支持持续交付而采取的措施和体系几乎肯定会提高软件发布的质量，并且在软件版本出错时将给予您额外的安全防护。

Your software will still go through the same rigorous stages of testing as it does now, potentially including manual QA testing phases. Continuous delivery is simply about allowing your software to flow through the pipeline that you design, from development to production, in the most rigorous and efficient way possible.

您的软件仍然会经历与现在相同的严格测试阶段，可能包括手动质量检查测试阶段。持续交付只是让您的软件以最严格和最有效的方式在您设计的流程中流通，从开发一直到生产。


## 4.THE KEY BUILDING BLOCK OF CONTINUOUS DELIVERY: AUTOMATION!

## 4.连续交付的关键构建模块：自动化！

Though it is quite valid and realistic to have manual steps in your continuous delivery pipeline, automation is central in speeding up the pace of delivery and reducing cycle time.

尽管在持续交付流程中采取手动步骤是非常有效和现实的，但自动化是加快交付步伐和缩短周期时间的关键。

After all, even with a well-resourced team, it is not viable to build, package, compile, test, and deploy software many times per day by hand, especially if the software is in any way large or complex.

毕竟，即使拥有再丰富资源的团队，手工构建，打包，编译，测试和部署软件也是不可行的，尤其是软件很大或者复杂的情况下。

Therefore, the overriding aim should be to increasingly automate away much of the pathway between the developer and the live production environment. Here are some of the major areas you should focus your automation efforts on.

因此，最重要的目标应该是使开发者和生产环境之间的大部分路径自动化。 以下是您应该专注于自动化工作的一些主要领域。

## 4.1 AUTOMATED BUILD AND PACKAGING

## 4.1 自动化构建和打包

The first thing you will need to automate is the process of turning developers’ source code into deployment-ready artifacts.

需要实现自动化的第一件事就是将开发人员的源代码转换为部署就绪制品的这个过程。

Though most software developers make use of tools such as Make, Ant, Maven, NuGet, npm, etc. to manage their builds and packaging, many teams still have manual steps that they need to carry out before they have artifacts that are ready for release.

虽然大多数软件开发人员使用诸如Make，Ant，Maven，NuGet，npm等工具来管理其构建和打包，但是许多团队在制作好准备发布的制品之前仍需要执行一些手动步骤。

These steps can represent a significant barrier to achieving continuous delivery. For instance, if you release every three months, manually building an installer is not too onerous. If you wish to release multiple times per day or week, however, it would be better if this task was fully and reliably automated.

这些步骤是实现持续交付的重大障碍的代表。 例如，如果每三个月发布一次，手动构建与安装就显得不那么繁杂。但是，如果您希望每天或每周发布多次，那么这个任务完全可靠地自动化会更合适。

| AIM TO（目标）:|
|:---|
| Implement a single script or command that enables you to go from version controlled source code to a single deployment ready artifact.|
|实现单个脚本或命令，使您能够将版本控制的源代码转换为单个部署就绪的制品。|


## 4.2 AUTOMATED CONTINUOUS INTEGRATION

## 4.2 自动化持续整合

Continuous integration is a fundamental building block of continuous delivery.

持续整合是持续交付的基本组成部分。

It involves combining the work of multiple developers and continually compiling and testing the integrated code base such that errors are identified as early as possible.

它涉及整合多个开发人员的代码，并不断编译和测试集成的代码库，以便尽可能早地识别错误。

Ideally, this process will make use of your automated build so that your continuous integration server is continually emitting a deployment artifact containing the integrated work of the development team, with the result of each build being a viable release candidate.

理想情况下，此过程将利用自动化构建，从而使您的持续集成服务器不断地发布包含开发团队集成工作的部署制品，每个构建的结果都是可行的发布候选。

Typically, you will set up a continuous integration server or cloud service such as Jenkins, TeamCity, or Team Foundation Server to carry out the integration many times per day, potentially on each commit.

通常，您将建立一个连续集成服务器或相关云服务（如Jenkins，TeamCity或Team Foundation Server），每天可能会执行多次集成，很可能在每次提交时触发。

Third party continuous integration services such as CloudBees DEV@cloud, Travis CI, or CircleCI can help to expedite your continuous delivery efforts. By outsourcing your continuous integration platform, you are free to focus on your continuous delivery goals, rather than on administration and management of tools and infrastructure.

第三方持续集成服务，如CloudBees DEV @ cloud，Travis CI或CircleCI可以帮助您加快您的持续集成进度。 通过外包您的持续整合平台，您可以自由地专注于持续交付的目标，而不是管理工具和基础架构。

| AIM TO: |
| :------|
| Implement a continuous integration process that continually outputs a set of deployment-ready artifacts.|
|实现持续集成过程就是持续输出一组可用于部署的制品 |
| Evaluate cloud-based continuous integration offerings to expedite your continuous delivery efforts.|
| 评估基于云的持续集成产品，以加快您的持续交付进程 |
| Integrate a thorough audit trail of what has changed with each build through integration with issue tracking software such as Jira.|
| 通过发布跟踪软件（如Jira）的集成，整合对每个构建所发生变化的详细审计跟踪|

Your continuous integration tooling will likely be central for your continuous delivery efforts. For instance, it can go beyond builds and into testing and deployment. For this reason, continuous integration is a key element of your continuous delivery strategy.

您的持续集成工具可能对您的持续交付工作至关重要。 例如，它可以超越构建并进入测试和部署。 因此，持续集成是您持续交付策略的关键要素。

## 4.3 AUTOMATED TESTING
## 4.3 自动化测试

Though continuous delivery can (and frequently does) include manual exploratory testing stages performed by a QA team, or end user acceptance testing, automated testing will almost certainly be a key feature in allowing you to speed up your delivery cycles and enhance quality.

虽然持续交付可以（并且经常）包括由质量保证团队执行的手动测试阶段或最终用户验收测试，但是自动测试几乎肯定将是您加快交付周期并提高质量的关键功能。

Usually, your continuous integration server will be responsible for executing the majority of your automated tests in order to validate every developer check-in.

通常，您的持续集成服务器将负责执行大多数的自动化测试，以验证每个开发人员提交的代码。

However, other automated testing will likely subsequently take place when the system is deployed into test environments, and you should also aim to automate as much of that as possible. Your automated testing should be detailed, testing multiple facets of your application:

然而，当系统部署到测试环境中时，某些自动化测试可能会被需要执行，因此您还应该尽可能多的实现自动化测试。您的自动化测试应该是详尽的，能够覆盖测试应用程序的多个方面：

|TEST TYPE|TO CONFIRM THAT|
|:--|:--|
|Unit Tests 单元测试|Low level functions and classes work as expected under a variety of inputs. 底层函数和类在不同输入条件下按照预期工作|
|Integration Tests 集成测试|Integrated modules work together and in conjunction with infrastructure such as message queues and databases.集成模块与消息队列和数据库等基础设施协同工作|
|Acceptance Tests 验收测试|Key user flows work when driven via the user interface, regarding your application as a complete black box.按照目标用户操作流程通过用户界面驱动，将您的应用程序作为一个完整的黑盒子|
|Load Tests 负载测试|Your application performs well under simulated real world user load.您的应用程序在模拟的真实用户负载下运行良好|
|Performance Tests 行为测试|The application meets performance requirements and response times under real world load scenarios.该应用程序在实际负载情况下满足性能要求和响应时间要求。|
|Simulation Tests 模拟测试|Your application works in device simulation environments. This is especially important in the mobile world where you need to test software on diverse emulated mobile devices.您的应用程序在设备仿真环境中工作。这在移动端尤其重要，您需要在各种模拟的移动设备上测试软件。|
|Smoke Tests 冒烟测试|Tests to validate the state and integrity of a freshly deployed environment.验证新部署环境的状态和完整性|
|Quality Tests 质量测试|Application code is high quality – identi ed through techniques such as static analysis, conformance to style guides, code coverage etc.应用代码是高质量的 - 通过静态分析，代码风格指南，代码覆盖度等技术来识别。|

Ideally, these tests can be spread across the deployment pipeline,
with the slower and more expensive tests occurring further down the pipeline, in environments that are increasingly production-like as the release candidate looks increasingly viable. The aim should be to identify problematic builds as early as possible in order to avoid re-work, keep the cycle time fast and get feedback as early as possible:

理想情况下，这些测试可以分布在部署管道中，随着管道中的测试越来越详细和价值越来越高，在生产环境中，这些发布候选工件看起来越来越可靠。其目标应该是尽早确定有问题的构建，以避免返工，尽快缩短周期时间和获得反馈。

| AIM TO: |
| :------|
| Automate as much of your testing as possible.|
|让您的测试尽可能多的实现自动化 |
| Provide good test coverage at multiple levels of abstraction against both code artifacts and the deployed system.|
| 提供针对代码部件和部署系统的多级抽象的良好测试覆盖 |
| Distribute different classes of tests along your deployment pipeline, with more detailed tests occurring in increasingly production-like environments later on in the process, while avoiding human re-work.|
| 在您的部署管道中分发不同类别的测试，模拟日后的生产环境并进行更加详细的测试，同时避免人力返工。|

In a microservice-style environment, integration and contract tests across deployed components are increasingly important. In such an environement, the ability to automatically deploy all necessary related apps (see next section) becomes a high-priority task.

在微服务风格的环境中，跨部署组件的集成和协同测试越来越重要。在这样的环境中，自动化部署所有必需的应用程序（见下一节）的功能成为高优先级的任务。

Automated tests are your primary line of defense in your aim to release high-quality software more frequently. Investing in these tests can be expensive up front, but this battery of automated tests will continue to pay dividends across the lifetime of the application.

自动化测试是您发布高质量软件的主要防线，投资这些测试可能是昂贵的，但这一系列的自动化测试将在应用的整个生命周期内提供持续帮助。

## 4.4 AUTOMATED DEPLOYMENTS
## 4.4 自动化部署

Software teams typically need to push release candidates into different environments for the different classes of testing discussed above.

软件团队通常需要将上述的不同类别的测试推送到不同的环境中。

For instance, a common scenario is to deploy the software to a test environment for human QA testing, and then into some performance test environment where automated load testing will take place. If the build makes it through that stage of the testing, the application might later be deployed to a separate environment for UAT or beta testing.

例如，常见的情况是将软件部署到测试环境进行人为的质量检查测试，然后进行一些性能测试环境，其中将进行自动化负载测试。 如果构建通过该测试阶段，则应用程序可能稍后部署到用于UAT或beta测试的单独环境中。

Ideally, the process of reliably deploying an arbitrary release candidate, as well as any other systems it communicates with, into an arbitrary environment should be as automated as possible.

理想情况下，将任意发布候选部件以及与之通信的其他系统可靠地部署到任意环境中的这个过程应尽可能实现自动化。

## Page 4

If you want to operate at the pace that continuous delivery implies, you are likely to need to do this many times per day or week, and it’s essential that it works quickly and reliably.
如果您希望按照计划的速度持续交付，那可能需要每天或每周多次执行此，因此它的工作速度和可靠性至关重要。

Application release automation tools such as XebiaLabs’ XL Deploy can facilitate the process of pushing code out to environments. XL Deploy can also provide self-service capabilities that allow teams to pull release candidates into their environments without requiring development input or having to create change tickets or wait on middleware administrators.
XebiaLabs的XL Deploy等应用程序发布自动化工具可以帮助你推送代码到运行环境。 XL Deploy还可以提供发布自助服务功能，使团队能够将发布应用程序到他们的环境中，而无需开发的帮助，或者必须需要中间件开发者的帮助。

This agility in moving software between environments in an automated fashion is one of the main areas where teams new to continuous delivery are lacking, so this should also be a key focus in your own preparations for continuous delivery
用自动化方式在环境之间移动软件是作为继续交付的团队的主要特性之一，因此这也是继续交付的关键重点


AIM TO:
Be able to completely roll out an arbitrary version of your software to an arbitrary environment with a single command.
能够简化化的在任意环境中部署发布某特定版本。

Incorporate smoke test checks to ensure that your deployed environment is then valid for use.
使用冒烟测试确保部署的系统的可用性。

Harden the deploy process so that it can never leave environments in a broken or partially deployed state. Incorporate self-service capabilities into this process, so QA staff or business users can select a version of the software and have that deployed at their convenience. In larger organizations, this process should incorporate business rules such that specific users have deployment permissions for specific environments.
加强部署过程，使其永远不会使环境处于断开或部分部署状态。 将自助服务功能纳入此过程，因此质量保证人员或业务用户可以选择软件版本，并在其方便部署。 在较大的组织中，此过程应包含业务规则，使特定用户具有特定环境的部署权限。


Evaluate application release automation tools in order to accelerate your continuous delivery efforts.
评估应用的版本自动化工具，以加快持续部署能力。


# 管理你的基础架构和云服务


In a continuous delivery environment, you are likely to want to create and tear down environments with much more flexibility and agility in response to the changing needs of the project.

在持续的交付环境中，可能希望具有更多灵活性和敏捷性来应对环境的创建和拆除，应对项目不断变化的需求。

If you want to start up a new environment to add into your deployment pipeline and that process takes months to requisition hardware, configure the operating system, configure middleware, and set it up to accept a deployment of the software, your agility is severely limited and your ability to deliver is impacted.

如果要启动新的环境以添加到部署管道中，并且该过程需要几个月的购买硬件，配置操作系统，配置中间件并将其正确部署，敏捷性受到严重限制， 你的交付能力受到限制。


Taking advantage of virtualization and cloud-based offerings can help here. Consider cloud hosts such as Amazon EC2, Microsoft Azure or Google Cloud Platform to give you flexibility in bringing up new environments and new infrastructure as the project dictates.

利用虚拟化和基于云的产品可以帮助你。 考虑像Amazon EC2，Microsoft Azure或Google Cloud Platform这样的云端主机，您可以根据项目的要求灵活地开发新的环境和新的基础架构。

The cloud can also make an excellent choice for production applications, giving you more consistency across your development and production environments than previously achieved.

云也可以为生产应用程序做出最佳选择，使你在开发和生产环境中实现高度的一致性。

AIM TO:
Cater for flexibility to your continuous delivery processes so you can alter pipelines and scale up or down as necessary. 
为持续交付流程提供更多灵活性，以便您可以根据需要更改配置。

Implement continuous delivery infrastructure in the cloud, giving you agility in quickly rolling out new environments, and elasticity to pause or tear down those environments when there is less demand for them.
在云中实施持续的交付基础设施，能够快速推出新环境，并在弹性较小的情况下暂停或拆除这些环境。


# INFRASTRUCTURE AS CODE

A very common class of production incidents, errors, and rework happens when environments drift out of line in terms of their configuration — for instance, when development environments start to differ from test, or when test environments drift out of line with production.

当配置方面不一致时候，例如当开发环境与测试不同，或当测试环境与生产不一致时，会发生非常常见的一系列生产事件，错误最终导致返工。

Configuration management tools such as Puppet, Chef, Ansible, or Salt and environment modeling tools such as Vagrant or Terraform can help you avoid this by defining infrastructure and platforms as version controlled code, and then having the environments built automatically in a very consistent and repeatable way.
配置管理工具（如Puppet，Chef，Ansible或Salt）和环境建模工具（如Vagrant或Terraform）可帮助您通过将基础架构和平台提供版本控制代码来避免这种情况，然后将环境自动构建成非常一致和可重复。

Combined with cloud and outsourced infrastructure, this cocktail allows you to deploy accurately configured environments with ease, giving your pace of delivery a real boost.
结合云和外包基础设施，这种鸡尾酒【混合模式】可以让您轻松部署准确配置的环境，从而为您的交付速度带来真正的提升。

Vagrant and Terraform can also help by giving developers very consistent and repeatable development environments that can be virtualized and run on their own machines. Container frameworks (see next section) are another popular option to achieve this.
Vagrant和Terraform也可以为开发人员提供非常一致和可重复的开发环境，可以在自己的机器上进行虚拟化和运行。 容器（见下一节）是实现此目的的另一个常用选项。

These tools are all important because consistency of environments is a huge enabler in allowing software to flow through the pipeline in a consistent and reliable way
这些工具都非常重要，因为环境的一致性是允许软件以一致和可靠的方式流过管道的巨大推动力

AIM TO:
Implement configuration management tools, giving you much more control in building environments consistently, especially in conjunction with the cloud.

实施配置管理化，更加全面地控制构建环境，特别是与云结合使用。

Investigate Vagrant, Terraform and container frameworks as a means of giving developers very consistent local development environments.
Vagrant，Terraform和容器框架，以此为开发者提供非常一致的本地开发环境。

# CONTAINER FRAMEWORKS
One way or another, containers will very likely crop up at some point in your continuous delivery preparation: whether as a new runtime for your production environment, a more lightweight means of creating a reproducible local development setup, or simply as a technology to track for potential future use.

另一种方式，容器在连续交付准备中的某个时刻很可能会出现：无论是作为生产环境的新运行时间，或者创建可重复的本地开发设置的更轻量级的手段，还是简单的跟踪技术潜在的未来使用。

If you decide to try to containerize your applications, ensure you also investigate orchestration frameworks such as Docker Swarm, Mesos, or Kubernetes, which will allow you to define and control groups of related containers as a single, versioned entity. Since these frameworks are still “rough around the edges” in places, optionally also look one of a variety of management tools such as Deis, Rancher, or OpenShift, to simply usage.

如果您决定尝试将应用程序集中起来，请确保您还可以调查业务流程框架，如Docker Swarm，Mesos或Kubernetes，这将允许您将相关容器组定义和控制为单个版本化实体。由于这些框架在某些地方仍处于“粗糙的边缘”，所以还可以选择Deis，Rancher或OpenShift等各种管理工具之一来简单地使用。

Should you be planning to run your production environment on containers, ensure that the orchestration framework you choose can be used for local development, too. Otherwise, there is, again, a risk that the way containers are “linked up” on a developer’s machine will not match what will happen in production.
如果您计划在容器上运行生产环境，请确保您选择的业务流程框架也可用于本地开发。否则，再次存在容器在开发机器上“联系起来”的方式的风险与生产中将会发生什么不一致。

# AUTOMATED PRODUCTION DEPLOYMENTS

Though most software teams have a degree of automation in their builds and testing, the actual act of deployment onto production servers is often still one of the most manual processes for the typical software team.
虽然大多数软件团队的构建和测试都具有一定程度的自动化，但是在生产服务器上部署的实际行为通常仍然是典型软件团队最为人为的过程之一。


For instance, teams might have multiple binaries that are pushed onto multiple servers, some database upgrade scripts that are manually executed, and then some manual installation steps to connect these together. Often they will also carry out manual steps for the startup of the system, and smoke tests.
例如，团队可能会将多个二进制文件推送到多个服务器上，一些手动执行的数据库升级脚本，然后是一些手动安装步骤来将它们连接在一起。他们通常也会执行系统启动的手动步骤和冒烟测试。


Because of this complexity, releases often happen outside of business hours. Indeed, some unfortunate software teams have to perform their upgrades and scheduled maintenance at 3am on a Sunday morning in order to cause the least disruption to the customer base!
由于这种复杂性，发布经常发生在营业时间之外。事实上，一些不幸的软件团队必须在星期天早上凌晨3点进行升级和定期维护，以免对客户群造成最小的影响！

To move towards continuous delivery, you’ll need to tackle this pain and slowly script and automate away the manual steps from your production release process such that it can be run repeatedly and consistently. Ideally, you will need to get to the stage where you can do this during business
要实现持续交付，您需要解决这一痛苦，并缓慢地编写脚本并自动从您的生产发布过程中手动执行步骤，以便可以重复和一致地运行。理想情况下，您将需要到达您在业务过程中可以做到的阶段

hours while the system is in use. This may have significant consequences for the architecture of your system. To make production deploys multiple times per day whilst the system is in use, it’s important to ensure that the process is also tested and hardened so you never leave the production application in a broken state due to a failed deploy.
系统正在使用的时间。这可能会对系统的体系结构产生重大影响。为了使系统在使用中每天进行多次生产，重要的是要确保该过程也经过测试和加固，以免由于部署失败而使生产应用程序处于断开状态。


AIM TO:

Completely automate the production deploy process such that it can be executed from a single command or script.
完全自动化生产部署过程，使其可以从单个命令或脚本执行。

Be able to deploy the next version of the software while the production system is live, and switch over to the new version with no degradation of service.
在生产系统生效的同时，可以部署软件的下一个版本，并切换到新版本，而不会降低服务质量。

Be able to deploy to production using exactly the same process by which you deploy to other environments.
能够使用完全相同的部署到其他环境的进程部署到生产。

Implement the best practices described below, such as canary releasing, rollback, and monitoring in order to enhance stability of the production system.
实施下面描述的最佳做法，例如金丝雀释放，回滚和监控，以提高生产系统的稳定性。



# IMPLEMENTING A CONTINUOUS DELIVERY PIPELINE


A delivery pipeline is a simple but key pattern that gives you a framework towards implementing continuous delivery.
输送管道是一个简单但关键的模式，为您提供实现持续交货的框架。

The pipeline describes:
管道描述
• The explicit stages that software moves between on its path from source control to production;
软件在从源头控制到生产的路径之间的显式阶段;
• Which stages are automated and which have manual steps;
哪些阶段是自动化的，哪些阶段有手动步骤？
• What the criteria are for moving between pipeline stages,capturing which gateways are automated and which are manual;
在流水线阶段之间移动的标准是什么，捕获哪些网关是自动化的，哪些是手动的？
• Where parallel flows are allowed.
允许并联流量。
Importantly, the delivery pipeline concept gives us visibility into the production readiness of our release candidates.
重要的是，交付管道的概念让我们了解我们发布候选人的生产准备情况。
For instance, if you know that you have a release candidate in UAT and a release candidate just about to pass performance testing with a certain set of additional features, you can use this to make decisions about how, when,and what to release to production.
例如，如果您知道您在UAT中有发布候选，以及即将通过一系列附加功能进行性能测试的发行人候选，则可以使用它来决定如何，何时以及要发布到生产 。


STEP 1: MODEL YOUR PIPELINE
步骤1：建立您的管道

The first step in putting together a delivery pipeline is to identify the stages that you would like your software to go through to get from the source control repository into production.
组装交付流程的第一步是确定您希望软件完成的阶段，以便从源代码库进行生产管理。


The typical software development team will have a number to choose from, some of which are automated and some of which are manual:
典型的软件开发团队将有一些可供选择，其中一些是自动化的，其中一些是手动的：


While implementing continuous delivery, you may wish to take the opportunity to add in stages above and beyond those that you do today.
在实施持续交付的同时，您可能希望借此机会添加超越你今天所做的工作。

For instance, perhaps adding automated acceptance testing would reduce the scope of manual testing required, speeding up development cycles and increasing your potential for continuous delivery. Perhaps adding automated performance testing or manual user beta testing will allow you to shorten your cycle time still further and release more frequently.
例如，也许增加自动验收测试将减少所需的手动测试的范围，加快开发周期并增加您的持续交货的能力。也许添加自动性能测试或手动用户测试可以让您缩短周期时间，进一步发布并更频繁地发布。

Having identified which stages are important to you, you should then think about how to arrange the stages into an ordered pipeline, noting the inputs and outputs of each phase. A very simple example of a pipeline might look like this:
确定了哪些阶段对您很重要，然后您应该考虑如何将阶段安排到有序的流程中，并注意到每个阶段的投入和产出。管道的一个非常简单的例子可能如下所示：

Every software team does things in a subtly different way, however. For instance, depending on your comfort with and levels of automated testing, you may decide to skip any form of exploratory testing and rely completely on automated testing processes, reducing the length of the pipeline to a very short fully automated process:
不过，每个软件团队都会以微妙的方式做事情。例如，根据您的舒适度和自动化测试的水平，您可以决定跳过任何形式的探索性测试，并完全依赖于自动测试过程，将管道的长度缩短到非常短的全自动化过程：

Other teams might choose to parallelize flows and testing stages. This is especially useful where testing is manual and stages are time consuming — a fairly likely prospect at the very outset of your continuous integration journey.
其他团队可能会选择并行化流程和测试阶段。这在测试是手动的和阶段是耗时的时候特别有用 - 在您的持续整合之旅开始时，相当可能的前景。

Running performance tests at the same time as UAT might be one example of this parallelization. This can obviously speed up the end to end delivery process when things go well, but it could lead to wasted effort if one branch of the pipeline fails and the release candidate is rejected:
与UAT同时运行性能测试可能是这种并行化的一个例子。当事情进展顺利的时候，这显然可以加快端到端的交付过程，但如果管道的一个分支发生故障，并且发布候选人被拒绝，可能导致浪费的努力：

The pipelines above are extremely simple but illustrate the kind of decisions you will need to make in modeling the flow and implementing your pipeline. The best pipeline isn’t always obvious and requires tradeoffs:
上面的管道非常简单，但说明了您需要在建模流程和实施管道时做出的决策。最好的管道并不总是很明显，需要权衡

IDEAL SITUATION TRADEOFF

All stages and gateways would be automated.
所有阶段和网关都将自动化。

Requires substantial investment in automated tests and release automation.
需要大量投资自动化测试和发布自动化。

Always avoid expensive human re-work.
始终避免昂贵的人工重新工作。

You need to parallelise test phases if they are slow and manual, giving the risk of failed release candidates in another stage of the pipeline. Always perform automated testing.
如果测试阶段较慢且手动，则需要将测试阶段并行化，从而在管道的另一个阶段出现失败的候选人的风险。始终执行自动化测试。

Detailed automated testing is also expensive in production like environments.
详细的自动测试在诸如环境的生产中也是昂贵的。


Implement lots of environments to support testing phases.
实施大量环境来支持测试阶段。


Maintaining environments has an associated management and financial cost.
维护环境具有相关的管理和财务成本。


Whatever position you take on the various tradeoffs, the output of your delivery pipeline modeling should be a basic flow chart that documents the path that your software takes on its journey from source code to production.
无论您对各种权衡采取何种立场，输送管道建模的输出应该是一个基本的流程图，记录了软件从源代码到生产的路径。

STEP 2: IDENTIFY NON-AUTOMATED ACTIVITIES AND GATEWAYS

As previously mentioned, you would ideally like all of the phases of the pipeline to be automated.
如前所述，您最好将管道的所有阶段自动化。


## 小火占坑
In this ideal world, developers would check in code and release candidates would then flow through the pipeline, 

在这个理想的世界中，开发者将要检查代码，然后通过管道将候选者发布出去，


with each step and each gateway between phases automated. Eligible release candidates would be emitted at the end of the pipeline ready for deployment, 

每个步骤和各阶段之间的每个网关都自动化。合格的发布候选版本将被放在管道的末端以备部署，

and you would have complete confidence in every one.

而你会对每个人都抱有信心。

For various reasons, however, this is not always viable. For instance, 

但是，由于各种各种的原因，这并不总是行得通。例如，

common problems are a shortfall in the amount of automated testing or business requirements that mandate manual user acceptance or beta testing. 

通常由于自动化测试的不足以及授权用户和beta测试的业务要求，

Even where high degrees of automated testing are in place, many businesses would likely include manual sign-offs before builds can flow through the pipeline and into production.

即使在高度自动化测试的地方,许多企业在构建通过管道到达生产之前都需要人工签字。

For this reason, our delivery pipeline definitely needs to acknowledge, model, and allow for humans and manual phases in the process:

因为这些原因，我们的发布管道的确需要通知，建模，以及在过程中允许人为和手工操作。

In some instances, you will find that the gateways between phases can also be automated.

在某些情况下，你会发现各个阶段的网关也可以自动化。

For instance, you might allow software to flow into a development- and performance-testing environment automatically if it passes automated testing within the continuous integration server,

比如，如果软件在持续集成服务器中通过了自动化测试，你将会允许它进入一个开发-性能测试自动化的环境。

but you might want exploratory test environment deploys to be controlled by QA staff as a manual and, ideally, self-service step.

但是你可能希望由QA同事去掌控测试环境发布的探索性，这种方式是人工的，甚至是自我服务的。

For both automated and manual gates, you will want to identify the criteria that make them pass. 

对于自动化和人工化两个途径，你都希望去确定它们通过的标准。


If gate criteria are not met, the system should prevent release candidates from progressing through the delivery pipeline.

如果有一个流程的标准没有被满足，那么系统就应该从分发管道中去阻止候选者的发布。

STEP 3: IMPLEMENT YOUR PIPELINE

步骤3：实现你的管道

Once you’ve modeled the flow of your pipeline, you’ll then have the fun task of actually implementing it.

一旦你为了管道流建立了模型，你将在之后的实际实现中感到乐趣。

Most of the implementation work falls into the category of general release automation, as discussed in the above topic.

大部分的实现工作会陷入到一般发布自动化的种类中，如上面讨论的一样。

If you can automate the main tasks around compilation, testing, and deployments, then you will be in good shape to tie these together in the context of the delivery pipeline.

如果你能使编译、测试、和发布都变的自动化，那么你就会很好的在一个发布的管道上下文中把它们变得紧密。

SOFTWARE AND TOOLING

软件和工具

To manage your pipeline, you will need to make the choice between either building proprietary scripting or making the investment in off-the-shelf application release automation tooling to implement the processes.

为了管理你的管道，你将要在构建合适的脚本和致力于实现过程的现成的应用发布自动化工具二者之间做出选择。

Do not discount vendor tools, as these can potentially free up valuable developer and system administrator time that would otherwise be spent managing internal infrastructure and developing release automation glue code. Good software in this category will:

不要在供应工具上打折扣，因为这将会潜在的解放优秀的开发者和系统管理员的时间，省下的时间或者可以用于管理内部基础设施或者开发发布自动化胶水代码。这种优秀的软件将会：

• Formalize the stages and the flows that your software goes through;

标准化阶段和流程，使你的软件良好运转；

• Define the criteria that must be met for release candidates to move between stages in the pipeline;

定义标准使其满足于发布候选者在管道中推进；

• Allow you to parallelize stages of the pipeline where appropriate;

在合适的地方让你在管道阶段并行；

• Report and audit on deployments for management and operational purposes;

在管理和操作目的上报告和审计发布；

• Give you visibility into your pipeline, showing how builds have progressed and what specific changes are associated with each release candidate;

让你能监视到你的管道，构建进度以及各个发布候选者之间显著的改变；

• Give your teams self-service facilities, for instance allowing operations staff to deploy to production and QA to bring a version into their test environment when they are ready. 

A permission model may be incorporated so that only certain authorized people have deployment permissions.

给你的团队自助服务设施，比如让操作者发布产品以及在其就绪的时候让QA把版本放置于他们的测试环境。可以使用权限模型注册功能，以便只有某些授权人员具有部署权限。

CONTINUOUS DELIVERY BEST PRACTICES

持续交付最佳实践

Once you’ve put the fundamentals in place and set up a delivery pipeline, you’ll hopefully already begin to benefit from decreased cycle times and faster delivery.

一旦你将基础就绪并建立好发布管道，你很可能已经准备好从减少循环时间以及快速发布中收益。

Automation should be taking care of lots of the manual jobs, environments and deployments should be more consistent, and release candidates should be flowing between pipeline phases using automated gates and self-service tools.

自动化会取代很多人工任务，环境和发布会变的一致，发布候选者将使用自动路由和自助服务工具在各个管道阶段中流动。

Your software should almost always be in a production-ready state, with release candidates coming out of the end of the pipeline much more frequently than with a traditional delivery process. Each release candidate should be adding a relatively small batch of changes.

你的软件应该近似于时刻处在生产就绪状态，伴随着发布候选人最后从管道的出来的频率远大于从传统途径的频率。每个发布候选者应该增加一批相对较小的改动。

Once you are at this stage, there is always more you can do to improve and move towards even faster delivery cycles while enhancing the stability of your system.

一旦你处于这个阶段，那么你常常会有更多机会去提升和推进更快的发布周期，而这会是你的系统的稳定性更强。

A few of these best practices are listed below.

下面列举了一些最佳实践

IMPLEMENT MONITORING

实施监控

Though everything we have discussed in this document describes a rigorous process that will help you avoid releasing bugs into production, 

尽管这份文档讨论的所有事情是描述一个严格的过程过程去帮助你避免在发布产品的时候出现bug，

it’s also important that you are alerted if something does go wrong in the system as a result of a deployment.

但是一旦发布结果显示出系统出现了某种故障，那么被警示同样显得非常重要。

For instance, if your application starts throwing alerts or exceptions after a deployment, it’s important that you are told straight away so that you can investigate and resolve.

举个例子，如果你的应用开始在分发结束之后开始抛出警示和异常，那么被直接告之就显得极其重要，这意味着你可以调查和结局。

Ideally, this alert will be delivered via some dashboard or monitoring front- end, though an email, text message, Slack alert, or something similar could work as a first step.

理想情况下，这种警示将会以仪表盘或者监控终端的途径传递，比如邮件，文本消息，Slack，或者功能类似的其他途径。

You may also need to go a layer deeper than simply checking for errors in logs and start monitoring the metrics that your application is pushing out. 

你可能会需要更加深入而不仅仅是简单的检查错误日志，并且开始监视应用程序正在推导出的度量。

For instance, if your shopping cart completion rate drops by 20% after a release, then this could indicate a more subtle but very serious error with the latest deployment. Open source tools such as StatsD and Graphite or traffic and browsing analytics tools such as Google Analytics can help here. 

比如，如果你的购物车完成了在一个发布后下降了20%，这可能预示着上一个发布的一个微妙但是严重的错误。诸如StatsD和Graphite之类的开源工具以及通信和浏览分析工具Google Analytics会在此帮助到你。

Again, these metrics should be pushed into your monitoring and alerting dashboards when possible.

同样的，这些度量应该在可能的情况下加入到你的监视和警告的仪表盘当中。

There are a wealth of open source and hosted tools that can help you with intelligent monitoring of your applications. These are definitely worth evaluating as a fast and cost-effective means of supporting your continuous delivery project.
 
有许多有价值的开源和商业工具可以帮助你智能的去监控你的应用。这些都是值得评估作为一个快速和符合成本效益的手段，支持你的连续交付项目。

AIM TO:
Provide real time monitoring of your application, ideally via a visual dashboard.
Track key metrics regarding your applications usage, and alert if a deployment appears to negatively impact these.

为了实时监控你的应用，最理想的方法应该是通过可视化的仪表盘。追踪应用的关键度量，如果部署出现负面的影响就使其发出警报。

IMPLEMENT ROLLBACK

实现回滚

Being able to quickly and reliably roll back changes applied to your production environment is the ultimate safety net.

使你的生产环境具备快速可靠的回滚是最后的安全网。

If a bug slips through despite all of your automated and manual testing, the rollback will enable you to quickly move back to the previous working version of the software before too many users are impacted.

如果一个bug在你的所有手动和自动的测试中通过，那么回滚将使应用在许多用户受到影响之前快速回到之前可用版本。

With the comfort of a good rollback process, you can then be even more ambitious in terms of automating delivery pipeline stages and opening up the gates between stages.

伴随着一个优秀的回滚途径带来的益处，你将在自动发布管道和打开各个阶段之间的大门变得更加有野心。

This confidence will result in a much faster pace of delivery, and you’ll feel more confident in adding automation into the delivery process.

这种信心会导致更快速度的发布，在发布流程中加入自动化会让你变得更有信心。

If the delivery pipeline is implemented well, you should essentially get this rollback capability for free. If version 9 of your software breaks, simply deploy version 8, which should still be available, all signed off and ready to re-deploy at the end of your pipeline.

如果发布管道实现的非常好，你应该不用花费额外代价就能得到这种回滚能力。如果版本9发生了故障，部署版本8就好了，它应该同样可用，所有签署和准备重新部署在您的管道末端。

AIM TO:
Provide a mechanism to quickly and repeatedly roll back any software changes to the system.
Build this into the pipeline process to reduce the need for developers to explicitly code for rollbacks.
Test your rollback regularly as part of your pipeline to retain confidence in your process.

为快速重复的回滚任何软件提供一种机制。
将其构建在管道流程中，以减少开发者显示的去编码。
定义的去测试你的回滚机制，作为测试管道的一部分，以保持你对流程的信心。

EXTRACT ENVIRONMENT-SPECIFIC CONFIGURATION

提取特异于环境的配置

It’s important that you use the same binaries and artifacts right through the pipeline. If QA and UAT are carried out against different binaries, your testing is completely invalidated.

在管道中正确并且一致的去使用二进制包和产品是很重要的。如果QA和UAT在开展工作的过程中不同的二进制包，那么你的测试完全是无效的。

For this reason, you need the ability to push the same application binaries into arbitrary environments, and then deploy environment-specific configuration separately. 

基于这个理由，你需要获得将同样的应用二进制包推进任意环境的能力，接着分开发布环境特异的配置。

This configuration should be version controlled just like any other code, giving you much more repeatability and a better audit trail.

这种配置应该像其他任何代码一样进行版本控制，这使得你获得更多的复用和更好的审计。

Quite often, the main stumbling block to doing this is having environment- specific configuration tied too tightly to the actual application binaries.

确实，这样做最大的阻碍是拥有环境特异的配置会和实际的二进制包紧密的耦合在一起。

Extracting this environment-specific configuration into external properties files or other configuration sources gives you much more agility with regards to this.

将这些环境特异的配置提取出来，放到额外的属性文件或者其它配置源会使你获得对待此类阻碍更大的敏捷性。

AIM TO:
Use the same application binaries throughout your deployment pipeline. Avoid rebuilding from source or processing binaries in any way, even if you believe it is safe to do so.
Extract environment-specific information into version controlled configuration that can be deployed separately from the main deployment artifacts.

在你的发布管道中使用同样的二进制包。避免重新构建源或者以任何方式处理二进制文件，即便你觉得这样做是安全的。将环境特异的信息提取到版本控制的配置，这会使其从主要的发布产品中分开。

PERFORM CANARY RELEASES

实行金丝雀发布

A really useful technique for increasing stability of your production environment is the canary release. This concept involves releasing the next version of your software into production,

一个增加产品环境稳定性的有用的技术是金丝雀发布。这个概念涉及到将你的软件的下个版本发布到生产中，

but only exposing it to a small fraction of your user base. This can be achieved, for example, by using a load balancer to direct only a small percentage of traffic to the new version.

但是只将其暴露给你的用户的一小部分。这可以被做到，例如，使用一个负载均衡器仅仅将一小部分比例的信号量定向到新的版本。

Though the aim is never to introduce any bugs into the production environment, if you do, you would obviously prefer to insulate the bulk of the user base from any issues.

尽管我们的目的从来都不是将任何bug引入到生产环境中，很明显，你更愿意将用户的大部分与任何问题隔离起来。

As you build confidence in the deployment, you can then increasingly expose more of the user base to it, until the previous version is completely out of scope.

当你在发布过程中建立了信心，随后你可以增加新版本对用户的暴露的数量，知道完全取代旧版本。

Being able to deploy canary releases is a huge win with regards to continuous delivery, but it can require significant work and architectural changes in the application.

金丝雀发布在持续发布的一个重大的胜利，但它需要显著的工作和应用架构上的变化。

AIM TO:
Deliver the capability to canary release, ideally while the production application is in use by users.
进行金丝雀发布，当用户数量很多的时候是很理想的。

CAPTURE BUILD AUDIT INFORMATION

捕捉构建审计信息

Ideally, your continuous delivery pipeline should give you a very clear pattern of what specifically has changed about the software with each release candidate. This has benefits all the way through the pipeline.

理想情况下，你的持续发布管道应该会给你带来一个清晰的途径，关于每个发布候选者的变更。这使管道的所有途径受益。

Manual testing can specifically focus on the areas that have changed and you can move forward with more confidence if you know the exact scope of each deployment.

手动测试可以特定的去关注改变过的区域，在你知道每个发布的实际范围后你会更有自信的前进。

By integrating your continuous integration server with issue-tracking software such as Jira or Pivotal Tracker, you can develop highly automated systems where release notes can be built and linked back to individual issues or defects.

通过使用集成服务器上的问题追踪软件诸如Jira和Pivotal Tracker，你可以开发高度自动化的系统发，行说明可以被构建和链接回到个别问题或缺陷。

You should also be sure to capture all binaries that are released into an environment for traceability and investigative reasons. Repository management tools such as Nexus or Artifactory can help here. There are also a growing number of tools that act as dedicated registries for container images.

你应该确保为了可追踪性和可调查性去捕捉被发布到某个环境的所有二进制文件，仓库管理工具诸如Nexus和Artifactory在此非常有用。也有很大一批工具，作为容器镜像作为专门的登记。

AIM TO:
Integrate with issue tracking or change management software to provide detailed audits of issues that are addressed with each release candidate.
Change-control all relevant code and archive all released binaries.

集成问题追踪软件或者变更管理软件将会为每个发布候选着定位到详细的问题审计信息。
变更控制所有相关的代码以及归档所有发布过的二进制文件。

IMPLEMENT FEATURE FLAGS

实现特征标志

Feature flags are a facility that developers build into the software that gives them the ability to toggle specific features on or off with a high degree of granularity. This simple technique can add stability into the system through greater control of how new features are put into production use:

特征标志是一个开发者构建进软件的设施，以获得在一个较高水准的粒度上切换不同特性的能力。这项简单的技术可以通过加大控制如何使新的特性投入生产使用以增加系统的可靠性：

Good feature flags will be:

一个优秀的特性标志将会：

• Managed at runtime without a restart or user interruption.

在运行时被管理而不需要重启或者用户中断。

• Well-tested, in that you should make sure your tests cover all the combinations of features that you plan to use in production.

良好测试，这意味着你应该确保你的测试覆盖了所有你将要计划在生产使用的新特性的组合。

• Identifiable at runtime, allowing you to identify which features are active where, and how they correlate with usage of the system.

运行时识别，允许你识别哪个特性在什么场景下被激活，以及他们与系统用量之间的关系。

Simple feature flags may seem straightforward to build yourself. You will likely quickly find, however, that the number of required features for your feature flag implementation expands rapidly: historical comparison, audit trails and role-based access control to name but a few.

简单的特性标志可能看上去会简单的构建自己。你可能会喜欢快速发现，然而，你的功能标志实现所需的功能数量迅速扩展：历史比较，审计跟踪和基于角色的访问控制的名称，但是不多。

For this reason, investigate dedicated feature flag platforms such as LaunchDarkly, which already provide these and other advanced capabilities, and are usually easily integrated into existing code.

基于这个理由，研究专门的特征标志平台如launchdarkly，已经提供了这些或者其它先进的特性，并且常常很容易就集成到现有的代码中。

AIM TO:
Implement feature flags, with full understanding of implications for QA and production operations.
实现特性标志的时候要充分理解QA和生产操作的影响。

USE CLOUD-BASED AND MANAGED INFRASTRUCTURE

使用基于云和管理基础设施

Throughout this document, there have been many mentions of cloud and various managed infrastructure providers. This is because they represent a fast and cost-effective way of speeding up your continuous delivery efforts.

通过这篇文章，很多云和基础管理提供商被提及。这是因为他们代表了一种快速和成本效益的方式，加快您的持续交付努力。

Cloud and managed infrastructure-as-a-service are particularly useful in supporting teams that have variable requirements of their build and release infrastructure. For instance, you may find that you need to temporarily increase your capacity as you approach release.

云和管理基础设施即服务在支持在构建和发布基础设施时有诸多需求的团队时显得格外有用。比如你可能会发现在你进行发布时，你需要暂时提升你的容量。

The combination of cloud and automated infrastructure management are ideal for handling this variability in your CI requirements. For this reason, cloud-hosted services like CloudBees DEV@cloud (managed Jenkins in the cloud), Travis CI, or CircleCi, are ideal candidates for outsourcing your continuous delivery infrastructure.

云和自动化基础设施管理的结合是处理CI中的变化理想选择。基于这个理由，云托管业务诸如DEV@cloud、Travis CI、CircleCi是外包您的连续交付基础设施的理想候选人。

AIM TO:
Reduce infrastructure administration and management, and support variability in your infrastructure requirements by deploying onto the cloud or infrastructure as a service.

减少基础设施管理，并通过部署到云或基础设施服务使其支持需求的变化。