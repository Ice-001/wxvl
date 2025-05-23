#  清华大学 | 通过攻击意图同步增强Web应用程序的漏洞检测效果   
原创 谢国峰  安全学术圈   2023-08-14 00:02  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFryZShUClb9SBfuKAGFibGdjicGcQicagr3qGUdNibrwLUFOnkm51py11vpIaKkg9EtvSOeO5nmRiaxZw/640?wx_fmt=png "")  
> 原文标题：Scanner++: Enhanced Vulnerability Detection of Web Applications with Attack Intent Synchronization原文作者：Zijing Yin, Yiwen Xu, Fuchen Ma, Haohao Gao, Lei Qiao, Yu Jiang原文链接：https://dl.acm.org/doi/abs/10.1145/3517036发表会议：ACM Transactions on Software Engineering and Methodology主题类型：漏洞检测笔记作者：谢国峰@Web攻击检测与追踪主编：黄诚@安全学术圈  
  
### 研究概述  
  
如今，部分web应用存在严重的安全漏洞，可能会导致敏感信息泄露、用户身份被盗，甚至整个服务器被攻击者控制，web漏洞扫描器是处理这些问题最常用的工具之一。随着web应用程序攻击面的增加和隐蔽漏洞的出现，市面上现有的扫描器在检测web应用程序漏洞方面面临着极大的挑战。每种扫描器都有各自的扫描策略，但它们的性能受到了目标应用程序的复杂攻击面和隐蔽漏洞的限制。从这个角度看，如何设计一种扫描过程更加稳健，能够更好地适用于各种现实目标的扫描框架具有很重要的实际意义。在此背景下，本文提出了一种对攻击意图进行同步的增强型web漏洞检测框架Scanner++。该框架通过将现有扫描器的能力与攻击意图同步相结合，提高了对复杂攻击面和隐蔽漏洞的检测能力。所设计的框架采用代理式架构和基于包的意图同步方法，在其中应用了攻击意图净化，攻击意图同步等机制，能够提供更全面和多样化的扫描过程。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFryZShUClb9SBfuKAGFibGdSF9LiaAoA4FQ8JxhtbZfmpfnnFoboNMPugVMoqdb2PtZJjjcwk8mwIw/640?wx_fmt=png "")  
图1 Scanner++扫描框架的整体框架  
  
图1给出了本文设计模型的具体框架示意图，包含基础扫描器、攻击意图净化过程、同步的攻击意图库、运行时意图同步等模块，其核心思想是通过攻击意图同步来增强现有扫描器的漏洞检测能力。基础扫描器发送请求数据包进行漏洞检测，并与攻击意图库进行同步，从而实现更全面和准确的漏洞检测。整体过程具体而言是基础扫描器发送请求数据包，攻击意图净化过程提取和精炼攻击面和攻击向量，并构建同步的攻击意图。实验结果表明，Scanner++能够提高漏洞检测的准确性和覆盖面，从而提高Web 用程序的安全性。  
### 贡献分析  
  
(1). 论文针对web漏洞扫描器通常在一个完整的闭环工作流程中运行，直接干涉其内部状态将严重损害其普遍性，在集成新的扫描器时需要大量的工作，提出了一个基于代理的体系结构，实现了一个不需要对基础扫描器进行任何修改，能够快速集成新的扫描器的架构。  
  
(2). 论文针对当多个工具扫描同一目标时，收集到的一些攻击向量的语义是等效的，直接同步可能导致漏洞检测的效率甚至低于隔离工作的扫描器的合并结果。提出了攻击意图净化机制，实现了重复信息的减少，只保留了有价值的攻击意图。  
  
(3). 论文针对每个扫描器只能使用自己的内容发现方法，针对发现的部分攻击面，它们只能各自根据自己的策略生成攻击向量，提出了一个攻击意图同步机制，实现了一个基于包的意图同步方法，使扫描器的测试覆盖率和攻击意图识别能力得到显著增强。  
### 代码分析  
  
代码链接：https://github.com/ScannerPlusPlus/ScannerPlusPlus  
  
(1)	代码使用类库分析，是否全为开源类库的集成  
  
Scanner++开源代码库中使用了python标准库中的pymysql库，提供了与MySQL数据库进行交互的功能；使用了python标准库中的subprocess库，用于在Python程序中创建和管理子进程；还使用了自定义的第三方模块proxier，并导入了NoAttackVectorProxier、NoRefinementProxier和NormalProxier这三个类；使用了开源的mitmproxy 库，它是一个用于中间人攻击代理的开源工具，用于拦截、修改和观察网络流量。  
  
因此，综上所述，不全是开源类库的集成。  
  
(2)	代码实现难度及工作量评估  
  
