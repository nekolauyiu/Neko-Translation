
Franken-algorithms: the deadly consequences of unpredictable code

弗兰克算法：不可预知代码的致命后果

The death of a woman hit by a self-driving car highlights an unfolding technological crisis, as code piled on code creates ‘a universe no one fully understands’

一名妇女被一辆无人驾驶汽车撞死，突显了正在发生的技术危机，无数代码创造了“一个无人能完全理解的世界”。

by Andrew Smith

The 18th of March 2018, was the day tech insiders had been dreading. That night, a new moon added almost no light to a poorly lit four-lane road in Tempe, Arizona, as a specially adapted Uber Volvo XC90 detected an object ahead.Part of the modern gold rush to develop self-driving vehicles, the SUV had been driving autonomously, with no input from its human backup driver, for 19 minutes. An array of radar and light-emitting lidar sensors allowed onboard algorithms to calculate that, given their host vehicle’s steady speed of 43mph, the object was six seconds away – assuming it remained stationary. But objects in roads seldom remain stationary, so more algorithms crawled a database of recognizable mechanical and biological entities, searching for a fit from which this one’s likely behavior could be inferred.

2018年3月18日，是一个令科技业内人士感到担心的一天。那天晚上，一轮暗淡的新月照在亚利桑那周洲坦佩市的四车道公路上，一辆特别改装过的优步沃尔沃XC90侦测到前方的一个物体。作为现代淘金者开发无人驾驶汽车的一部分，这辆SUN一直自动行驶——并没有输入人类的备用驱动程序——了19分钟。雷达和光发射雷达传感器组阵列允许机载算法去计算数据，并且控制车速稳定地保持时速43英里，假设物体保持静止——它距离汽车大概有六秒。但是物体在雷达上很少能保持静止，所以更多的算法会在能识别机械和生物体的数据库中进行缓慢查询，搜索一个适合的方法去推断可能发生的行为。

At first the computer drew a blank; seconds later, it decided it was dealing with another car, expecting it to drive away and require no special action. Only at the last second was a clear identification found – a woman with a bike, shopping bags hanging confusingly from handlebars, doubtless assuming the Volvo would route around her as any ordinary vehicle would. Barred from taking evasive action on its own, the computer abruptly handed control back to its human master, but the master wasn’t paying attention. Elaine Herzberg, aged 49, was struck and killed, leaving more reflective members of the tech community with two uncomfortable questions: was this algorithmic tragedy inevitable? And how used to such incidents would we, should we, be prepared to get?
在最后一秒才清楚地识别物体——一名骑着自行车的妇女，购物袋挂在车把上，深信沃尔沃会会像其它汽车那样绕开她。由于禁止计算机自行采取规避行为，它突然把汽车的控制权交还给人类主人，但是主人并未留意。49岁的伊莱恩赫兹伯格，被撞死了，给科技社区的技术反思者留下了两个令人不适的问题：这种算法的悲剧是不可避免的吗？以及，对这样的事件，我们可以、或者应该准备好去面对它了吗？

“In some ways we’ve lost agency. When programs pass into code and code passes into algorithms and then algorithms start to create new algorithms, it gets farther and farther from human agency. Software is released into a code universe which no one can fully understand.”

“在某些方面我们已经失去了代理。当程序进入到代码，代码进入到算法以及算法开始建立一种新的算法，它已经离人类的代理已经很远了。算法把软件发送到没人能全部理解的代码世界中。”

If these words sound shocking, they should, not least because Ellen Ullman, in addition to having been a distinguished professional programmer since the 1970s, is one of the few people to write revealingly about the process of coding. There’s not much she doesn’t know about software in the wild.

假如这些话听起来让人感到震惊，那么也应该会如此。不仅是因为艾伦·乌曼，除了在70年代起就已经是一名杰出的专业程序员外，他还是为数不多对在编程过程中有启发见解的人之一。关于在自然环境下的软件，她了解得并不多。

