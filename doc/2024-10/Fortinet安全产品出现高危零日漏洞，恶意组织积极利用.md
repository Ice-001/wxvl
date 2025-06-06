#  Fortinet安全产品出现高危零日漏洞，恶意组织积极利用   
看雪学苑  看雪学苑   2024-10-25 17:59  
  
网络安全公司Fortinet日前披露了自家软件产品FortiManager存在的一个关键零日漏洞，能够允许未经身份验证的远程攻击者通过特制请求执行任意代码或命令。目前该漏洞已在野外被积极利用。   
  
  
该漏洞被追踪为CVE-2024-47575，CVSS v3评分高达9.8，对多个版本的FortiManager以及FortiManager Cloud都有影响。Fortinet已经发布了一个补丁，并列出了用户可以采用的几种解决方法。   
  
  
根据Mandiant的报告，该漏洞已被利用以自动从FortiManager中泄露敏感文件，包括IP地址、凭证和托管设备配置。参与此漏洞调查的Mandiant表示，一个新的威胁组织UNC5820早在2024年6月27日就利用了FortiManager漏洞，泄露并暂存了FortiManager管理的FortiGate设备配置数据。   
  
  
受影响的版本包括：  
- FortiManager：版本7.6.0、7.4.0到7.4.4、7.2.0到7.2.7、7.0.0到7.0.12以及6.4.0到6.4.14。  
  
- FortiManager Cloud：  
版本7.4.1到7.4.4、7.2（所有版本）和7.0（所有版本）。  
   
  
Fortinet建议立即采取措施保护受影响的系统，包括：  
  
**升级：**  
安装FortiManager的最新补丁，如果使用FortiManager Cloud，请迁移到固定版本。  
  
  
**查看配置：**  
通过将配置与IoC检测之前获取的备份进行比较，确保配置未被篡改。  
  
  
**更改凭证：**  
更新所有托管设备的密码和用户敏感数据。  
  
  
**隔离受感染的系统：**  
将受感染的FortiManager系统与互联网隔离，并将其配置为离线模式，以便与新设置进行比较。   
  
  
此外，Fortinet还提供了几种指标来帮助组织检测是否受到了此漏洞的影响，包括：  
- 日志条目显示未注册设备被添加。  
  
- 特定IP地址与恶意活动相关：  
45.32.41.202、104.238.141.143、158.247.199.37和45.32.63.2。  
  
- 位于/tmp/.tm和/var/tmp/.tm目录中的文件。  
  
资讯来源：cybersecuritynews  
  
转载请注明出处和本文链接  
  
  
﹀  
  
﹀  
  
﹀  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Uia4617poZXP96fGaMPXib13V1bJ52yHq9ycD9Zv3WhiaRb2rKV6wghrNa4VyFR2wibBVNfZt3M5IuUiauQGHvxhQrA/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球分享**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球点赞**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球在看**  
  
****  
****  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/1UG7KPNHN8FxuBNT7e2ZEfQZgBuH2GkFjvK4tzErD5Q56kwaEL0N099icLfx1ZvVvqzcRG3oMtIXqUz5T9HYKicA/640?wx_fmt=gif "")  
  
戳  
“阅读原文  
”  
一起来充电吧！  
  