本框架的代码包含了前端界面部分，后端数据库配置部分，以及具体的代理模块，攻击意图同步模块。主要是基于mitmproxy模块实现代理服务器的功能，同时和数据库进行交互，进行攻击意图的同步，代码实现难度中等，工作量较大。  
  
(3)	代码关键实现的功能  
  
在代理模块中，根据配置文件中的设置，启动不同类型的扫描器，并在指定端口上启动代理程序，拦截和处理HTTP请求和响应。代理程序的处理逻辑根据配置中的代理模式进行相应的处理，包括正常代理、无攻击向量代理、无精细化处理代理和精细化处理代理。在具体扫描的时候，通过正则表达式识别特定的字符串模式，检查用户输入或数据传输中是否存在潜在的安全问题。  
  
攻击意图同步模块则是根据扫描器的检测点选择相应的攻击意图，指导扫描器的扫描过程。  
### 论文点评  
  
本文提出了Scanner++框架，采用代理式架构集成多个扫描器，通过引入攻击意图同步机制，结合现有扫描器的能力，提高了漏洞检测的准确性和覆盖面。攻击意图净化机制从基础扫描器发送的请求数据包中提取和精炼攻击面和攻击向量，构建一个同步的攻击意图库。实验结果表明，Scanner++相对于现有扫描器具有更高的漏洞检测能力和更全面的攻击面覆盖。  
  
然而，本文也存在一些潜在的不足之处。首先，对于攻击意图净化机制，本文仅仅是给出了攻击意图精炼的算法过程，却没有对攻击意图净化机制的准确性和可靠性等进行深入的分析和评估。之后可以使用一些数据集和测试用例，使用召回率、准确率和F1分数等作为评估指标，更全面地评估攻击意图净化机制的性能。同时，可以进行错误分析和敏感性分析来发现可能存在的问题和缺陷。  
  
其次，本文没有考虑攻击者可能采用的反制措施，比如攻击者可能通过发送虚假的请求数据包或者攻击意图来欺骗Scanner++的攻击意图同步机制，也可能通过漏洞利用或者拒绝服务攻击来干扰Scanner++的正常运行。因此，未来此框架可以引入一些安全机制，例如数字签名或者加密技术，来确保请求数据包和攻击意图的真实性和完整性。也可以引入异常检测和容错机制，来增强系统的鲁棒性。  
  
最后，本文的测试样例严重不足，本文的实验结果仅基于三个Web应用程序，可能无法完全代表所有Web应用程序的情况。因此，未来需要扩展实验范围，增加更多的Web应用程序和漏洞类型，进行更深入的分析和优化来提高Scanner++的性能和适用性，使其更加适用于不同类型的Web应用程序和漏洞。  
  
因此，未来的研究可以进一步探索攻击意图净化机制的准确性和可靠性，以及Scanner++的鲁棒性和安全性。此外，可以考虑扩展实验范围，以更全面地评估Scanner++的性能和适用性。  
### 论文信息  
- Zijing Yin, Yiwen Xu, Fuchen Ma, Haohao Gao, Lei Qiao, and Yu Jiang. 2023. Scanner++: Enhanced Vulnerability Detection of Web Applications with Attack Intent Synchronization. ACM Trans. Softw. Eng. Methodol. 32, 1, Article 7 (January 2023), 30 pages. https://doi.org/10.1145/3517036  
  
### 论文作者团队  
- 姜宇博士为清华大学软件学院副教授，博士生导师。主要方向为软件工程、物理信息融合系统，重点关注大数据程序，人工智能程序，量子程序等的安全测试分析。曾获计算机学会优秀博士学位论文、微软亚洲研究院铸星计划、中国科协青年人才托举计划、优秀青年科学基金以及阿里巴巴达摩院青橙奖。https://sites.google.com/site/jiangyu198964  
  
- 清华大学软件学院软件系统安全保障小组以“保障大规模软件系统安全”为使命，重点关注人工智能、区块链，工控系统，操作系统等领域的软件安全，致力于科研创新，利用深度学习、符号执行与模糊测试等技术，进行软件缺陷的自动挖掘与理解，实现大规模软件系统的安全测评。http://www.wingtecher.com  
  
  
  
> [安全学术圈招募队友-ing](http://mp.weixin.qq.com/s?__biz=MzU5MTM5MTQ2MA==&mid=2247484475&idx=1&sn=2c91c6a161d1c5bc3b424de3bccaaee0&chksm=fe2efbb0c95972a67513c3340c98e20c752ca06d8575838c1af65fc2d6ddebd7f486aa75f6c3&scene=21#wechat_redirect)  
 有兴趣加入学术圈的请联系   
**secdr#qq.com**  
  
  
  
