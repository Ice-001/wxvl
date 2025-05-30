#  如何有效遏制针对通达OA漏洞利用活动   
原创 debugeeker  奶牛安全   2023-06-05 10:17  
  
## 前言  
  
假设你是网络安全建设或防御负责人，相信针对通达 OA 的渗透测试一定是使你提心吊胆、夜不能寐且辗转反侧的原因之一。  
  
在此篇文章中，我们将给出可以遏制针对通达 OA 进行漏洞利用的策略，及时发现并降低它可能给终端安全带来的风险。  
## 关于通达 OA  
  
通达 OA（Office Anywhere），是一款适用于企事业单位的通用型网络办公软件，是北京通达信科科技有限公司旗下产品之一。  
  
北京通达信科科技有限公司隶属于中国兵器工业信息中心，简称通达信科。是一支以协同管理软件研发与实施、服务与咨询为主营业务的高科技团队，是国内协同管理软件行业里一家央企单位，中国协同管理软件的领军企业。  
## 漏洞公开情况  
  
为了更好了解产品的自身安全问题，通过对百度搜索信息的公开检索，我们调查了自 2020 年 1 月到 2023 年 6 月，有关通达 OA 的全部漏洞公开情况：  
- 2020 年 3 月  
  
1. 未授权文件上传漏洞  
  
1. 未授权文件包含漏洞  
  
- 2020 年 4 月  
  
1. 未授权任意用户伪造漏洞（CNVD-2020-25050）  
  
- 2020 年 8 月  
  
1. 未授权 SQL 注入漏洞  
  
1. 未授权任意文件删除漏洞（CNVD-2021-14827）  
  
- 2020 年 9 月  
  
1. 后台任意文件上传漏洞（CNVD-2020-57815）  
  
1. 后台 SQL 注入漏洞  
  
- 2021 年 3 月  
  
1. 任意在线用户登录凭据窃取漏洞  
  
- 2021 年 7 月  
  
1. 后台 SSRF 漏洞  
  
- 2022 年 2 月  
  
1. 后台 SQL 注入漏洞  
  
- 2022 年 4 月  
  
1. 未授权任意文件上传漏洞  
  
- 2022 年 7 月  
  
1. 未授权任意文件上传漏洞  
  
1. 后台任意文件上传漏洞  
  
- 2022 年 11 月  
  
1. 任意文件上传漏洞  
  
**上述 14 个漏洞均允许攻击者完全的控制受影响目标系统，全部为高危漏洞，影响范围极大。**  
## 利用方式分析  
  
通达 OA 安装时，会以 **SYSTEM** 权限向宿主系统部署并启动以下主要组件：  
- **MySQL**：提供数据存储功能  
  
- **Redis**：提供缓存功能  
  
- **Nginx**：提供网站功能  
  
- **PHP-CGI**：提供 PHP 代码执行功能  
  
主要组件运行关系如下：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0Dx3uicgTsSDXLUVteiaiaP95xcFVPCESZ9DBCwCF6nojJb7fypJzicN1jkg/640?wx_fmt=png "")  
  
结合历史漏洞类型和组件运行关系，我们总结的攻击者通用操作如下：  
1. 如果可以进行未授权的文件上传，且上传后的文件扩展名可控，则直接部署 PHP WebShell。  
  
1. 如果可以进行未授权的文件上传，但后缀名被限制且无法绕过，则上传带有后门 PHP 代码的图片或其他文件。  
  
1. 如果可以进行未授权的文件包含（include），则包含带有后门 PHP 代码的图片或其他文件（如日志文件），继而部署 PHP WebShell。  
  
1. 如果可以进行未授权的 SQL 注入漏洞利用，则利用 MySQL 在网站目录中部署 PHP WebShell 或执行系统命令。  
  
1. 如果可以得到认证后相关 API 的调用权限（登陆后），则利用登陆后相关漏洞。  
  
1. 如果存在后台 SQL 漏洞，利用方式参考 4。  
  
1. 如果存在后台文件上传类漏洞，利用方式参考 1、2、3。  
  
1. 如果后台存在 SSRF 漏洞，则利用 redis 在网站目录中部署 PHP WebShell。  
  
由于通达 OA 的主要组件默认以 SYSTEM 权限运行，所以，攻击者在获得 PHP 任意代码执行的方式（如 WebShell）后，目标系统就已经完全沦陷，攻击者可能以 SYSTEM 权限进行任何后续操作，包括但不限于系统命令执行、可疑二进制文件上传/运行、窃取系统敏感凭据、部署勒索病毒、搭建机密网络隧道和任何针对内部网络的恶意活动。  
  
不要过于担心，虽然这些漏洞利用和后续操作看起来很可怕，但它们产生的系统行为特征非常明显，我们可以把所有相关行为抽象成以下几种：  
1. 写入 PHP 脚本文件  
  
