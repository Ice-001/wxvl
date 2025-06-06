#  OWASP 2025年十大漏洞   
铸盾安全  河南等级保护测评   2025-01-22 16:00  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rTibWNx9ARWnuoSW8vkOrDrWjBnQjSNrgbRjECWcYfzom2C3F55piaNUicf2InhPIHVic0iauIZXIhZUQ86s6X49plQ/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
开放 Web 应用程序安全项目 (OWASP) 发布了备受期待的 2025 年智能合约十大漏洞，这是一份全面的认识文件，旨在让 Web3 开发人员和安全团队掌握对抗智能合约中最关键  
漏洞  
的知识。  
  
随着去中心化金融 (DeFi) 和区块链技术的不断发展，强大的智能合约安全性的重要性从未如此明显。最新列表反映了不断发展的攻击媒介，并重点介绍了近年来最常被利用或发现的漏洞。  
  
OWASP 智能合约 Top 10  
是  
开发人员、审计人员和安全专业人员的重要资源，可提供对常见弱点和缓解策略的见解。  
  
它对其他 OWASP 项目（例如智能合约安全验证标准 (SCSVS) 和智能合约安全测试指南 (SCSTG)）进行了补充，为保护区块链生态系统提供了整体方法。  
## 2023 年至 2025 年的主要变化  
  
2025 年版根据现实世界中的事件和新兴趋势引入了更新的排名和新见解。值得注意的变化包括增加了价格预言机操纵和闪电贷攻击作为不同的类别，反映了它们在 DeFi 漏洞利用中的日益普遍。  
  
同时，早期版本中突出的时间戳依赖性和 Gas 限制问题等漏洞已被替换或集成到逻辑错误等更广泛的类别中。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rTibWNx9ARWnuoSW8vkOrDrWjBnQjSNrgwZEDbfgFNpnC8JC2YG3dY5IwJLZnGmHReS8BjRLLibJlyPRYkYz2qog/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
## OWASP 2025 年十大漏洞：  
1. **访问控制漏洞**  
  
1. **价格预言机操纵**  
  
1. **逻辑错误**  
  
1. **缺乏输入验证**  
  
1. **重入攻击**  
  
1. **未经检查的外部调用**  
  
1. **闪电贷攻击**  
  
1. **整数上溢和下溢**  
  
1. **不安全的随机性**  
  
1. **拒绝服务 (DoS) 攻击**  
  
## 主要漏洞的详细概述  
### SC01：访问控制漏洞  
  
访问控制缺陷仍然是智能合约财务损失的主要原因，仅 2024 年就造成了 9.532 亿美元的损失。这些漏洞是由于权限检查实施不当而发生的，允许未经授权的用户访问或修改关键功能或数据。一个显著的例子是 88mph 函数初始化错误，它允许攻击者重新初始化合约并获得管理权限。  
### SC02：价格预言机操纵  
  
操纵价格预言机（智能合约使用的外部数据馈送）可能会破坏协议的稳定性，导致财务损失或系统故障。攻击者经常利用设计不良的预言机机制来暂时抬高或压低资产价格。  
### SC03：逻辑错误  
  
当合约无法正确执行其预期功能时，就会出现业务逻辑漏洞。这些错误可能导致代币铸造不当、借贷协议存在缺陷或奖励分配不正确。  
### SC04：缺乏输入验证  
  
未能验证用户输入可能允许攻击者将恶意数据注入智能合约，从而导致意外行为或破坏合约逻辑。  
### SC05：重入攻击  
  
重入攻击利用了合约在完成自身状态更新之前调用外部函数的能力。这一经典漏洞在 2016 年的 DAO 黑客事件中被利用，该事件导致价值 7000 万美元的以太币被盗。  
### SC06：未经检查的外部调用  
  
当智能合约无法验证外部调用是否成功时，它们可能会对交易结果做出错误的假设。这可能会导致不一致或被恶意行为者利用。  
### SC07：闪电贷攻击  
  
闪电贷允许用户在单笔交易中无需抵押借入资金，但可被利用来操纵市场或耗尽流动性池。  
### SC08：整数溢出和下溢  
  
当计算超出数据类型限制时，就会出现算术错误，这可能使攻击者能够操纵余额或绕过限制。  
### SC09：不安全的随机性  
  
区块链的确定性使得生成安全随机性变得具有挑战性。可预测的随机性可能会危及彩票、代币分配或其他依赖随机结果的功能。  
### SC10：拒绝服务 (DoS) 攻击  
  
DoS 攻击针对智能合约中的资源密集型功能，通过耗尽 gas 限制或计算资源导致它们失去响应。  
## 现实世界的影响  
  
OWASP 智能合约 Top 10 是根据 SolidityScan 的 Web3HackHub 和 Immunefi 的加密损失报告等资源中记录的事件得出的。  
  
仅在 2024 年，就有 149 起记录在案的事件，这些事件是由于访问控制缺陷（9.53 亿美元）、逻辑错误（6300 万美元）和重入攻击（3500 万美元）等漏洞造成的，损失超过 14.2 亿美元。这些数字凸显了区块链开发中迫切需要强有力的安全实践。  
  
随着区块链技术的成熟，攻击者利用其漏洞的方法也在不断增加。2025 年 OWASP 智能合约 Top 10 为旨在保护去中心化生态系统免受不断演变的威胁的开发人员和安全团队提供了重要的路线图。  
  
通过遵守这些准则并将最佳实践融入到从设计到部署的每个开发阶段，Web3 项目可以增强其抵御潜在攻击的能力，同时培养用户和投资者之间的信任。  
  
  
