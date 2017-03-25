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

持续交付是一组模式和最佳实践，能够帮助软件开发团队极大的提高其软件交付的速度和质量。

Instead of infrequently carrying out relatively big releases, teams practicing continuous delivery instead aspire to deliver smaller batches of change into production, but much more frequently than usual — weekly, daily, or potentially multiple releases per day.

代替很少发布改动相对较大的版本，实行持续交付的团队希望将更小批量的变更投入生产，比通常更频繁 - 每周，每天或同一天都可能发布多个版本。

This style of software delivery can bring many benefits, as evidenced by market leaders such as Facebook, LinkedIn, and Twitter, who release software very frequently and iteratively and achieve success as a result. To get there, however, requires work and potentially significant changes to your development and delivery processes.

这种软件交付方式可以带来许多的好处，正如Facebook，LinkedIn和Twitter等市场领导者所证明的那样，他们经常迭代性地发布软件更新并取得了成功。 然而，要达到那样的目的，需要对您的开发和交付方式进行一些潜在的重大变革。

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

降低发布风险，避免测试和生产环境同时部署失败;
• Reduce waste and increase efficiency in the development and delivery process;

减少浪费，提高开发和交付过程的效率;
• Keep your software in a production-ready state such that you can deploy whenever you need.

使您的软件处于生产就绪状态，以便您可以随时部署。

# 3.预备步骤

To get there, however, you may need to put the following into place:

为了达到目的，您首先需要有以下基础：
• Development practices such as automated testing;

自动化测试等开发实践;
• Software architectures and component designs that facilitate more frequent releases without impact to users, including feature flags;

软件体系结构和组件设计，可促进更频繁的发布，同时不影响用户，包括功能标志；
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

您的软件仍然会经历与现在相同的严格测试阶段，可能包括手动质量检查测试阶段。持续交付只是让您的软件以最严格和最有效的方式在您设计的管道中流通，从开发一直到生产。


## THE KEY BUILDING BLOCK OF CONTINUOUS DELIVERY: AUTOMATION!

## 连续交货的关键模块：自动化！

Though it is quite valid and realistic to have manual steps in your continuous delivery pipeline, automation is central in speeding up the pace of delivery and reducing cycle time.

尽管在持续交付流程中采取手动步骤是非常有效和现实的，但自动化是加快交付步伐和缩短周期时间的关键。
After all, even with a well-resourced team, it is not viable to build, package, compile, test, and deploy software many times per day by hand, especially if the software is in any way large or complex.

毕竟，即使拥有再丰富资源的团队，手工构造，打包，编译，测试和部署软件也是不可行的，尤其是软件很大或者极其复杂的情况下。
Therefore, the overriding aim should be to increasingly automate away much of the pathway between the developer and the live production environment. Here are some of the major areas you should focus your automation efforts on.

因此，最重要的目标应该是使开发者和生产环境之间的大部分路径自动化。 以下是您应该专注于自动化工作的一些主要领域。

## AUTOMATED BUILD AND PACKAGING

The first thing you will need to automate is the process of turning developers’ source code into deployment-ready artifacts.

需要实现自动化的第一件事就是将开发人员的源代码转换为部署就绪构件的这个过程。
Though most software developers make use of tools such as Make, Ant, Maven, NuGet, npm, etc. to manage their builds and packaging, many teams still have manual steps that they need to carry out before they have artifacts that are ready for release.

虽然大多数软件开发人员使用诸如Make，Ant，Maven，NuGet，npm等工具来管理其构建和打包，但是许多团队在制作好准备发布的构件之前仍需要执行一些手动步骤。

These steps can represent a significant barrier to achieving continuous delivery. For instance, if you release every three months, manually building an installer is not too onerous. If you wish to release multiple times per day or week, however, it would be better if this task was fully and reliably automated.

这些步骤是实现持续交付的重大障碍的代表。 例如，如果每三个月发布一次，手动构建与安装就显得不那么繁杂。但是，如果您希望每天或每周发布多次，那么这个任务完全可靠地自动化会更合适。

AIM TO:Implement a single script or command that enables you to go from version controlled source code to a single deployment ready artifact.

实现单个脚本或命令，使您能够从版本控制的源代码转移到单个部署完成的构件上去。