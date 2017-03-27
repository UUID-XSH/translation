# THE DEVOPS JOURNEY: FROM WATERFALL TO CONTINUOUS DELIVERY
# DEVOPS演进：从瀑布式到持续交付

Implementing an agile, DevOps-centered workflow involves several distinct steps. In other words, the process required to evolve from traditional, “waterfall”-style development to continuous delivery of software is a journey, not something that organizations can implement all at once. 

实现一个敏捷的，以DevOps为中心的工作流涉及多个不同的步骤。换句话说，这个过程需要从传统慢慢演变而来，从“瀑布式”风格发展到软件的持续交付是一段历程，并非任何组织能够一蹴而就的。

While many organizations today have begun the DevOps journey by adopting some tools and methodologies that promote agility, a lack of fully automated testing often prevents them from completing it. The integration of effcient automated testing into work flows is therefore a crucial step for organizations seeking to achieve full agility and complete the DevOps journey.

虽然目前许多组织通过采用一些提高敏捷性的工具和方法来推进DevOps的进程，但自动化测试的缺乏通常会阻止他们完成这一目标。因此，在工作流中整合高效的自动化测试是任何组织实现完全敏捷并完成DevOps演进的关键步骤。

## EXECUTIVE SUMMARY 执行概要
DevOps. Agility. Continuous delivery. These are the buzzwords of the new software development and testing landscape. They describe some of the core values and processes that characterize an effcient, modern work flow.

DevOps、敏捷、持续交付，他们是新型软件开发和测试领域的流行语。 他们描述了一个高效的现代工作流的核心价值观和过程。
But they are also terms that can be misleading. That’s because they’re easy to conceptualize as qualities or values that an organization either has or does not have. In many common depictions, a company either “does” DevOps or it doesn’t. It’s agile or it’s not. It delivers its software continuously, or it adheres to archaic, staccato delivery schedules.

但是，他们也存在误导性，因为它们很容易被概念化为某个组织具有或不具有的特质或价值观。在常见的描述中，一家公司“采用”DevOps或者不采用，他们敏捷或者不敏捷，他们持续交付软件或者仍然坚持古老的staccato交付安排。
In reality, however, the DevOps landscape is not black and white. Adopting DevOps is a journey, not a choice that an organization opts to make or not, and implements all at once. That journey entails various steps and stages.

然而实际上，DevOps的涵盖范围并非总是那么黑白分明的。采用DevOps是一种演进，而不是一个组织能够直接做出的是否采用并立即实现的选择。这种演进需要多个步骤和阶段。
This whitepaper explains the DevOps journey and identifies the phases that the typical organization crosses as it adopts DevOps practices. It highlights the various degrees of agility associated with each stage, then discusses the barriers, including manual and sequential testing, that organizations have to overcome in order to progress further toward the endpoint of the DevOps journey, which means reaching full continuous delivery.

该白皮书解释了DevOps的演进，并定义了典型组织在实践DevOps时所需要经历的几个阶段。它强调了每个阶段的多种敏捷度，然后讨论了组织为了实现DevOps演进而必须克服的包括人力测试和顺序测试的障碍点，这意味着将达到全面的持续交付。## STAGES OF AGILITY  多阶段的灵敏度

Because modern software delivery chains involve so many tools and processes, it is best to think of the DevOps journey as a continuum. Organizations progress slowly from one phase to the next. They do not make the jump between phases overnight.

因为现代的软件交付链包含非常多的工具和过程，最好将DevOps演进视为一个连续的过程。组织是从一个阶段缓慢发展到另一个阶段，他们并不能在一夜之间发生突变。
That said, it is possible to identify four main stages of agility within the DevOps continuum. They include:

那即是说，我们能够定义DevOps持续发展过程中敏捷度的四个阶段：
- Waterfall. Organizations with a waterfall delivery process are living in the yesteryear of software development. They write, test and deliver code according to a sequential staccato rhythm. Their programming, Ops and quality assurance teams operate in silos, without collaborating with one another. They rely on manual processes, including manual testing.

- 瀑布式。曾经组织的软件交付常常利用瀑布式的交付过程。他们根据连续的staccato节奏编写、测试和交付代码。他们的编程，操作和质量保证团队在各自的孤岛上运作，没有彼此的协作。他们依赖人工过程，包括手动测试。

- Fast waterfall. Organizations that have implemented a certain degree of agility are in the fast waterfall stage. They still deliver software sequentially, but they take advantage of tools, such as GitHub, that introduce a certain degree of automation and scalability to their workflow. They also do some amount of automated testing, but constraints remain; for example, they may not take advantage of parallel testing, which adds a magnitude of efficiency to automated testing.

