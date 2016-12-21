##编写git commit message的七条建议

Git提交信息格式的7条优良规范

>Keep in mind: 
[This](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) [has](http://www.git-scm.com/book/en/Distributed-Git-Contributing-to-a-Project#Commit-Guidelines)
[all](https://github.com/torvalds/subsurface/blob/master/README#L82-109)
[been](http://who-t.blogspot.co.at/2009/12/on-commit-messages.html)
[said](https://github.com/erlang/otp/wiki/writing-good-commit-messages)
[before](https://github.com/spring-projects/spring-framework/blob/30bce7/CONTRIBUTING.md#format-commit-messages).  
>请牢记：以下7条均经考验.

1. 采用空行将主题和详情分隔开
2. 限制主题长度在50个字符以内
3. 主题首字母大写
4. 主题不要用句号结尾
5. 主题使用祈使语气
6. 详情任何一行都不要超过72个字符
7. 利用详情去说明三点： 是什么 为什么 如何做

举个例子：

	Summarize changes in around 50 characters or less

	More detailed explanatory text, if necessary. Wrap it to about 72
	characters or so. In some contexts, the first line is treated as the
	subject of the commit and the rest of the text as the body. The
	blank line separating the summary from the body is critical (unless
	you omit the body entirely); various tools like `log`, `shortlog`
	and `rebase` can get confused if you run the two together.

	Explain the problem that this commit is solving. Focus on why you
	are making this change as opposed to how (the code explains that).
	Are there side effects or other unintuitive consequences of this
	change? Here's the place to explain them.

	Further paragraphs come after blank lines.

	 - Bullet points are okay, too

	 - Typically a hyphen or asterisk is used for the bullet, preceded
	   by a single space, with blank lines in between, but conventions
	   vary here

	If you use an issue tracker, put references to them at the bottom,
	like this:

	Resolves: #123
	See also: #456, #789

###1.采用空行将主题和详情分隔开

`git commit`的说明中写道：
>Though not required, it's a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, git-format-patch(1) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

首先，并不是每一次提交都需要主题和详情,有时候单行描述就可以了，尤其是当改动比较简单时，复杂的说明是不必要的。例如：

	Fix typo in introduction to user guide

瞧，无需多言。如果读者好奇到底修复了什么错误，那么她可以直接去看看改动本身就好，比如使用`git show`或者`git diff`或者`git log -p`。

如果你在git上想提交上面的说明信息，那么可以采用`-m`将其附加到`git commit`之上。

	$ git commit -m"Fix typo in introduction to user guide"

然而，当一个提交需要较多的解释和说明的时候，你需要书写一个详情，例如：

	Derezz the master control program

	MCP turned out to be evil and had become intent on world domination.
	This commit throws Tron's disc into MCP (causing its deresolution)
	and turns it back into a chess game.

这时候利用`-m`去提交就显得比较困难了，你需要一个合适的编辑器。如果你还没有安装一个在git命令行运行的编辑器，那么请阅读[Pro Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

任何情况下，将主题和详情分离对于浏览查看日志都是有益的。以下是完整的日志体：

	$ git log
	commit 42e769bdf4894310333942ffc5a15151222a87be
	Author: Kevin Flynn <kevin@flynnsarcade.com>
	Date:   Fri Jan 01 00:00:00 1982 -0200

	 Derezz the master control program

	 MCP turned out to be evil and had become intent on world domination.
	 This commit throws Tron's disc into MCP (causing its deresolution)
	 and turns it back into a chess game.

当采用`git log --oneline`，其只能打印出主题行：
	
	$ git log --oneline
	42e769 Derezz the master control program

或者`git shortlog`，它可以显示一系列的用户提交记录，但是也仅仅展示简洁的主题行信息：

	$ git shortlog
	Kevin Flynn (1):
      Derezz the master control program

	Alan Bradley (1):
      Introduce security program "Tron"

	Ed Dillinger (3):
      Rename chess program to "MCP"
      Modify chess program
      Upgrade chess program

	Walter Gibbs (1):
      Introduce protoype chess program

当然，在不同的git语境中主题行和详情的差异还有很多，但是如果没有空行将其隔开，它们中任何一个都将工作不正常。

###2.限制主题长度在50个字符以内
50个字符并不是一个硬性限制，只是一个规则要求。限制主题行的长度在这个范围内可以保证其可读性，并且要求作者认真去思考如何简洁的表述本次提交的内容。
>Tip: If you're having a hard time summarizing, you might be committing too many changes at once. Strive for atomic commits (a topic for a separate post).

GitHub的UI就充分考虑了这个约定，当你提交主题超过50字符的长度时会发出警告：

![1](http://i.imgur.com/zyBU2l6.png)
并且会将长于72字符的主题截断，并以省略号代替：
![2](http://i.imgur.com/27n9O8y.png)
因此，争取在50字符以内，记住72字节是硬性上限。

###3.主题首字母大写
这条规则和听上去一样简单。所有主题行请以大写字母开始，例如：

- Accelerate to 88 miles per hour

去代替：

- accelerate to 88 miles per hour

###4.主题不要用句号结尾
在主题行中末尾的标点是不需要的，毕竟，当你绞尽脑汁尽力缩减字数在50字符以内的时候空间就显得那么重要了不是么。例如：

- Open the pod bay doors

去代替：

- Open the pod bay doors.

###5.主题使用祈使语气
所谓祈使语气就是“像发布命令或指令一样的说和写”。下面是一些示例：

- 打扫你的房间
- 关上门
- 把垃圾丢出去

我们一开始说的7条规则也都是用祈使句来书写的（“将详情折叠到72字符以内”等等）。
也许有时候祈使句会听起来有点粗鲁，这也是我们为什么不经常使用它的原因。但是用它书写git提交的主题行说明却很合适，因为当你利用git命令创建提交的时候它自己本身也是采用祈使语气的。

例如当你使用`git merge`的时候创建的默认信息如下：
	
	Merge branch 'myfeature'
当使用`git revert`时：

	Revert "Add the thing with the stuff"

	This reverts commit cc87791524aedd593cff5a74532befe7ab69ce9d.

又或者点击GitHub拉取请求的“Merge”按钮时：

	Merge pull request #123 from someuser/somebranch

因此当你使用祈使语句书写你的提交信息的时候，你正是按照了git自己的内置约定而已。例如：

- 为了可读性重构子系统X
- 更新入门指南
- 移除弃用方法
- 发布版本1.0.0

一开始这样写可能会有点尴尬，因为我们常常都只使用陈述语气来陈述事实，这就是为什么提交信息常常被表述成这样：

- 修复了Y的bug
- 改变了X的行为

有时候提交信息被写成了内容的描述：

- 对损坏组件的一些修复
- 很棒的API新方法

为了避免迷惑，以下的简单原则能够保证每次都正确的进行描述。
###一个格式正确的git提交主题行应该能够用来组成以下句子：
- 如果被应用，这个提交将 \___________(在这里书写你的主题行) 
例如：
	
- 如果被应用，这个提交将为了可读性重构子系统X
- 如果被应用，这个提交将更新入门指南
- 如果被应用，这个提交将移除弃用方法
- 如果被应用，这个提交将发布版本1.0.0
- 如果被应用，这个提交将从user/branch分支合并拉取请求#123

注意，对于不是祈使语气的结构，这个句子将不再通顺：

- 如果被应用，这个提交将修复了Y的bug
- 如果被应用，这个提交将改变了X的行为
- 如果被应用，这个提交将对损坏组件的一些修复
- 如果被应用，这个提交将很棒的API新方法

>记住，只需要在主题行中使用祈使语态，在书写详情的时候不需要如此严格。

###6.详情任何一行都不要超过72个字符
git不会自动去折叠文字。因此当你书写提交信息的详情时，请记得右对齐，然后手动对文字换行。
我们建议在72字符的位置进行换行，这样git可以在保证所有内容都在80字符范围内的情况下有足够的缩进空间。
对此一个好的文本编辑器能够帮助你。例如可以很轻松的对Vim进行设置，让其在你书写git提交的时候自动进行每72字符一次的换行。然而，传统的IDEs对于提交信息时候文字换行的智能支持表现得非常弱（虽然在最近的版本中， IntelliJ IDEA的表现终于让人比较满意了）。

###7.利用详情去说明三点： 是什么 为什么 如何做
这个[比特币核心代码](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6)的提交记录是解释改变了什么和为什么改变的一个很好的例子：

	commit eb0b56b19017ab5c16c745e6da39c53126924ed6
	Author: Pieter Wuille <pieter.wuille@gmail.com>
	Date:   Fri Aug 1 22:57:55 2014 +0200

	   Simplify serialize.h's exception handling

	   Remove the 'state' and 'exceptmask' from serialize.h's stream
	   implementations, as well as related methods.

	   As exceptmask always included 'failbit', and setstate was always
	   called with bits = failbit, all it did was immediately raise an
	   exception. Get rid of those variables, and replace the setstate
	   with direct exception throwing (which also removes some dead
	   code).

	   As a result, good() is never reached after a failure (there are
	   only 2 calls, one of which is in tests), and can just be replaced
	   by !eof().

	   fail(), clear(n) and exceptions() are just never called. Delete
	them.
	
看看`full diff`，想想因为作者提供的这个提交说明，为他的同伴和未来的代码提交者节约了多少时间。如果他没有这么做，那么有些信息可能永远的无法找回了。
通常，你可以忽略如何进行修改的细节，因为代码通常是不言自明的（如果代码过于复杂以至于需要进行必要的解释，这就需要进行代码注释了）。无论如何，请将重点放在说明你为什么要进行修改，请说明修改前的运行状态（以及存在的问题），当前的运行状态和你为什么选择该方式进行修改。

也许未来作为维护者的你会感谢做了以上这些工作的自己的。

##Tips
### 学习并热爱命令行，远离IDE
因为许多原因比如git subcommands这些命令，总是希望可以拥抱命令行。Git极具威力，IDE显然也是，但侧重在不同的方向上，我每天都使用IDE(IntelliJ IDEA)也使用一些额外的选择(Eclipse)，但是我从未见过有IDE将Git集成比命令行本身更强大的。（建立在你了解Git命令）

Git与IDE集成毫无疑问是极具意义的，比如使用 git rm删除某一个文件，或者给某个文件重命名的时候。但如果你尝试通过IDE去使用commit,merge,rebase,分析日志的时候，IDE所提供的那些便捷都将不复存在。

一旦当你想要 <del> 控制雷电</del> 学会使用Git的全部能力的时候，那命令行就是你唯一的选择。

记住无论你使用的是Base或者是Zshell，tab的自动补全都能够提示你那些子命令和切换。


## 阅读《Read Pro Git》
[《The Pro Git book》](http://git-scm.com/book) 已经有线上免费版本，非常棒，尝试下吧。

> 原文 [How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/)

**译者：** [misha913loki](https://github.com/misha913loki) [renlulu](https://github.com/renlulu) [yannxia](https://github.com/yannxia)