“People say, ‘Well, what about Facebook – they create and use algorithms and they can change them.’ But that’s not how it works. They set the algorithms off and they learn and change and run themselves. Facebook intervene in their running periodically, but they really don’t control them. And particular programs don’t just run on their own, they call on libraries, deep operating systems and so on ...”

“人们会说，‘脸书如何——它们创建并且使用算法以及能够改变算法。’但这不是它的工作原理。公司启动算法，学习算法、改变算法并且运行算法。脸书会定期干预算法的运行，但公司并不能真正地控制它们。另外，特别程序不仅能独立地运行，它们还会调用库、深层操作系统等等。。。”

What is an algorithm?
什么是算法？

Few subjects are more constantly or fervidly discussed right now than algorithms. But what is an algorithm? In fact, the usage has changed in interesting ways since the rise of the internet – and search engines in particular – in the mid-1990s. At root, an algorithm is a small, simple thing; a rule used to automate the treatment of a piece of data. If a happens, then do b; if not, then do c. This is the “if/then/else” logic of classical computing. If a user claims to be 18, allow them into the website; if not, print “Sorry, you must be 18 to enter”. At core, computer programs are bundles of such algorithms. Recipes for treating data. On the micro level, nothing could be simpler. If computers appear to be performing magic, it’s because they are fast, not intelligent.

现在很少有像算法这样热门的话题供大家讨论。但是算法是什么？
事实上，自互联网兴起以来,其使用方法便以一种有趣的方式发生了变化——特别是90年代中期的搜索引擎。根本上而言，算法是小型、简单的事情；是一种过去常用于自动处理数据的规则。假如发生了a，就执行b；假如不是，就执行c。这就是“if/then/else”逻辑的经典计算。假如用户达到18岁，就可以浏览网页，假如不是，就会出现“抱歉，18岁以上方可进入”。本质而言，计算机程序就是这类算法的集成。处理数据的方法。在微观层面，没什么能够更简单的了。假如计算机看起来像在玩魔术，是因为它们运行得很快，而不是聪明。

Recent years have seen a more portentous and ambiguous meaning emerge, with the word “algorithm” taken to mean any large, complex decision-making software system; any means of taking an array of input – of data – and assessing it quickly, according to a given set of criteria (or “rules”). This has revolutionized areas of medicine, science, transport, communication, making it easy to understand the utopian view of computing that held sway for many years. Algorithms have made our lives better in myriad ways. 

近年来出现了更令人惊讶和模棱两可的意义，用“算法”一词来表示大型的、复杂的软件决策系统；以任何方式获得一组输入数据——以及迅速对其进行评估，根据指定的标准（或者“规则”）。这会彻底改变医学、科学、交通、通讯领域，使得更容易去理解多年来一直占主导地位的计算机乌托邦观点。算法在众多方面令我们生活得更好。

Only since 2016 has a more nuanced consideration of our new algorithmic reality begun to take shape. If we tend to discuss algorithms in almost biblical terms, as independent entities with lives of their own, it’s because we have been encouraged to think of them in this way. Corporations like Facebook and Google have sold and defended their algorithms on the promise of objectivity, an ability to weigh a set of conditions with mathematical detachment and absence of fuzzy emotion. No wonder such algorithmic decision-making has spread to the granting of loans/ bail/benefits/college places/job interviews and almost anything requiring choice. 

直到2016年我们才对新算法现实有了更细致的考虑，并将其成形。假如我们用圣经术语讨论算法，作为有其生命的独立实体，这是因为我们鼓励用这种方式去看待它们。像脸书和谷歌这样的公司已经出售和维护算法并保证其客观性，拥有用数学分离和客观理性衡量一组条件的能力。难怪这样的算法决策可以拓展到如：发放贷款、保释、收益、大学场景、工作面试以及所有需要选择的事情。


