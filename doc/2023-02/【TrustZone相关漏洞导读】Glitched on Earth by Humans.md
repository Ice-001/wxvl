#  【TrustZone相关漏洞导读】Glitched on Earth by Humans   
 关键基础设施安全应急响应中心   2023-02-25 10:27  
  
- ![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10v8Vv9Pqnp3Awp03hibNByQem0PfX8XGAfYljagUP3Csbj7rhBkLwODA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
议题名称：Glitched on Earth by Humans: A Black-Box Security Evaluation of the SpaceX Starlink User Terminal (2022.08)[1]  
  
议题视频：https://www.youtube.com/watch?v=NXqLMmGwJm0 (2022.11)[2]  
  
议题作者：Lennert Wouters（伦纳特·沃特斯）  
  
破解目标：Starlink 天线锅盖（Version Before 2021.04.16） 本地获取 rootshell  
  
目标固件：未公开  
  
主要漏洞：未在安全启动过程中添加针对故障注入的防护代码  
  
利用方法：烧写flash替换固件，并进行电压故障注入攻击BL1的启动校验  
  
利用代码：修改的固件未公开，但自研的modchip(故障注入设备)公开了设计：KULeuven-COSIC/Starlink-FI[3]  
## 作者  
  
根据Linkedin资料显示，Lennert Wouters[4]（伦纳特·沃特斯 @LennertWo[5]），来自比利时，90年左右生人，目前30岁左右。本硕博均就读于比利时鲁汶大学（KU Leuven），目前为鲁汶大学 COSIC（Computer Security and Industrial Cryptography group）小组 [6]的安全研究员。虽然他目前工作于高校中，并且也有不少的论文成果[7]，但他并不局限于象牙塔派的抽象工作，同时也是一个真正的实战破解高手。除了本次让他登上BlackHat和DEF CON的星链破解工作以外，他还曾完成近场蓝牙解锁任意特斯拉汽车的破解，并以此登上 DEF CON 29 Car Hacking Village，议题为 My other car is your car[8]：  
> Youtube: COSIC researchers hack Tesla Model X key fob[9]  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10yxpwDEQrBeicNqcO2OQIK8ImFQUltyTUXu7RDr1kM32bkFsWmC9o2bA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
这个议题也来过中国：[GoGoHack 2022: 我的另一辆车是你的车——黑掉特斯拉ModelX无钥匙进入系统](https://mp.weixin.qq.com/s?__biz=Mzg4Mzc2MTY1Mg==&mid=2247483769&idx=1&sn=390e640918e778e2fe05afd1185dd20c&scene=21#wechat_redirect)  
  
，听下来就可以发现，他这次破解特斯拉的手法真是太朋克了！破解本身复用了特斯拉中控主板和车钥匙自身的软件逻辑，所以Lennert给他们都焊到一起，彻底复用硬件完成攻击。相当于拆一个自己的特斯拉，然后打别人的特斯拉，也可以说是用特斯拉打特斯拉：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa103EUrOzPjnApnGcrhS01Ycg4jJsn2lVibbpjVdSeFvZK667I1Fz2Yeng/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
其他中文报道：  
- • 比利时鲁汶大学科技大牛25€破解星链！曾90秒成功解锁特斯拉车锁密钥[10]  
  
- • 马斯克的Space X卫星被破解，25美元的工具就能入侵终端[11]  
  
- • 花170元黑掉马斯克星链终端，黑客公开自制工具[12]  
  
- • 特斯拉车钥匙又被黑，10秒钟就能开走Model Y[13]  
  
- • Tesla Model X无钥匙进入系统及固件升级漏洞[14]  
  
# 背景  
  
我们对星链可能有些陌生，星链主要有两个设备，一个锅盖天线，一个路由器，二者网线相连。锅放房顶，路由器放屋里，因此可把锅类比为光猫，锅接出来的网线连到路由器的wan口上即可。锅盖和路由器主系统都是Linux，其中星链二代路由器的固件全部开源：starlink-wifi-gen2[15]，并且开启了SSH，但其只支持私钥登录，私钥未公开，因此我们也无法登录路由器的shell（博客：Starlink: First Impressions[16]）。然而，开源的路由器并不是我们关注的目标，路由器也没什么特别的，破解的目标是锅盖天线。因为锅盖负责与星链的卫星进行通信，因此在本地拿到锅盖的root shell，是继续深入分析锅盖与卫星间通信链路的前提：  
- • 星链Starlink中文开箱测评 华语开箱星链第一人 安装配置测速[17]  
  
- • 太空Wifi有多快？STARLINK RV二代星链房车版野外速度测试/开箱/安装[18]  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10WOprC82KSIj7H834gF9W79FibjJRVCCBbtG9ibiasTcRkb2NKrBzkmwag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
Lennert Wouters通过替换固件并进行故障注入，最终在本地拿到了锅盖的root shell：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10JAXTiasGcvb603TNWBMR2EHIq2Z921XxQaibChReAgf6dRoZ7gWhFfnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
有了root shell，我们即可调试分析锅盖上的业务与驱动程序，对锅盖与卫星的通信协议进行逆向，最终达到破解协议、扩展攻击路径等更深入的破解工作：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10WcaSib7GUlwu6icvDA4vteGr5sll3gGNJ160vY27HMxMVkq9eDXlCnKA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
# 破解  
  
在早期版本的锅盖中，串口连上就是Linux shell，有输出，但是关闭了输入：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10oqJHCuBibF6vr8h4XGScPvCXYcIcZd0wqopA3QY4f5Zb3FZplqDBFdw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10svOFuIYkbj0cuSkU9KiadQCd1pT0qjD13FiaTckPwJC4LtmSPl2IHib0A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
Lennert Wouters的破解方法就是通过飞线烧写设备上的FLASH，将原始固件替换为打开串口输入的恶意固件。但由于安全启动的存在，恶意固件无法通过启动校验。因此还需要在启动过程进行电压故障注入，跳过固件校验的代码逻辑，才能获得设备的root shell。整个破解过程中最有特色的是他设计并开源了一个针对锅的故障注入设备，游戏机破解行当多称其为modchip：  
> 此modchip的PCB和软件操控代码均开源：KULeuven-COSIC/Starlink-FI[19]  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10lbxtVSYBMctTj5ICqpSPdkfEiaYqoa1E9tErKMVx0dOTqUMpzFu4Cgw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
此modchip的使用方式大概如下：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10S8rqe0nPwYPdfnhOGMn2tP1D4kGAEjwr1wGeXIiaKicWnmngFxxu3aLA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
既然作者给出了故障注入设备的PCB和软件操控代码，那这个破解好复现么？整个破解过程简单来说，就是利用故障注入攻击安全启动，进而启动可以拿到root shell恶意固件。然而这个的恶意固件是怎么修改出来的，Lennert Wouters基本没提，也没有公开出patch好的固件，因此这个破解不能无脑复现：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10iavZ5h1yrgL9Nqic8TNcAl8nf5IiaK19T9fyaqqiahs9iadpDIoENQFj8Ig/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
并且星链锅盖的固件甚至无法在网络上找到，只能从设备的FLASH上读出：  
> 只找到固件版本跟踪网站Starlink Firmware[20]，只有版本号，没有固件本体  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10pAwEAxMibY76rSbqkfoJcbHEcbRuc84p9ZAROfVHEpwBlcbzRqvRQPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
因此在没有设备的情况下，想要看清整个破解的全貌都是不可能的，所以我们就更难完整的理解并复现这个破解。  
# 复现准备  
> 要复现这个破解还有一些麻烦事，首先是要找到串口有输出的老版本锅盖。然后还有就是星链的业务遍布全球，不同国家版本的星链锅盖可能还会有细节上的不同，因此设备情况可能与破解示例有所差异，这或许也会给复现带来一些困惑。由于我还没缘分碰到设备，因此目前我只能以公开的TF-A和StarLink开源的u-boot为起点，做一些复现的准备工作。  
  
  
从原理上，整个破解的过程主要有两步，首先是先patch固件并烧写回FLASH，然后进行再故障注入。FLASH中固件的组织方式可以参考：SpaceX StarLink 星链卫星固件提取研究[21]，可见其安全启动的实现就是开源的TF-A（也即ATF），经过分析我们确认了他的具体打法：  
> 和checkm8、checkm30这种打手机root的方法一样，就是逐级patch  
  
- • Patch：从BL2内容证书开始patch，直至BL33  
  
- • Glitch：打BL1校验BL2内容证书  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10Eia2ILv0fpHPiaYWcCu1cicNhEdlyNicRxzMkdsc7P9wdthyLibOPjlWO8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
具体来说patch的主要有三部分：  
1. 1. BL2内容证书：SHA512的hash数据  
  
1. 2. BL2：校验BL33的代码位点  
  
1. 3. BL33：开启串口输入、默认启动shell  
  
### patch BL2  
> 目前无固件  
  
  
首先来说BL2固件的patch，因为BL2内容证书的patch依赖于BL2固件的SHA512。经过分析只要把auth_mod_verify_img[22]这个函数patch成返回0即可跳过所有校验：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10gJw3EI5UHzuNKPv7ialw1iasM3R959Yu4icr7bOjphTPlH588WM8ibQiaRw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
所以BL2往后的一切证书相关的校验数据都作废了：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10Eia2ILv0fpHPiaYWcCu1cicNhEdlyNicRxzMkdsc7P9wdthyLibOPjlWO8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
### patch BL2 内容证书  
> 目前无固件  
  
  
BL1校验BL2的过程主要有两步：  
1. 1. 首先校验BL2内容证书的签名  
  
1. 2. 然后校验BL2内容证书中的hash与BL2固件的hash是否一致  
  
当我们修改完BL2时，内容证书中的hash与BL2固件不一致，校验不通过，因此故障注入的位置有两种选择：  
1. 1. 修改BL2内容证书中的hash，故障注入BL1对BL2内容证书的签名校验  
  
1. 2. 不修改BL2内容证书，故障注入BL1对BL2固件的hash校验  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10T3ZmpVtibIOFic1icFWLZqQxt2zTlR6UytuJGIib8d9Gicn1ucgPhiawzG5g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
最终他根据测试结果发现，第一种选择，即修改BL2内容证书中的hash并通过故障注入跳过BL2内容证书的签名校验的成功率较高。因此按照第一种选择，我们需要在固件中找到这144字节的证书，并修改其中的hash数据为patch后BL2的SHA512，因此这个patch是在patch完BL2之后才能进行：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10lEWR6OAsyaKDxqRsPaE5FSlB8ZFibeJASia3jDqVXjfCWrU8C222ulEw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
### patch BL33（u-boot)  
> 锅的u-boot开源了，但是太奇怪了，spacex把修改过的u-boot放在仓库的release[23]中了，所以你直在目标仓库里搜相关头文件如spacex_catson_boot.h是搜不到的...  
  
  
用版本号为7左右的gcc：gcc-linaro-7.1.1-2017.05-x86_64_aarch64-linux-gnu.tar.xz[24]  
```
➜  wget https://github.com/SpaceExplorationTechnologies/u-boot/archive/refs/tags/sx_2020_11_25.tar.gz 
➜  tar -xvzf ./u-boot-sx_2020_11_25.tar.gz
➜  cd u-boot-sx_2020_11_25

➜  git init
➜  git add .
➜  git commit -m "1"

➜  make SPACEX_CATSON_UTERM_EMMC_defconfig 
➜  make ARCH=arm CROSS_COMPILE=/mnt/disk2/starlink/gcc-linaro-7.1.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu- -j `nproc`

➜  ls
api        drivers   Licenses     test                u-boot.map
arch       dts       MAINTAINERS  tools               u-boot-nodtb.bin
board      env       Makefile     u-boot              uboot.patch
cmd        examples  net          u-boot.bin          u-boot.srec
common     fs        post         u-boot.cfg          u-boot.sym
config.mk  include   README       u-boot.cfg.configs
configs    Kbuild    scripts      u-boot.dtb
disk       Kconfig   spacex       u-boot-dtb.bin
doc        lib       System.map   u-boot.lds
➜  file u-boot      
u-boot: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, with debug_info, not stripped
➜  strings ./u-boot | grep -i space
Warning: eth device name has a space!
SPACEX_CATSON_UTERM
FDT_ERR_NOSPACE
SpaceX telemetry console output
board/spacex/spacex_catson_uterm
```  
  
多说一句，这里u-boot的make配置文件SPACEX_CATSON_UTERM_EMMC_defconfig是diff出来的，在根目录的.azure-pipelines.yml文件中有原始u-boot版本信息：  
```
trini/u-boot-gitlab-ci-runner:bionic-20200112-21Feb2020
```  
- • 网上搜到：https://android.googlesource.com/platform/external/u-boot/+/0e80d597cfb3f674b65670c68c8375e002c95149/.gitlab-ci.yml  
  
- • 可以下载：https://android.googlesource.com/platform/external/u-boot/+archive/0e80d597cfb3f674b65670c68c8375e002c95149.tar.gz  
  
然后使用Linux下快速比较两个目录的不同[25]中提到的方法，diff目录发现新增文件，配置文件应该在u-boot的configs目录下，即可发现SPACEX_CATSON_UTERM_EMMC_defconfig（开始在configs目录搜starlink没搜到，后发现居然是spacex）  
```
➜  cat `which diffdir`
ppwd=`pwd`
cd $1; find ./ | sort >/tmp/file1
cd $ppwd
cd $2; find ./ | sort | diff /tmp/file1 -

➜  diffdir ./u-boot-sx_2020_11_25 ./u-boot-0e80d597cfb3f674b65670c68c8375e002c95149  | grep configs
< .//configs/SPACEX_CATSON_UTERM_EMMC_defconfig
< .//configs/SPACEX_CATSON_UTERM_defconfig
> .//configs/cf-x86_defconfig
< .//configs/cortina_presidio-asic-emmc_defconfig
< .//configs/p3450-0000_defconfig
< .//configs/socfpga_secu1_defconfig
> .//configs/woodburn_defconfig
> .//configs/woodburn_sd_defconfig
< .//configs/xilinx_zynq_virt_defconfig
> .//configs/zynq_virt_defconfig
< .//include/configs/SPACEX_CATSON_COMMON.h
< .//include/configs/SPACEX_CATSON_UTERM.h
< .//include/configs/SPACEX_STARLINK_COMMON.h
< .//include/configs/p3450-0000.h
< .//include/configs/socfpga_arria5_secu1.h
< .//include/configs/spacex_catson_boot.h
< .//include/configs/spacex_common.h
> .//include/configs/woodburn.h
> .//include/configs/woodburn_common.h
> .//include/configs/woodburn_sd.h
```  
  
现在统计其关键字：SPACEX、CATSON、starlink、gllcff  
#### 源码 patch  
  
分析源码通过类似bootargs的字符搜索，找到主要patch两个点：  
> 这些bootargs运行时以uboot环境变量的形式存在，其在源码中一般存在于某个头文件中的宏定义  
  
- • include/configs/spacex_catson_boot.h: 删掉 stdin=nulldev，开启串口输入  
  
- • include/configs/spacex_common.h: 启动程序rdinit=/usr/sbin/sxruntime_start换成/bin/sh，启动直接进shell  
  
  
- ```
diff -uprN ./u-boot-sx_2020_11_25_raw/include/configs/spacex_catson_boot.h ./u-boot-sx_2020_11_25/include/configs/spacex_catson_boot.h
--- ./u-boot-sx_2020_11_25_raw/include/configs/spacex_catson_boot.h     2020-12-15 10:28:45.000000000 +0800
+++ ./u-boot-sx_2020_11_25/include/configs/spacex_catson_boot.h 2022-10-14 13:05:42.000000000 +0800
@@ -191,8 +191,7 @@
        "setup_burn_memory=mw.q " __stringify(CATS_TERM_SCRATCH_ADDR) " 0x12345678aa640001 && " \
                "mw.l " __stringify(CATS_TERM_LOAD_ADDR) " 0xffffffff " __stringify(CATS_BOOTTERM1_SIZE) " && " \
                "mw.l " __stringify(CATS_TERM_TOC_SER_ADDR) " " __stringify(CATS_TERM_TOC_SER_VAL) "\0" \
-       "startkernel=unecc $kernel_load_addr $kernel_boot_addr && bootm $kernel_boot_addr${boot_type}\0" \
-       "stdin=nulldev\0"
+       "startkernel=unecc $kernel_load_addr $kernel_boot_addr && bootm $kernel_boot_addr${boot_type}\0"
 
 /* Needed for emmc but undefined by spacex_common.h */
 #ifdef CONFIG_CATSON_EMMC_ENABLED
diff -uprN ./u-boot-sx_2020_11_25_raw/include/configs/spacex_common.h ./u-boot-sx_2020_11_25/include/configs/spacex_common.h
--- ./u-boot-sx_2020_11_25_raw/include/configs/spacex_common.h  2020-12-15 10:28:45.000000000 +0800
+++ ./u-boot-sx_2020_11_25/include/configs/spacex_common.h      2022-10-14 13:06:22.000000000 +0800
@@ -209,7 +209,7 @@
  * with different serial implementations can not take them.
  */
 #define SPACEX_BOOTARGS                                                        \
-       "rdinit=/usr/sbin/sxruntime_start "                             \
+       "rdinit=/bin/sh "                               \
        "mtdoops.mtddev=mtdoops "                                       \
        "console=" SX_LINUX_CONSOLE "," __stringify(CONFIG_BAUDRATE) " "\
        SPACEX_KERNEL_VERBOSITY " "      
```  
  
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
-   
```
➜  pwd
/mnt/disk2/starlink/u-boot-sx_2020_11_25
➜  patch -p2 < ./uboot.patch
```  
```
```  
  
linux启动参数最终一般都会编译在u-boot本体里，运行时以uboot的环境变量的形式存在，也有在设备树里的：  
- • 环境变量：西湖论剑 2020 IoT闯关赛 赛后整理[26]  
  
- • 设备树：[2021西湖论剑IOT RW-WriteUp](https://mp.weixin.qq.com/s?__biz=MzUyMDEyNTkwNA==&mid=2247486241&idx=1&sn=176000328d9934755c8173e6b4f91e27&scene=21#wechat_redirect)  
  
  
所以也可以使用sed直接修改目标二进制的字符串，注意替换长度要一致，比较适合拿到真实固件后patch：  
```
➜  sed 's/\/usr\/sbin\/sxruntime_start/\/bin\/sh                  /g' ./u-boot > ./u-boot.tmp
➜  sed 's/stdin=nulldev/\x00            /g' ./u-boot.tmp >  u-boot.exp                       
➜  ls -al ./u-boot ./u-boot.exp 
-rwxrwxr-x 1 xuan xuan 3097272 10月 13 21:33 ./u-boot
-rw-rw-r-- 1 xuan xuan 3097272 10月 14 13:23 ./u-boot.exp
```  
  
```
```  
  
在加载id=6的镜像时进行故障注入，在TF-A中，6号image为BL2的内容证书：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10OfkkyIkibyGibJ6xSF4lH6NYMwadPibz6ib9eRnMvupUOsaofUbFicT9HYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
因此故障注入窗口就在目标串口输出Image id=6那句到Loading image id=1之间（目标设备比TF-A 2.6中间还多出一些打印）：  
```
NOTICE:  Booting Trusted Firmware
NOTICE:  BL1: v2.6(debug):v2.6-dirty
NOTICE:  BL1: Built : 13:54:11, Nov 15 2022
NOTICE:  BL1: RAM 0xe04e000 - 0xe058000
INFO:    Using crypto library 'mbed TLS'
INFO:    BL1: Loading BL2
INFO:    Loading image id=6 at address 0xe01b000
INFO:    Image id=6 loaded: 0xe01b000 - 0xe01b4b6
NOTICE:  [+] --------- Glitch: 6  ---------              // TRUSTED_BOOT_FW_CERT_ID
NOTICE:  [-] --------- Glitch: 6  ---------              // tb_fw.crt
INFO:    Loading image id=1 at address 0xe01b000
```  
  
  
所以可以确认故障注入就是在打BL2内容证书的签名校验：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eEicdLAdfSLIia4BNIIEia7V58fqeEufa10QbSvPylXvSXJEiauDhGX4CWicpIlgqfq2r9ibg5uZDPOeLjAPPiaEzP0rg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
### 电压故障注入细节  
  
在刷写完构造好的伪造固件后，就进入到破解的最后一步，电压故障注入。但这个电压故障注入到底怎么打？在哪打？打几个？只能等之后有缘分碰到设备再说啦！  
#### 引用链接：  
  
[1] Glitched on Earth by Humans: A Black-Box Security Evaluation of the SpaceX Starlink User Terminal (2022.08): https://www.blackhat.com/us-22/briefings/schedule/#glitched-on-earth-by-humans-a-black-box-security-evaluation-of-the-spacex-starlink-user-terminal-26982[2] https://www.youtube.com/watch?v=NXqLMmGwJm0 (2022.11): https://www.youtube.com/watch?v=NXqLMmGwJm0[3] KULeuven-COSIC/Starlink-FI: https://github.com/KULeuven-COSIC/Starlink-FI[4] Lennert Wouters: https://www.linkedin.com/in/lennert-wouters-1667bb124/[5] @LennertWo: https://twitter.com/LennertWo[6] COSIC（Computer Security and Industrial Cryptography group）小组 : https://www.esat.kuleuven.be/cosic/[7] 论文成果: https://www.esat.kuleuven.be/cosic/people/lennert-wouters/[8] My other car is your car: https://www.youtube.com/watch?v=36AvYW48JtQ[9] Youtube: COSIC researchers hack Tesla Model X key fob: https://www.youtube.com/watch?v=clrNuBb3myE[10] 比利时鲁汶大学科技大牛25€破解星链！曾90秒成功解锁特斯拉车锁密钥: https://www.sohu.com/a/576982812_121124414[11] 马斯克的Space X卫星被破解，25美元的工具就能入侵终端: https://zhuanlan.zhihu.com/p/555722185[12] 花170元黑掉马斯克星链终端，黑客公开自制工具: https://zhuanlan.zhihu.com/p/554381162[13] 特斯拉车钥匙又被黑，10秒钟就能开走Model Y: https://36kr.com/p/1744817892732548[14] Tesla Model X无钥匙进入系统及固件升级漏洞: https://www.freebuf.com/articles/network/343741.html[15] starlink-wifi-gen2: https://github.com/SpaceExplorationTechnologies/starlink-wifi-gen2[16] Starlink: First Impressions: https://www.bitsinflight.com/starlink-first-impressions/[17] 星链Starlink中文开箱测评 华语开箱星链第一人 安装配置测速: https://www.youtube.com/watch?v=dzgzeRGNIHc[18] 太空Wifi有多快？STARLINK RV二代星链房车版野外速度测试/开箱/安装: https://www.youtube.com/watch?v=zbWEB8Uk-rA[19] KULeuven-COSIC/Starlink-FI: https://github.com/KULeuven-COSIC/Starlink-FI[20] Starlink Firmware: https://starlinktrack.com/firmware/[21] SpaceX StarLink 星链卫星固件提取研究: https://www.4hou.com/posts/EW50[22] auth_mod_verify_img: https://elixir.bootlin.com/arm-trusted-firmware/v2.8/source/common/bl_common.c#L151[23] release: https://github.com/SpaceExplorationTechnologies/u-boot/releases/tag/sx_2020_11_25[24] gcc-linaro-7.1.1-2017.05-x86_64_aarch64-linux-gnu.tar.xz: https://releases.linaro.org/components/toolchain/binaries/7.1-2017.05/aarch64-linux-gnu/gcc-linaro-7.1.1-2017.05-x86_64_aarch64-linux-gnu.tar.xz[25] Linux下快速比较两个目录的不同: https://www.cnblogs.com/f-ck-need-u/p/9071033.html[26] 西湖论剑 2020 IoT闯关赛 赛后整理: https://xuanxuanblingbling.github.io/iot/2020/11/17/iot/  
  
  
  
原文来源：纽创信安  
  
“投稿联系方式：孙中豪 010-82992251   sunzhonghao@cert.org.cn”  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/iaz5iaQYxGogucKMiatGyfBHlfj74r3CyPxEBrV0oOOuHICibgHwtoIGayOIcmJCIsAn02z2yibtfQylib07asMqYAEw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
