#  【首发复现】Apache Ofbiz XML-RPC 远程代码执行漏洞   
长亭应急响应  黑伞安全   2023-12-06 09:22  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FOh11C4BDicSMpKSibYGGsb6KqqBlUKAzEmtosaTIMKsyiaXBgaqFajdb8e0RhRia98Siamal0Q5eddshRJ2VgM3obw/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
Apache OFBiz是一个开源的企业资源规划（ERP）系统，提供了多种商业功能和模块。2023年12月，Apache官方发布新版本修复了OFBiz中的一个远程代码执行漏洞（CVE-2023-49070），攻击者能够利用该漏洞获取服务器权限。该漏洞利用无前置条件，影响范围较大，建议尽快修复漏洞。  
**漏洞描述**  
  
   
Description   
  
  
  
**0****1**  
  
漏洞成因2020年，为修复 CVE-2020-9496 增加权限校验，存在绕过。2021年，增加 Filter 用于拦截 XML-RPC 中的恶意请求，存在绕过。2023年四月，彻底删除 xmlrpc handler 以避免同类型的漏洞产生。尽管主分支在四月份已经移除了XML-RPC组件，但在Apache OFBiz的正式发布版本中，仅最新版本18.12.10彻底废除了XML-RPC功能。利用特征流量分析：攻击者利用这个漏洞时，会发送包含用户名和密码的 HTTP 请求到 XML-RPC 接口。在网络流量中，这可能表现为对 /webtools/control/xmlrpc 的异常访问请求。异常请求内容：利用 Filter 绕过机制的请求可能包含不寻常的 URI 结构，如使用分号或路径穿越技术（../）。特定的错误日志：在尝试进行反序列化攻击时，可能会在日志中观察到相关错误或异常信息，尤其是与 XML-RPC 组件相关的。漏洞影响远程代码执行风险：这个漏洞允许未授权的攻击者在服务器上执行任意代码，这可能导致数据泄露、系统被勒索或控制权被夺取等严重的安全威胁。数据安全和业务连续性：由于 Apache OFBiz 通常用于管理关键业务流程，此类攻击可能对业务操作产生重大影响，包括数据损坏和服务中断。影响范围 Affects 02Apache OFBiz < 18.12.10解决方案 Solution 03临时缓解方案禁用接口：如果XML-RPC接口不是业务必需的，可以考虑暂时禁用该接口，这也是官方补丁采取的做法。可以在服务器配置中禁用或重定向/control/xmlrpc路径，并通知相关团队和用户，说明暂时禁用的原因和期限。增强监控：通过增强对网络和应用级的监控，以便及时响应可疑活动。利用入侵检测系统（IDS）和/或入侵防御系统（IPS）/或Web应用防火墙（WAF）对网络流量进行监控。定期审查系统日志，寻找异常或可疑流量。网络ACL限制：通过网络层面的访问控制，限制对XML-RPC接口的访问，进一步减少潜在的攻击面。如果可能，暂时阻断所有外部访问XML-RPC接口的流量，直到安全漏洞得到彻底解决。升级修复方案Apache官方已发布安全更新，建议访问官网（https://ofbiz.apache.org/download.html）升级至最新版本。漏洞复现 Reproduction 04  
**产品支持**  
  
   
Support   
  
  
  
**0****5**  
云图：默认支持该产品的指纹识别，同时支持该漏洞的PoC原理检测。雷池：已发布规则升级包，支持检测该漏洞的利用行为。全悉：默认支持检测该漏洞的利用行为。  
  
  
**时间线**  
  
   
Timeline   
  
  
  
**0****6**  
12月4日 漏洞情报在互联网公开12月5日 长亭应急响应实验室复现漏洞12月5日 长亭安全应急响应中心发布通告  
参考资料：  
  
[1].https://ofbiz.apache.org/download.html  
  
[2].https://www.openwall.com/lists/oss-security/2023/12/04/2  
  
  
**长亭应急响应服务**  
  
  
  
  
全力进行产品升级  
  
及时将风险提示预案发送给客户  
  
检测业务是否收到此次漏洞影响  
  
请联系长亭应急团队  
  
7*24小时，守护您的安全  
  
  
第一时间找到我们：  
  
邮箱：support@chaitin.com  
  
应急响应热线：4000-327-707  
  
