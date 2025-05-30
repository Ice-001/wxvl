#  全球政府邮件系统沦陷：黑客利用XSS漏洞发起大规模间谍活动   
 汇能云安全   2025-05-19 02:12  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/NSXvotEG4JwA6iae234BZTcVibeERibUSXzuEcPBm0KEMkcib3FBtLd1knaf9G3YvldE9VVDHbQX0dTbHob41hCqoQ/640?wx_fmt=jpeg&from=appmsg "")  
  
**5****月****19****日，星期一****，您好！中科汇能与您分享信息安全快讯：**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/NSXvotEG4JyCAxkw72ib3dJGWLuXiaQkxK2XlLONiclsibicxdtwTLDE3A4jKGqvp7PeiaTp7bV2RTTxXM5P9dB6tmtw/640?wx_fmt=gif&from=appmsg "")  
  
**01**  
  
  
  
**恶意NPM包利用Google日历作为C2中间人实施隐蔽攻击**  
  
  
Veracode威胁研究团队近日发现恶意NPM包"os-info-checker-es6"，巧妙利用Google日历平台作为命令与控制(C2)服务器连接的中间人，同时采用Unicode隐写术在不可见字符中隐藏恶意代码。  
  
该恶意包于2025年3月19日首次发布，最初仅执行打印操作系统详情等基本功能。截至5月15日，该包每周下载量约566次，同时作为四个其他包（skip-tot、vue-dev-serverr、vue-dummy和vue-bit）的依赖项，这些包被认为是同一攻击活动的组成部分。  
  
**利用Google日历作为初始执行和恶意C2连接之间的中介，攻击者能够利用Google日历的合法性绕过安全工具并掩盖包的恶意性质。Veracode已向NPM安全团队报告了该包，但截至5月15日，该包仍在NPM存储库中可用。**  
  
**02**  
  
**Interlock勒索软件组织瞄准国防承包商及其供应链发动攻击**  
  
  
危险的勒索软件组织Interlock正加大对美国国防承包商及其供应链的攻击力度，威胁敏感军事物流、知识产权和国家安全。该组织自2024年9月首次被发现以来，采用"大型猎物狩猎"策略，针对高价值组织实施双重勒索，即在加密系统前先窃取数据。  
  
**近期受害者包括美国致命弹药制造商AMTEC及其母公司National Defense Corporation(NDC)。Resecurity分析师确认，Interlock的数据泄露网站"Worldwide Secrets Blog"现已托管涉及美国国防部(DoD)、Raytheon和Thales等合同的机密文件。**  
  
Interlock的初始访问通常源于伪装成物流合作伙伴的钓鱼活动或被入侵的第三方供应商。一旦进入系统，攻击者部署自定义PowerShell脚本禁用安全工具，然后使用Mimikatz从lsass.exe中提取凭证，实现横向移动。攻击者创建名为"WindowsUpdateSync"的计划任务以维持持久性，执行连接到Interlock命令与控制(C2)服务器的Base64编码负载。该组织还利用企业虚拟专用网络和Microsoft Exchange服务器中的未修补漏洞。  
  
**03**  
  
**全球政府邮件系统沦陷：黑客利用XSS漏洞发起大规模间谍活动**  
  
  
**安全研究机构ESET近期披露了一项名为"RoundPress"的全球网络间谍活动，黑客利用多个零日和N日漏洞入侵政府机构的网络邮件服务器，窃取高价值情报。该活动疑由黑客组织APT28（又称"Fancy Bear"或"Sednit"）实施。**  
  
攻击通常以鱼叉式钓鱼邮件开始，内容引用时事新闻或政治事件，增加可信度。恶意JavaScript载荷嵌入在邮件HTML正文中，当受害者仅仅打开邮件查看时，就会触发网络邮件浏览器页面中的跨站脚本(XSS)漏洞，无需任何其他交互。该恶意脚本创建隐形输入字段，诱使浏览器或密码管理器自动填充受害者邮箱账户的存储凭证。此外，它还能读取DOM或发送HTTP请求，收集邮件内容、联系人、网络邮件设置、登录历史、双因素认证和密码等信息，然后通过HTTP POST请求将数据发送至硬编码的命令控制(C2)地址。  
  
**04**  
  
**警惕：无文件Remcos RAT攻击通过PowerShell实现"隐形入侵"**  
  
  
**Qualys威胁研究部门（TRU）近期发现一种复杂的网络攻击，攻击者利用PowerShell脚本在内存中直接运行恶意代码，偷偷安装Remcos远程访问木马（RAT），成功规避了传统杀毒软件的检测。**  
  
