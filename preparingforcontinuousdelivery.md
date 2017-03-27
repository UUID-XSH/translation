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
• Reduce release risk and avoid failed deployments into both testand production environments;

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
The truth is quite the opposite. The practices and systems you putin place to support continuous delivery will almost certainly raise quality and give you additional safety nets if things do go wrong with a software release.

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
|:---|| Implement a single script or command that enables you to go from version controlled source code to a single deployment ready artifact.|
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
| :------|| Implement a continuous integration process that continually outputs a set of deployment-ready artifacts.|
|实现持续集成过程就是持续输出一组可用于部署的制品 || Evaluate cloud-based continuous integration offerings to expedite your continuous delivery efforts.|
| 评估基于云的持续集成产品，以加快您的持续交付进程 || Integrate a thorough audit trail of what has changed with each build through integration with issue tracking software such as Jira.|
| 通过发布跟踪软件（如Jira）的集成，整合对每个构建所发生变化的详细审计跟踪|

Your continuous integration tooling will likely be central for your continuous delivery efforts. For instance, it can go beyond builds and into testing and deployment. For this reason, continuous integration is a key element of your continuous delivery strategy.

您的持续集成工具可能对您的持续交付工作至关重要。 例如，它可以超越构建并进入测试和部署。 因此，持续集成是您持续交付策略的关键要素。## 4.3 AUTOMATED TESTING
## 4.3 自动化测试

Though continuous delivery can (and frequently does) include manual exploratory testing stages performed by a QA team, or end user acceptance testing, automated testing will almost certainly be a key feature in allowing you to speed up your delivery cycles and enhance quality.

虽然持续交付可以（并且经常）包括由质量保证团队执行的手动测试阶段或最终用户验收测试，但是自动测试几乎肯定将是您加快交付周期并提高质量的关键功能。
Usually, your continuous integration server will be responsible for executing the majority of your automated tests in order to validate every developer check-in.

通常，您的持续集成服务器将负责执行大多数的自动化测试，以验证每个开发人员提交的代码。
However, other automated testing will likely subsequently take place when the system is deployed into test environments, and you should also aim to automate as much of that as possible. Your automated testing should be detailed, testing multiple facets of your application:

然而，当系统部署到测试环境中时，某些自动化测试可能会被需要执行，因此您还应该尽可能多的实现自动化测试。您的自动化测试应该是详尽的，能够覆盖测试应用程序的多个方面：

|TEST TYPE|TO CONFIRM THAT|
|:--|:--|
|Unit Tests 单元测试|Low level functions and classes work as expected under a variety of inputs. 底层函数和类在不同输入条件下按照预期工作||Integration Tests 集成测试|Integrated modules work together and in conjunction with infrastructure such as message queues and databases.集成模块与消息队列和数据库等基础设施协同工作||Acceptance Tests 验收测试|Key user flows work when driven via the user interface, regarding your application as a complete black box.按照目标用户操作流程通过用户界面驱动，将您的应用程序作为一个完整的黑盒子||Load Tests 负载测试|Your application performs well under simulated real world user load.您的应用程序在模拟的真实用户负载下运行良好||Performance Tests 行为测试|The application meets performance requirements and response times under real world load scenarios.该应用程序在实际负载情况下满足性能要求和响应时间要求。||Simulation Tests 模拟测试|Your application works in device simulation environments. This is especially important in the mobile world where you need to test software on diverse emulated mobile devices.您的应用程序在设备仿真环境中工作。这在移动端尤其重要，您需要在各种模拟的移动设备上测试软件。||Smoke Tests 冒烟测试|Tests to validate the state and integrity of a freshly deployed environment.验证新部署环境的状态和完整性||Quality Tests 质量测试|Application code is high quality – identi ed through techniques such as static analysis, conformance to style guides, code coverage etc.应用代码是高质量的 - 通过静态分析，代码风格指南，代码覆盖度等技术来识别。|

Ideally, these tests can be spread across the deployment pipeline,with the slower and more expensive tests occurring further down the pipeline, in environments that are increasingly production-like as the release candidate looks increasingly viable. The aim should be to identify problematic builds as early as possible in order to avoid re-work, keep the cycle time fast and get feedback as early as possible:

理想情况下，这些测试可以分布在部署管道中，随着管道中的测试越来越详细和价值越来越高，在生产环境中，这些发布候选工件看起来越来越可靠。其目标应该是尽早确定有问题的构建，以避免返工，尽快缩短周期时间和获得反馈。

| AIM TO: |
| :------|| Automate as much of your testing as possible.|
|让您的测试尽可能多的实现自动化 || Provide good test coverage at multiple levels of abstraction against both code artifacts and the deployed system.|
| 提供针对代码部件和部署系统的多级抽象的良好测试覆盖 || Distribute different classes of tests along your deployment pipeline, with more detailed tests occurring in increasingly production-like environments later on in the process, while avoiding human re-work.|
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



## 小火占坑