We no longer accept the sales pitch for this type of algorithm so meekly. In her 2016 book Weapons of Math Destruction, Cathy O’Neil, a former math prodigy who left Wall Street to teach and write and run the excellent mathbabe blog, demonstrated beyond question that, far from eradicating human biases, algorithms could magnify and entrench them. After all, software is written by overwhelmingly affluent white and Asian men – and it will inevitably reflect their assumptions (Google “racist soap dispenser” to see how this plays out in even mundane real-world situations).Bias doesn’t require malice to become harm, and unlike a human being, we can’t easily ask an algorithmic gatekeeper to explain its decision. O’Neil called for “algorithmic audits” of any systems directly affecting the public, a sensible idea that the tech industry will fight tooth and nail, because algorithms are what the companies sell; the last thing they will volunteer is transparency. 

我们已经不再接受这种类型算法的推销说辞。在2016年一本名为《数学杀伤性武器》——凯茜奥尼尔，一名前数学神童，离开了华尔街去教授、撰写和操作著名的”mathbabe”博客）——毫无疑问地指出，远未消除人类的偏见，算法能够放大并且巩固这种偏见。毕竟，软件大部分是由富裕的白人和亚洲人来编写—— 这将不可避免地反映他们的猜想（谷歌“种族主义的自动洗手液感应器”可以看到这在现实世界中是如何发生的。（racist soap dispense：指的是厕所里的自动洗手液感应器的一段视频，一个白人的手放在感应器下面，洗手泡沫就自动出来了，而换了黑人的手泡沫就没有出来，当黑人的手上面放了白色的厕纸时，洗手泡沫就出来了；这段话是暗示了尽管是软件，但是由于编写的人种的原因，有可能软件和算法也会出现种族歧视，译者注）偏见并不需要恶意便能产生伤害，同时不像人类，我们不能简单地问算法看门人，解释其决定。奥尼尔要求任何直接影响公众的算法进行“算法审计”，明智的想法就是技术行业将竭尽全力维护之，因为算法是公司的卖点；科技行业自愿做的最后一件事就是将其透明化。

The good news is that this battle is under way. The bad news is that it’s already looking quaint in relation to what comes next. So much attention has been focused on the distant promises and threats of artificial intelligence, AI, that almost no one has noticed us moving into a new phase of the algorithmic revolution that could be just as fraught and disorienting – with barely a question asked.

好消息是这场战争正在进行中。坏消息是，对于接下来发生的事情显得有些古怪。太多的关注集中在远期承诺上，以及人工智能的威胁，关于人工智能，几乎没什么人留意到我们已经进入到了一个算法革命的新阶段，这个算法革命也能让人感到担忧和困惑——几乎没人提出过问题。

The algorithms flagged by O’Neil and others are opaque but predictable: they do what they’ve been programmed to do. A skilled coder can in principle examine and challenge their underpinnings. Some of us dream of a citizen army to do this work, similar to the network of amateur astronomers who support professionals in that field. Legislation to enable this seems inevitable.

被奥尼尔和其他人标注的算法是不透明但是可以预测的：它们做被安排做的事情。一个熟练的程序员原则上能够审查和质疑其基础。我们当中某些人梦想着有一支公民队伍来做这件事，类似于网络上的业余天文学爱好者，他们支持某领域的权威。立法变得不可避免。


We might call these algorithms “dumb”, in the sense that they’re doing their jobs according to parameters defined by humans. The quality of result depends on the thought and skill with which they were programmed. At the other end of the spectrum is the more or less distant dream of human-like artificial general intelligence, or AGI. A properly intelligent machine would be able to question the quality of its own calculations, based on something like our own intuition (which we might think of as a broad accumulation of experience and knowledge). To put this into perspective, Google’s DeepMind division has been justly lauded for creating a program capable of mastering arcade games, starting with nothing more than an instruction to aim for the highest possible score. This technique is called “reinforcement learning” and works because a computer can play millions of games quickly in order to learn what generates points. Some call this form of ability “artificial narrow intelligence”, but here the word “intelligent” is being used much as Facebook uses “friend” – to imply something safer and better understood than it is. Why? Because the machine has no context for what it’s doing and can’t do anything else. Neither, crucially, can it transfer knowledge from one game to the next (so-called “transfer learning”), which makes it less generally intelligent than a toddler, or even a cuttlefish. We might as well call an oil derrick or an aphid “intelligent”. Computers are already vastly superior to us at certain specialized tasks, but the day they rival our general ability is probably some way off – if it ever happens. Human beings may not be best at much, but we’re second-best at an impressive range of things.