1. 写入可执行文件  
  
1. 执行了常用黑客命令  
  
1. 产生了不寻常的网络连接  
  
相关进程包括：php-cgi.exe、mysqld.exe 和 redis-server64.exe。  
  
这些行为看起来似乎与通达 OA 产品没有任何关系，因为它们完全是通用的渗透测试行为。  
## 检测与防御  
  
通过复杂之眼 EDR 提供的 MEQL 语法，根据上述行为，我们编写了以下示例策略：  
```
(EventType IN ("File Data Write", "File Data Modification") AND
FileExt IN ("php", "php5", "phtml")
OR
EventType = "Network Establish" AND NetworkFlow = "OUT" AND
NetworkRemotePort IN ("445", "135", "139", "3389", "22", "88")) AND
ProcessName IN ("php-cgi.exe", "mysqld.exe", "redis-server64.exe")
OR
EventType = "Process Creation" AND
(ProcessName IN ("cmd.exe", "powershell.exe", "pwsh.exe") OR
ProcessName IN ("whoami.exe", "systeminfo.exe", "ipconfig.exe",
"netstat.exe", "certutil.exe")) AND
ProcessParentName IN ("php-cgi.exe", "mysqld.exe", "redis-server64.exe")

```  
  
为了证实策略的有效性，我们在一台 Windows Server 2019 服务器上部署了通达 OA V11.2 与复杂之眼 EDR 客户端。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0DaxM88ANpwN0yTI5DUiciaEtaxLhtQc3KELcUicTE8jHqVSBDBqxh6gVpg/640?wx_fmt=png "")  
  
部署完成后，使用上述提到的 2020 年 4 月公开的漏洞，模拟攻击者对这个服务器系统进行渗透测试。  
1. 使用公开的利用程序一键上传 PHP WebShell![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0DW0TuAxGuwiaM1e3AMvzALW39q8GibV2ibcia2DLNK6ko6VVLtvygSosibyQ/640?wx_fmt=png "")  
  
  
1. 通过 PHP WebShell 执行系统命令，如 ipconfig 和 whoami 等。![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0DV6Q9ObwJReRYpf5nUibCjeJYAk1rjQZ7cY3u6Ala5bmj5eE7VUsBa2g/640?wx_fmt=png "")  
  
  
从漏洞利用的结果来看，名为 test.php 的 PHP WebShell 被成功部署，但没有命令的执行结果，表明系统命令并没有成功执行，这似乎对渗透测试人员来说不是一个好迹象。  
  
回到复杂之眼 EDR 后台，我们可以观察到目标系统产生了警报，并向安全人员发送了邮件提醒。  
- 安全警报邮件![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0Dvz4tdA8yfLMJdxsSjaE63fLUmLhP7ZWKo6rhQs1Gwds4FVwyt4jxnw/640?wx_fmt=png "")  
  
  
- 检测仪表盘![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0DdWzFRuDL3ibZv8ywetRcqkyTlky3AYdLWCYVCEMdiajp12LINYwfkHkA/640?wx_fmt=png "")  
  
  
- 威胁细节![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0DSp29yJkdcIVAfq0u5QOV3T9xzNDuBSkEynKqgibAgQQfPcEiaJwTs3uw/640?wx_fmt=png "")  
  
  
- 事件猎手
通过前面制定的策略，我们还发现了 PHP WebShell 部署的关键数字证据。![](https://mmbiz.qpic.cn/mmbiz_png/QXsgGBUcicbyFMLVrcCLyBWe6IGicALf0D7ibWpEfiay5OP9SibGHgQyYo19rsdwMfrkHKvsdBhjPicpNHS69ZqNEYyA/640?wx_fmt=png "")  
  
  
我们并没有针对通达 OA 进行特定的策略指定，而这场游戏却已经结束了。  
  
OA 市场未来会出现新的高危零日漏洞吗？它们可以逃避下一场由我们主导的猎杀游戏吗？  
## 关于复杂之眼  
  
**复杂之眼端点检测与响应系统（MultiEye EDR），依托领先的终端安全大数据技术，实现了与传统安全行业完全不同的，基于云的终端安全服务（SaaS EDR），并开展基于云的托管安全服务（MDR），快速地揭露潜藏在客户网络深处的恶意活动者，可以向客户提供网络安全高级威胁狩猎（APT Hunter）、快速应急响应、调查取证与高级别的网络安全咨询服务。**  
### 我们可以发现并挫败一切网络攻击，保护您的网络安全。  
#### 立刻申请试用：https://www.mistiny.com/index.php/trial-submit/  
# 请点一下右下角的“在看”，谢谢！！  
# 请帮忙点赞， 谢谢！！  
# 请帮忙转发，谢谢！！  
# 暗号: 318658  
  
  
  
