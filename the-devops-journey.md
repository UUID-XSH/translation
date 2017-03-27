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
• Waterfall. Organizations with a waterfall delivery process are living in the yesteryear of software development. They write, test and deliver code according to a sequential staccato rhythm. Their programming, Ops and quality assurance teams operate in silos, without collaborating with one another. They rely on manual processes, including manual testing.

• 瀑布式。曾经组织的软件交付常常利用瀑布式的交付过程。他们根据连续的staccato节奏编写、测试和交付代码。他们的编程，操作和质量保证团队在各自的孤岛上运作，没有彼此的协作。他们依赖人工过程，包括手动测试。

• Fast waterfall. Organizations that have implemented a certain degree of agility are in the fast waterfall stage. They still deliver software sequentially, but they take advantage of tools, such as GitHub, that introduce a certain degree of automation and scalability to their workflow. They also do some amount of automated testing, but constraints remain; for example, they may not take advantage of parallel testing, which adds a magnitude of efficiency to automated testing.

• 快速瀑布式。具有一定程度灵活性的组织处于快速瀑布式阶段。他们仍然顺序的交付软件，但他们利用了GitHub等工具，为他们的工作流程带来了一定程度的自动化和可扩展性。他们也执行一些自动化测试，但是约束依然存在。例如，他们可能不会合理利用并行测试的优势，尽管它们可以大大增加自动化测试的效率。