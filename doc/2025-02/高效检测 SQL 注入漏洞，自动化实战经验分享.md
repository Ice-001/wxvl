#  高效检测 SQL 注入漏洞，自动化实战经验分享   
 黑白之道   2025-02-22 01:30  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3xxicXNlTXLicwgPqvK8QgwnCr09iaSllrsXJLMkThiaHibEntZKkJiaicEd4ibWQxyn3gtAWbyGqtHVb0qqsHFC9jW3oQ/640?wx_fmt=gif "")  
  
在渗透测试项目中，经常会收集大量的接口信息，为了提高效率，通常会使用工具来完成自动化测试，针对大量接口的漏洞探测，xray 这方面做的非常不错，但对于 POST 请求，探测方式只能采用被动请求或逐个接口测试的方式。  
  
但是我在实际的工作中，需要针对大量 GET、POST 接口和参数做漏洞探测，而目前比较关注的是 SQL 注入漏洞的检测，所以基于 Xray 关于 SQL 注入检测的 payload，自己完成了一个自动检测 SQL 注入的工具。  
  
今天就来分享一下整个工具的检测逻辑，有兴趣的朋友可以加入信安之路知识星球，注册文库后即可下载源代码进行查看和使用。  
### 检测步骤  
  
整个测试过程分为五步：  
  
1、请求 URL 接口，判断其是否存在，避免做无用功  
  
2、基础判断，添加一些特殊字符（如单引号、双引号、括号等），判断页面是否发生变化，如果发生变化，且在页面中出现报错信息，则进一步使用报错注入的语句进行判断，是否存在漏洞，用到的 payload 如下：  
```
#判断报错的漏洞类型执行 payload 计算 1 的 md5 值 c4ca4238a0b923820dcc509a6f75849
error_mysql_payloads = ["extractvalue(1,concat(char(126),md5(1)))", "1/**/and/**/extractvalue(1,concat(char(126),md5(1)))", "'and/**/extractvalue(1,concat(char(126),md5(1)))and'",")/**/AND/**/extractvalue(1,concat(char(126),md5(1)))/**/IN/**/(1", "')/**/AND/**/extractvalue(1,concat(char(126),md5(1)))/**/IN/**/('a"]
error_mssql_payloads = ["convert(int,sys.fn_sqlvarbasetostr(HashBytes('MD5','1')))", "1/**/and/**/convert(int,sys.fn_sqlvarbasetostr(HashBytes('MD5','1')))","'and/**/convert(int,sys.fn_sqlvarbasetostr(HashBytes('MD5','1')))>'0", ")/**/and/**/convert(int,sys.fn_sqlvarbasetostr(HashBytes('MD5','1')))/**/in/**/(1", "')/**/and/**/convert(int,sys.fn_sqlvarbasetostr(HashBytes('MD5','1')))/**/in/**/('a"]
```  
  
3、针对数字型参数，通过减号验证漏洞是否存在，比如 ID 为 101-1 的结果是否与 ID 为 100 的结果一致，如果一致则认为该参数存在漏洞  
  
4、针对参数进行布尔注入验证，例如常用的 and 1=1 这样的参数，主要 payload 参考：  
```
#判断是否存在 bool 型注入，计算页面相似度
bool_common_payload = [["1/**/and+3=3", "1/**/and+3=6"], ["'and'c'='c", "'and'd'='f"], ["%'and'%'='", "%'and'%'='d"]]
```  
  
5、最后检查参数是否存在延时注入，通过判断延时的请求时间来判断是否存在注入，延时 payload 参考：  
```
time_common_payload =[["'and(select*from(select+sleep(6))a/**/union/**/select+1)='", "'and(select*from(select+sleep(1))a/**/union/**/select+1)='"], ["(select*from(select+sleep(6)union/**/select+1)a)", "(select*from(select+sleep(1)union/**/select+1)a)"],["/**/and(select+1)>0waitfor/**/delay'0:0:6'/**/", "/**/and(select+1)>0waitfor/**/delay'0:0:1'/**/"], ["'and(select+1)>0waitfor/**/delay'0:0:6", "'and(select+1)>0waitfor/**/delay'0:0:1"]]
```  
### 关键点  
  