- 快速瀑布式。具有一定程度灵活性的组织处于快速瀑布式阶段。他们仍然顺序的交付软件，但他们利用了GitHub等工具，为他们的工作流程带来了一定程度的自动化和可扩展性。他们也执行一些自动化测试，但是约束依然存在。例如，他们可能不会合理利用并行测试的优势，尽管它们可以大大增加自动化测试的效率。


- 持续集成：Devops最为重要的一环就是使用时许集成平台，比如Jenkins，CircleCI，TeamCity。这些工具帮助我们执行大量构建软件的自动化过程，并且为开发者，运维，质检人员提供协作的框架，不管怎样，运行持续的集成服务并且消除了组织内的协作孤岛并不能保证全面的敏捷性。这需要一个完全自动化和优化的测试过程。它还涉及下一代部署方法，例如使用脚本化基础架构。

- 持续交付：这是DevOps演进的最初阶段。实现持续交付的组织采用了全自动化的开发流程，包括高频自动化测试。此外，他们的开发，运维和质量保证团队作为一个单一的团队，彼此保持同步。
TODO


再者，在实践中，这些阶段连续发生，互相的分界线是模糊的。此外，与每个阶段相关的具体特征将因组织而异。没有一个单一的工具或实践，这也是DevOps演进的特征之一。
一般来说，这些阶段是公司重要的里程碑，因为这是从传统的软件交付方式转化为持续交付的漫长旅程。


在DevOps范围内从一个阶段到另一个阶段的进展需要简单地识别交付链中尚未敏捷的部分，然后使其变得更加灵活。
在许多情况下，软件测试是挡住提高敏捷性的首要屏障。然而，DevOps团队在寻求提高敏捷性的时候，可能不会注意这种情况。事实上，许多组织可能相信他们已经完全敏捷，因为他们采用了其他DevOps工具，并忽视了自身测试的缺陷。

这种趋势并不奇怪。毕竟，迄今为止最流行的DevOps工具 - 持续集成服务，代码存储库，容器等 - 主要侧重于使源代码的生产，管理和交付更有效率。他们做的很少，以解决这个测试问题。

因此，公司通过采用与开发，运维和生产相关的工具而开始DevOps演进，可能会因缺乏自动测试而受到限制，即使他们不曾注意。但对于组织来说，提高测试效率是提高敏捷的一个显而易见的方式。


## 提高测试敏捷性

幸运的是，对于这样的组织，存在一些DevOps工具和方法来提高测试部分的效率。他们包括：

- 自动化测试：自动化测试是基于DevOps的交付流程的必要条件。手动测试在其他自动化工作上施加了巨大的瓶颈，因为它们要求在需要执行测试时，软件开发和交付过程暂停下来。手动测试也不可扩展，因为组织可以执行的手动测试的数量受到人员的限制。
- 基于云的测试：将测试转移到云端可以通过两个主要方式提高敏捷性。首先，它使测试更具可扩展性，因为获取额外的基于云的基础架构比扩展内部部署基础设施要容易得多，速度更快。第二，基于云的测试可以通过提供比本地可用的资源更多的资源来提高测试速度。
- 并行测试。并行测试比顺序测试更好，组织可以通过在可能的情况下并行运行而变得更加敏捷。并行测试意味着完整的测试套件更快完成。它们还防止了交付过程中的滞留，如果一个连续测试的问题延迟了其他测试，则会发生这种情况。
- 提前测试。即使组织已经采用自动化测试，也可以通过将测试前置来获得更多的敏捷性。换句话说，他们应该在开发和交付过程的早期进行测试，以便更快地发现问题。这将导致更快的总体交付，同时也增加了DevOps团队创建和测试新功能的能力，而不会延迟交付。


这些通过这些策略使测试更有效率，提高敏捷性，到目前为止还没有成为DevOps的中心部分。但是，它们构成了简单，低成本的增长方式，朝着全面持续交付的目标而努力。

## 自动化测试不是一个选择，而是必须
值得注意的是，为了敏捷性，自动化测试不仅仅是一个选择，而且绝对必要。即使组织已经构建了构成DevOps交付链的所有其他工具和实践，提高测试敏捷性会极大提升项目的敏捷性。
这意味着自动化测试以及上述优化测试的其他方法不仅仅是可选的，如果你希望最大化敏捷性。自动测试将是一个重要的成分。

** Web应用程序：从瀑布到连续交付 **

要了解为什么自动化测试对于实现全面灵活性至关重要，请考虑过去十年来一直在开发Web应用程序的组织的例子。该组织已经做了很多时间，以利用DevOps启发的工具和技术。它已将其代码移动到GitHub存储库。它采用了Jenkins的持续集成工具来自动化构建。它确保编写应用程序代码的程序员可以轻松地与测试它的质量检查团队以及部署它的IT操作员站进行通信。

