#  日本遭定向攻击！Ivanti ICS零日漏洞被利用部署新型恶意软件DslogdRAT   
 sec0nd安全   2025-04-26 02:26  
  
**“**  
 零日漏洞。**”**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/L369x9IF3yPA9bic9zzTydWv4XTTHH2NAiamMp8Kxsh4s2lukPuyuwnia3NiaHkiaU8a3JGFhLvNnYvtLvHTFAd91Rw/640?wx_fmt=png&from=appmsg "")  
  
      
![](https://mmbiz.qpic.cn/sz_mmbiz_png/L369x9IF3yPMwVHx9iaPDKDhBJiajRW2DIdq0Wxe7JcpgKDia3zMfgicaaD6Auwn6Q3GGm2vI0eNh1Qic6OUhHMjE7g/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
  
  
01  
  
—  
  
  
  
导语  
  
  
  2024年12月，日本多家组织遭遇针对Ivanti Connect Secure（ICS）VPN设备的定向攻击。攻击者利用高危零日漏洞  
**CVE-2025-0282**  
部署了新型远控木马  
**DslogdRAT**  
及隐蔽的网页后门。此漏洞被描述为“未经身份验证的栈缓冲区溢出漏洞”，攻击者可借此远程执行任意代码，进而控制目标网络并窃取敏感数据。  
#### 一.漏洞与攻击链分析  
1. **漏洞CVE-2025-0282的核心威胁**  
  
1. **无需身份验证**  
：攻击者无需登录凭证即可通过发送特制请求触发漏洞，直接获取系统控制权。  
  
1. **持久化攻击**  
：攻击者植入的恶意软件（如DslogdRAT）通过劫持系统升级进程、嵌入系统文件等手段，即使在设备修补后仍能存活。  
  
1. **攻击流程与恶意工具**  
  
1. **时间敏感型活动**  
：仅在每日8:00至20:00运行，伪装成正常流量以降低检测风险。  
  
1. **进程隔离技术**  
：主进程快速终止并创建子进程，规避基于单进程监控的安全软件。  
  
1. **加密通信**  
：与C2服务器的通信采用7字节轮换XOR密钥混淆数据，支持文件窃取、命令执行及代理功能。  
  
1. **初始入侵**  
：利用CVE-2025-0282部署基于Perl的网页后门，攻击者通过特定Cookie发送HTTP请求执行任意命令。  
  
1. **载荷扩展**  
：后门作为跳板，进一步下载DslogdRAT等恶意软件。该远控木马具备以下特征：  
  
1. **隐蔽与反取证手段**  
  
1. **日志清除**  
：删除内核消息、调试日志、系统事件及SELinux审计日志，掩盖入侵痕迹。  
  
1. **虚假升级界面**  
：恶意软件PHASEJAM伪造HTML进度条，误导管理员认为系统正在升级，实则阻止合法更新。  
  
#### 二.幕后黑手与攻击背景  
- **国家级APT嫌疑**  
：Mandiant与JPCERT分析指出，攻击者疑似与中国关联的  
**UNC5337**  
等黑客组织有关，其工具链包含SPAWN恶意软件家族（如SPAWNSNAIL、SPAWNMOLE）及新型变种DRYHOOK、PHASEJAM。  
  
- **间谍活动目标**  
：受攻击的VPN设备存储的会话数据、API密钥及证书可能已被窃取，威胁企业数据安全及供应链信任。  
  
#### 三.防御建议与应对措施  
1. **紧急修补漏洞**  
  
1. 立即升级Ivanti设备至官方修复版本（CVE-2025-0282补丁于2025年1月发布）。  
  
1. **入侵检测与响应**  
  
1. 检查系统日志异常清除记录，监控  
/home/webserver/htdocs/dana-na/cc/ccupdate.cgi  
等路径的异常文件。  
  
1. 部署流量分析工具，识别DslogdRAT的XOR加密通信特征。  
  
1. **强化安全策略**  
  
1. 启用多因素认证（MFA），限制VPN设备的暴露面。  
  
1. 定期审计系统进程与升级流程，防范恶意软件持久化。  
  
#### 总结  
  
     针对Ivanti产品的攻击活动持续升级。GreyNoise监测显示，全球范围内针对ICS设备的可疑扫描激增（过去24小时达270个IP），预示未来可能出现大规模利用。企业需保持高度警惕，避免成为下一目标。  
  
  
免责声明：  
### 本人所有文章均为技术分享，均用于防御为目的的记录，所有操作均在实验环境下进行，请勿用于其他用途，否则后果自负。  
  
第二十七条：任何个人和组织不得从事非法侵入他人网络、干扰他人网络正常功能、窃取网络数据等危害网络安全的活动；不得提供专门用于从事侵入网络、干扰网络正常功能及防护措施、窃取网络数据等危害网络安全活动的程序和工具；明知他人从事危害网络安全的活动，不得为其提供技术支持、广告推广、支付结算等帮助  
  
第十二条：  国家保护公民、法人和其他组织依法使用网络的权利，促进网络接入普及，提升网络服务水平，为社会提供安全、便利的网络服务，保障网络信息依法有序自由流动。  
  
任何个人和组织使用网络应当遵守宪法法律，遵守公共秩序，尊重社会公德，不得危害网络安全，不得利用网络从事危害国家安全、荣誉和利益，煽动颠覆国家政权、推翻社会主义制度，煽动分裂国家、破坏国家统一，宣扬恐怖主义、极端主义，宣扬民族仇恨、民族歧视，传播暴力、淫秽色情信息，编造、传播虚假信息扰乱经济秩序和社会秩序，以及侵害他人名誉、隐私、知识产权和其他合法权益等活动。  
  
第十三条：  国家支持研究开发有利于未成年人健康成长的网络产品和服务，依法惩治利用网络从事危害未成年人身心健康的活动，为未成年人提供安全、健康的网络环境。  
  
  
  
  
  
  
