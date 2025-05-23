#  警惕！APT组织利用ESET软件漏洞悄然植入恶意软件   
原创 紫队  紫队安全研究   2025-04-25 04:00  
  
**大家好，我是紫队安全研究。建议大家把公众号“紫队安全研究”设为星标，否则可能就无法及时看到啦！因为公众号只对常读和星标的公众号才能大图推送。操作方法：先点击上面的“紫队安全研究”，然后点击右上角的【...】,然后点击【设为星标】即可。**  
  
****  
![](https://mmbiz.qpic.cn/mmbiz_png/sUKKZDdVP8TjYUGo1k7aIkAfBqtHKcnUQQ2FR0uZa26jMCmbEsJVnh8RxYfn8N3Kl2tR28InfJf0CjP5icph8eA/640?wx_fmt=png&from=appmsg "")  
  
  
在网络安全的战场上，APT（高级持续性威胁）组织不断寻找各种机会发动攻击，给企业和机构的信息安全带来了严重威胁。近期，卡巴斯基的研究人员发现，至少有一个APT组织利用ESET软件中的漏洞，成功绕过安全措施，悄然执行恶意软件，这一事件再次敲响了网络安全的警钟。  
  
  
APT组织ToddyCat的恶意行径  
  
被追踪为ToddyCat的APT组织，利用了ESET软件中的一个漏洞（CVE-2024-11859）来实施恶意操作。这个漏洞属于DLL搜索顺序劫持问题，拥有管理员权限的攻击者可以利用它加载恶意动态链接库并执行其中的代码。  
  
  
早在2024年初，卡巴斯基的研究人员在调查ToddyCat APT组织的攻击活动时，发现了一种此前从未见过的C++工具——TCESB。该工具旨在绕过设备上安装的保护和监控工具，悄然执行有效载荷。卡巴斯基发布的报告中提到：“在ToddyCat的攻击中，这是一种全新的手段，它能在不被察觉的情况下，避开设备上的防护与监控工具，执行有效载荷。”  
  
  
对TCESB的DLL库进行静态分析后发现，其导出的所有函数均从系统文件version.dll（版本检查和文件安装库）中导入同名函数。这表明攻击者采用了DLL代理技术（劫持执行流，T1574）来运行恶意代码。通过这种技术，恶意DLL导出合法DLL的所有函数，但并不实际实现这些函数，而是将对这些函数的调用重定向到原始DLL。这样一来，加载恶意库的应用程序将继续正常工作，而恶意代码则在该应用程序的后台运行。  
  
  
ESET软件漏洞被利用的过程  
  
研究人员进一步发现，ESET的命令行扫描器（ecls）在加载version.dll时存在安全隐患。它会先在当前目录中检查该文件，然后才在系统目录中搜索，这就导致恶意的version.dll（即TCESB）有机会被加载，从而实现了隐秘的有效载荷执行。卡巴斯基将这个被追踪为CVE-2024-11859的漏洞报告给了ESET，ESET也在2025年1月对其进行了修复。  
  
  
报告中指出：“动态分析显示，扫描器在加载系统库version.dll时不安全，首先在当前目录中查找该文件，然后才在系统目录中搜索。这可能导致恶意DLL库被加载，从而构成漏洞。”  
  
  
TCESB恶意软件的复杂运作机制  
  
TCESB恶意软件采用了“自带漏洞驱动程序”（BYOVD）技术来躲避检测。它通过设备管理器安装存在漏洞的戴尔驱动程序（CVE-2021-36276）。之后，它会等待特定的有效载荷文件，使用文件中嵌入的AES - 128密钥对其进行解密，并从内存中执行该文件。这个工具会详细记录其活动，并且支持像kesp和ecore这样无扩展名的加密有效载荷。  
  
  
ESET的应对举措  
  
ESET在2025年1月针对CVE-2024-11859漏洞发布了安全公告：“在安装了受影响的ESET产品的系统上，攻击者可以将恶意动态链接库植入特定文件夹，并通过运行ESET命令行扫描器来执行其内容，因为扫描器会加载植入的库，而不是预期的系统库。”不过需要注意的是，“这种技术本身并不会提升权限，攻击者首先需要拥有管理员权限才能实施这种攻击。”  
  
  
此次事件提醒我们，网络安全形势日益严峻，即使是知名的安全软件也可能成为攻击目标。企业和机构需要密切关注软件漏洞信息，及时进行安全更新和防护措施的优化。同时，安全软件厂商也应不断提升产品的安全性和漏洞修复能力，共同应对来自APT组织等网络威胁的挑战，守护网络空间的安全与稳定。   
  
****  
**加入知识星球，可继续阅读**  
  
**一、"全球高级持续威胁：网络世界的隐形战争"，总共26章，为你带来体系化认识APT，欢迎感兴趣的朋友****入圈交流。**  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/sUKKZDdVP8RRAic0GwkHmSw2QZes8kK1AfysU8oPBib56yJpTWxmMuHRQBk3DHtibEASDuO7FTia8jIpeYtMFicBy5A/640?wx_fmt=jpeg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/sUKKZDdVP8Sm53HIUuI9RNR5Vpk1TWmpt3dw7icrMOJchapl0qTHsxVnXHyicBmV2kNlgpt3WLGLgdBJKrWiaUGicw/640?wx_fmt=png&from=appmsg "")  
  
**二、"Deep****Seek：APT攻击模拟的新利器"，为你带来APT攻击的新思路。**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sUKKZDdVP8SmEmOb6eVreW81Qh8DCAQvT2jLpI7JoYFWHibP6wCCI2AicqKAgbc4GzoAafviavpdxGjBqGrs1nlibQ/640?wx_fmt=png&from=appmsg "")  
  
  
**喜欢文章的朋友动动发财手点赞、转发、赞赏，你的每一次认可，都是我继续前进的动力。**  
  
  
  
  
