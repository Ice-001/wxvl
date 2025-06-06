#  Apple发布针对间谍软件式0day漏洞的紧急修复   
 安全客   2023-04-11 11:38  
  
Apple 发布了安全更新，以解决两个在野外被积极利用并针对 iPad、Mac 和 iPhone 的零日漏洞。  
  
这些漏洞被跟踪为CVE-2023-28205和CVE-2023-28206。根据 Apple安全公告，这些修复解决了谷歌威胁分析小组的 Clément Lecigne 和国际特赦组织安全实验室的 Donncha Ó Cearbhaill 发现的相同安全问题。  
  
最新的零日漏洞影响 iPhone 8 及更高版本、所有型号的 iPad Pro、第三代 iPad Air 及更高版本、第五代及更高版本的 iPad、第五代 iPad mini 及更高版本以及运行 macOS Ventura 的 Mac。  
  
“这些更新解决了两个不同的漏洞。重要的是，这两个漏洞不仅被描述为导致“任意代码执行”，而且被描述为‘被积极利用’，使它们成为零日漏洞，”安全研究员 Paul Ducklin Sophos，在博客文章中  
说。  
  
由于越界写入缺陷（指定为 CVE-2023-28206），在 Apple 的 IOSurfaceAccelerator 显示代码中，任何 iOS 应用程序都可以使用内核权限执行任意代码。  
  
Ducklin 说：“这个漏洞允许一个设下陷阱的本地应用程序将自己的流氓代码直接注入操作系统内核本身。” “内核代码执行错误不可避免地比应用程序级别的错误严重得多，因为内核负责管理整个系统的安全性，包括应用程序可以获得哪些权限，以及应用程序之间如何自由共享文件和数据。”  
  
越界写入是指在缓冲区开始之前或结束之后写入数据。“通常，这会导致数据损坏、崩溃或代码执行，”根据 Mitre 的常见弱点枚举  
网站  
。  
  
虽然苹果公司表示它“知道有关此问题可能已被积极利用的报告”，但它并未将此类漏洞利用归因于任何特定的网络犯罪或民族国家组织。  
  
另一个被追踪为 CVE-2023-28205 的漏洞存在于开源 Web 浏览器引擎 WebKit 中，该引擎在 iOS 和 Apple 设备上使用。WebKit 是 Apple 的网页内容显示子系统。它说，未打补丁的“恶意制作的网络内容可能会导致任意代码执行”。  
  
WebKit 漏洞可让攻击者控制用户的浏览器或任何使用 WebKit 呈现和显示 HTML 内容的应用程序。这些应用程序使用“WebKit 来向您展示网页预览、显示帮助文本，甚至只是生成一个好看的关于屏幕，”Ducklin 说。  
  
“Apple 自己的 Safari 浏览器使用 WebKit，使其直接容易受到 WebKit 错误的影响。此外，Apple 的 App Store 规则意味着 iPhone 和 iPad 上的所有浏览器都必须使用 WebKit，这使得这种错误成为移动 Apple 设备真正的跨浏览器问题， "达克林说。  
  
攻击者也有可能将这两个漏洞链接在一起 - 例如，利用 WebKit 并使用它来转向内核漏洞。  
  
内核级错误依赖于诱杀应用程序，由于其严格的 App Store围墙花园  
规则，这通常本身就是对 Apple 设备的威胁，这使得攻击者很难诱骗受害者安装流氓应用程序。  
  
Ducklin 表示，用户不会去市场并从二级或非官方来源安装应用程序，“即使你愿意，所以骗子需要先将他们的流氓应用程序偷偷带入 App Store，然后他们才能试图说服你进入安装它。但是当攻击者可以将远程破坏浏览器的错误与本地破坏内核的漏洞结合起来时，他们就可以完全避开 App Store 问题。”  
  
Ducklin 说，这个错误就是这种情况。跟踪为 CVE-2023-28205 的第一个错误允许攻击者远程接管手机的浏览器应用程序 - 此时攻击者拥有一个陷阱应用程序，他们可以使用该应用程序来利用跟踪为 CVE-2023-28206 的第二个错误来接管整个设备。  
  
“请记住，由于所有具有 Web 显示功能的 App Store 应用程序都需要使用 WebKit，因此即使您安装了第三方浏览器而不是 Safari，CVE-2023-28205 错误也会影响您，”Ducklin 补充道。  
  
