#  漏洞预警 | 神州数码DCME-320出口网关任意文件读取漏洞   
浅安  浅安安全   2025-04-12 00:04  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 中危  
  
**0x02 漏洞概述**  
  
DCME-320是神州数码推出的一款多功能网络出口网关，集路由、交换、防火墙、VPN、流量管理等功能于一体，专为高流量、高并发网络环境设计。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SWuUXNbWxDXwAcpib7wlsy5UM01bJpnAooiaGH3XZh6ibssXA6kyNrCosKUgFicFSPgrjcFuNNlDw3icnA/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
任意文件读取  
  
**影响：**  
获取敏感信息  
  
****  
  
**简述：**  
神州数码DCME-320出口网关的  
/function/auth/user/black_list.php接口存在任意文件读取漏洞，未经身份验证的攻击者可以通过该漏洞读取服务器任意文件，从而获取大量敏感信息。  
  
**0x04 影响版本**  
- 神州数码DCME-320出口网关  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.digitalchina.com/  
  
  
  