在实现的过程中，最为关键的部分就是判断两次请求的页面是否一致，并且排除一些错误干扰，主要有以下几点：  
  
1、状态码判断，两次请求的状态码是否一致  
  
2、错误信息判断，根据报错的关键词进行匹配，比如：  
```
error_mysql_infolist = ["SQL syntax"]
error_mssql_infolist = ["Exception", "SQL Server", "80040e14", "引号不完整"]
error_access_infolist = ["Microsoft JET Database Engine"]
```  
  
3、页面相似度判断，计算两次请求的页面相似度，使用的是 github 上分享的相似度计算  
> https://github.com/SPuerBRead/HTMLSimilarity  
  
  
4、判断页面是否被 WAF 拦截，将 WAF 检测应用其中，针对存在 WAF 的接口，暂时跳过  
  
5、针对一些 PHP 的框架，参数中有些路径啥的无需探测，增加了黑名单参数关键词，比如：  
```
#黑名单参数名
black_parma = ["method", "mod", "s", "act", "Action", "a", "m", "c"]

```  
  
6、在参数中增加检测 payload 时，除了报错注入检测，其他检测不应该出现非正常响应，所以定义了一个黑名单响应码列表：  
```
black_code = [-1, 0, 404, 403, 500, 503, 405, 999]
```  
### 实战测试  
  
1、报错注入  
  
![image-20250218104104036](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6lpkCyH5k3ZGpnhRdVHdnX9KJA28ibk75NaLkJ93V7N9bOhlnicBSHibTJQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
程序逻辑设置，只要存在报错注入，且能执行 md5 函数，则跳出检测，因为可以百分之百确定存在注入漏洞。  
  
2、数字注入  
  
案例只检测出数字注入，没有检测出布尔注入，通常这两个是同时存在的，可能是 payload 的问题：  
  
![image-20250218102055907](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6lyUmcLRWWrNnYxRGWF7KARfZ2bId8J3vUlXAhzlzQbdYHnibibpuMJY4A/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
使用 SQLmap 进行验证：  
  
![image-20250218102129101](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6lPK3gd6VuSTNKDQ7qGf8SvdzlibLtzhby496GSyh2AV1EyfylkfG7cicQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
sqlmap 检测出了布尔注入，而脚本未检测出。  
  
3、布尔注入  
  
![image-20250218104814534](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6lAxFZRhVfDCwKgD2iaSuw2sIp3VM1VIWRmZe8LZGlpQc1e3AyXEFlJMA/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
使用 sqlmap 进行验证：  
  
![image-20250218104841004](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6liasxHSbl4iamVFDRGibcdTh1weYia9XO6JkQ2ZribfJGtnjXDn90VDoFyKQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
该案例只检测出目标存在布尔注入，跟脚本检测结果相同。  
  
4、时间盲注  
  
案例中只检测到时间盲注，为检测到其他注入方式：  
  
![image-20250218103736843](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6l8kxtEaXGiaia8z4m4Kyvu1l8a3FYzxMTuJw1CbpODlXGY0YEAnOU5dmg/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
使用 SQLmap 进行验证：  
  
![image-20250218103849149](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfficw0aC0aicGeRG072hp6N6lob7vl6pf0G7iaaAOQpfdicoiaicBw0TGFYib98om9jRMXicp6AptuD0qYMCQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
从结果上看，工具在验证方面还存在可以优化的部分，因为 sqlmap 检测出来了布尔注入，而该脚本未能实现。  
  
> **文章来源：信安之路**  
  
  
  
黑白之道发布、转载的文章中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途及盈利等目的，否则后果自行承担！  
  
如侵权请私聊我们删文  
  
  
**END**  
  
  