攻击始于用户打开ZIP压缩包"new-tax311.ZIP"中的快捷方式文件"new-tax311.lnk"。该文件通过Windows工具"mshta.exe"执行混淆的PowerShell脚本，随后尝试削弱Windows Defender防护，修改PowerShell安全设置，并通过Windows注册表确保Remcos RAT开机自启动。一旦感染，Remcos能执行键盘记录、剪贴板监控、屏幕截图、麦克风和网络摄像头录制等多种恶意操作，并窃取用户信息。  
  
Fortinet的FortiGuard Labs安全研究员总结，Remcos背后的攻击者正在改变策略，从利用CVE-2017-0199漏洞转向使用伪装成PDF图标的欺骗性LNK文件，诱使受害者执行恶意HTA文件。  
  
**05**  
  
**Coinbase遭遇重大数据泄露，公司损失或达4亿美元**  
  
  
**加密货币交易巨头Coinbase近日确认其系统遭到入侵，客户数据包括政府颁发的身份证件在内的敏感信息被盗。根据Coinbase向美国监管机构提交的法定文件，一名黑客本周告知Coinbase已获取客户账户信息，并要求支付赎金以换取不公开被盗数据。**  
  
被盗数据包括客户姓名、邮寄和电子邮件地址、电话号码、社会安全号码的最后四位数字、掩码银行账号和部分银行标识符，以及驾照和护照等政府颁发的身份证件。此外，账户余额数据、交易历史记录和一些公司内部文档也在此次泄露中被窃取。  
  
Coinbase表示，受影响的客户数量不到其970万月活用户的1%。该公司预计将花费约1.8亿至4亿美元用于事件修复和客户赔偿。为加强安全防御，Coinbase宣布将在美国境内设立新的支持中心，并强化其安全防御措施。  
  
**06**  
  
**Proofpoint将斥资10亿美元收购Hornetsecurity**  
  
  
**全球网络安全与合规领导者Proofpoint, Inc.日前宣布达成最终协议，将收购欧洲领先的AI驱动Microsoft 365(M365)安全、合规和数据保护服务提供商Hornetsecurity Group。据业内消息，此项交易价值约10亿美元。**  
  
这一战略性收购是Proofpoint推进其以人为中心的安全解决方案使命的重要一步，特别是通过为中小型企业服务的托管服务提供商(MSP)渠道。此次并购将Proofpoint受到85%财富100强企业信赖的企业级安全专业知识，与Hornetsecurity强大的MSP生态系统相结合，后者支持欧洲地区超过1.2万个渠道合作伙伴和12.5万家中小企业。  
  
交易预计将在2025年下半年完成。收购后，Proofpoint计划向全球MSP提供Hornetsecurity平台，Hornetsecurity将作为MSP和SMB客户的中心枢纽。  
  
**07**  
  
**Pwn2Own柏林首日战报：Windows 11和Red Hat Linux等多系统遭零日漏洞攻破**  
  
  
**计划在5月15日至17日举办的Pwn2Own柏林2025黑客竞赛首日，安全研究人员成功演示了针对Windows 11、Red Hat Linux和Oracle VirtualBox的零日漏洞利用，共获得26万美元奖金。**  
  
Red Hat Enterprise Linux for Workstations首先在本地权限提升类别中被攻破，DEVCORE研究团队的Pumpkin利用整数溢出漏洞获得2万美元奖金。Hyunwoo Kim和Wongi Lee也通过链接use-after-free和信息泄露漏洞获得了Red Hat Linux设备的root权限，但由于其中一个漏洞是N-day漏洞，导致了漏洞冲突。  
  
Pwn2Own柏林2025黑客竞赛聚焦企业技术并首次引入AI类别。参赛者将针对AI、网页浏览器、虚拟化等多个类别的完全修补产品进行攻击，有机会赢取超过100万美元的现金和奖品。  
  
  
**08**  
  
**黑客勾结员工窃取数据 上市加密货币交易所预估最高损失4亿美元**  
  
  
Coinbase Global周四（5月15日）宣布，拒绝向网络犯罪分子支付2000万美元的赎金要求。这些犯罪分子通过贿赂Coinbase海外客服代理窃取了用户敏感数据。据公司提交的监管文件估算，此事件可能导致1.8亿至4亿美元的损失，包括修复问题和赔偿客户的费用。  
  
