#  漏洞预警 | PrestaShop tshirtecommerce SQL注入漏洞   
浅安  浅安安全   2025-06-02 23:00  
  
**0x00 漏洞编号**  
- # CVE-2023-27637  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
PrestaShop是一款功能丰富、基于PHP和MySQL的开源电子商务平台，而PrestaShop tshirtecommerce是基于PrestaShop搭建的，专注于T恤销售的在线商店系统，具有界面友好、易于上手、支持多语言多货币等特点，能帮助商家快速创建和管理T恤销售的在线业务。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SV9NDND6X8IXxCMkeiatzSt6CYJZmjGBJc8KuwlyQEL6GqHQmqjk1ekoOT2libyhCx9Lf8CPmYBbibtA/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
  
CVE-2023-27637  
  
漏洞类型：  
SQL注入  
  
**影响：**  
窃取敏感信息  
  
**简述：**  
PrestaShop tshirtecommerce的/module/tshirtecommerce/designer接口存在SQL注入漏洞，未经身份验证的攻击者可以通过该漏洞获取数据库敏感信息。  
  
**0x04 影响版本**  
- PrestaShop tshirtecommerce 2.1.4  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://tshirtecommerce.com/  
  
  
  