我们也许把这些算法称为“愚蠢”，某种意义上而言，它们根据人类定义的参数进行着自己的工作。这些工作结果的质量处决于它们被写作程序时的想法和技术。在光谱的另一端，有着或多或少类似人类的通用人工智能梦想，或者称为AGI。一个适当的智能机器能够质疑自己的计算质量，基于一些类似于我们自己的直觉力（可以把它看作是经验和知识的广泛积累）。从这个角度来看，谷歌的深度思维部门，由于创建了一个可以控制电玩的程序受到了应有的称赞，这个游戏是从一条指令开始，以尽可能高得分为目标。这个技术被称为“强化学习”，之所以有效，是因为一台电脑能够同时快速地启动游戏，为了了解什么是积分。一些人把这种能力称为“人工狭隘智能”，但这里的“智能”一词就像脸书使用“朋友“一词一样的广泛——暗指一些比其更安全以及更容易理解的东西。为什么？因为机器对其正在做和不能做的其它事没有参照。至关重要的是，也不能把知识从一个游戏转向另一个游戏（称之为“转换学习“），这使得机器还不如一个蹒跚学步的孩子一样的聪明，甚至还比不上墨鱼。我们不妨把油井架和蚜虫称为“智能“。计算机在某些特别的任务中已经远远地超越了我们，但是它们与我们抗衡的能力还很遥远——假如这一天会发生的话。人类也许不是最佳的对手，但我们在一系列令人印象深刻的事情上排名第二。

Here’s the problem. Between the “dumb” fixed algorithms and true AI lies the problematic halfway house we’ve already entered with scarcely a thought and almost no debate, much less agreement as to aims, ethics, safety, best practice. If the algorithms around us are not yet intelligent, meaning able to independently say “that calculation/course of action doesn’t look right: I’ll do it again”, they are nonetheless starting to learn from their environments. And once an algorithm is learning, we no longer know to any degree of certainty what its rules and parameters are. At which point we can’t be certain of how it will interact with other algorithms, the physical world, or us. Where the “dumb” fixed algorithms – complex, opaque and inured to real time monitoring as they can be – are in principle predictable and interrogable, these ones are not. After a time in the wild, we no longer know what they are: they have the potential to become erratic. We might be tempted to call these “frankenalgos” – though Mary Shelley couldn’t have made this up.

问题就在这里。“愚蠢“固定了算法和真正人工智能之间存在着一些——我们没有思考过和几乎没有讨论过——的悬而未定的问题，更不用说在目标、道德、安全和最佳实践中达成一致。假如我们身边的算法还不算聪明，意味着能独立地说“那个计算/行动过程看起来不太对：我再重复一次”，尽管如此，它们还是从自身的环境中开始学习。一旦算法开始学习，我们不再清楚地知道规则和参数是什么。我们不能确定它如何与其它算法、物质世界以及和我们是如何做互动的。那些“愚蠢”固定算法——复杂、不透明以及适应实时监控——原则上可以预测和查询的，这些算法却不是。在自然环境中一段时间后，我们不再了解它们是什么：它们可能变得不太稳定。我们也许把其称之为“科学狂人”——虽然玛丽·雪莱不可能编造出来。

原文：https://www.theguardian.com/technology/2018/aug/29/coding-algorithms-frankenalgos-program-danger?CMP=twt_a-technology_b-gdntech


相关报导：https://www.theguardian.com/technology/2018/mar/19/uber-self-driving-car-kills-woman-arizona-tempe

https://www.theguardian.com/technology/2018/may/08/ubers-self-driving-car-saw-the-pedestrian-but-didnt-swerve-report