**受此消息影响，Coinbase股价周四上午下跌超7%。此次数据泄露对以安全性著称的Coinbase构成重大打击。作为美国领先的加密货币交易所，Coinbase此前成功避免了困扰许多海外交易所的攻击和盗窃事件。**  
  
Coinbase表示，周日收到一封来自不明身份的电子邮件，声称获取了部分客户账户信息。调查显示，攻击者通过贿赂在美国以外地区从事支持工作的多个承包商或员工获得数据。泄露信息包括用户姓名、地址、电话号码、电子邮件地址、部分掩码的社保和银行账户号码、政府颁发的身份证件（如驾驶执照和护照）以及账户余额快照和交易历史等，但客户资金未被直接访问。  
  
**09**  
  
**美国FBI警报：AI语音深度伪造成为针对美国高官的新型网络威胁**  
  
  
美国联邦调查局(FBI)近日发布公共服务公告，警告网络犯罪分子正利用AI生成的语音深度伪造技术(audio deepfakes)针对美国官员发起语音钓鱼攻击，这类攻击自2025年4月开始出现。  
  
**"自2025年4月以来，恶意行为者冒充美国高级官员，针对包括现任或前任美国联邦或州政府高级官员及其联系人在内的个人发起攻击。如果您收到声称来自美国高级官员的信息，请勿假定其真实性。"FBI警告称。**  
  
攻击者通过发送短信和AI生成的语音信息（分别称为smishing和vishing技术）冒充美国高级官员，试图在获取个人账户访问权限前建立融洽关系。他们通常发送伪装成邀请切换至其他消息平台的恶意链接，一旦成功入侵官员账户，就能获取其他政府官员的联系信息，进而通过社会工程学手段冒充被入侵的官员，窃取敏感信息或诱骗目标转账。  
  
此次警告是FBI持续关注深度伪造技术威胁的最新动态。早在2021年3月，FBI就曾警告深度伪造技术将被广泛应用于"网络和外国影响力行动"。2024年4月，美国卫生与公众服务部(HHS)也警告网络犯罪分子利用AI语音克隆技术针对IT服务台发起社会工程学攻击。  
  
  
10  
  
**区块链安全标准委员会发布首批四项安全标准**  
  
  
**区块链安全标准委员会(BSSC)近日通过其官方网站发布了首批四项安全标准，标志着区块链生态系统向更安全、更可信方向迈出重要一步。这些标准旨在解决区块链安全的关键方面，提升数字资产的信任度和区块链网络的可靠性。**  
  
此次发布的四项标准包括：节点操作标准(NOS)、代币集成标准(TIS)、密钥管理标准(KMS)和通用安全与隐私标准(GSP)。节点操作标准定义了安全操作区块链节点的要求，重点关注节点在区块链网络中的完整性、弹性和安全参与；代币集成标准确保数字资产安全地集成到区块链生态系统中，涵盖资产治理和数字资产在区块链网络中可靠运行所需的技术配置；密钥管理标准提供了区块链环境中加密密钥安全处理和使用的最佳实践，包括区块提议、验证和钱包托管；通用安全与隐私标准则定义了区块链参与者应满足的基线风险管理、安全和隐私要求。  
  
随着区块链技术获得广泛应用，健全的安全措施变得越来越关键。这些标准的发布对于提升区块链生态系统的整体安全性具有重要意义，有望在减少安全漏洞和增强用户信任方面发挥关键作用。  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/NSXvotEG4JyCAxkw72ib3dJGWLuXiaQkxKHndkjicUt0VmAdF7lvibRRaLRQ1DQnURaPd51R4G5h5PkJicnoAwksVHA/640?wx_fmt=gif&from=appmsg "")  
  
信息来源：人民网 国家计算机网络应急技术处理协调中心 国家信息安全漏洞库 今日头条 360威胁情报中心 中科汇能GT攻防实验室 安全牛 E安全 安全客 NOSEC安全讯息平台 火绒安全 亚信安全 奇安信威胁情报中心 MACFEESy mantec白帽汇安全研究院 安全帮  卡巴斯基 安全内参 安全学习那些事 安全圈 黑客新闻 蚁景网安实验室 IT之家IT资讯 黑客新闻国外 天际友盟  
  
![](https://mmbiz.qpic.cn/mmbiz_png/NSXvotEG4Jy8LNPtUFy94c9CGG0ASQqK4WcEDppBOWoymg5KyMOPPy14tftLVEgIibMlwuvsTjffaicjlVtficB2A/640?wx_fmt=png&from=appmsg "")  
  
本文版权归原作者所有，如有侵权请联系我们及时删除  
  
  
