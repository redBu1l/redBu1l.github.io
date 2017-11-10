---
title: "安全工具之信息收集续"
tagName: 基础篇
search: exclude
permalink: tag_navigation.html
sidebar: mydoc_sidebar
folder: tags
---
## 1、whois查询网站及服务器信息

如果知道目标的域名，你首先要做的就是通过Whois数据库查询域名的注册信息，Whois数据库是提供域名的注册人信息，包括联系方式，管理员名字，管理员邮箱等等，其中也包括DNS服务器的信息。
默认情况下，`Kali`已经安装了`Whois`。你只需要输入要查询的域名即可：
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/1.jpg)

利用以上收集到的邮箱、QQ、电话号码、姓名、以及服务商，可以针对性进行攻击，利用社工库进行查找相关管理员信息，另外也可以对相关DNS服务商进行渗透，查看是否有漏洞，利用第三方漏洞平台，查看相关漏洞。
## 2、Dig使用
可以使用dig命令对DNS服务器进行挖掘
Dig命令后面直接跟域名，回车即可
 ![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/2.jpg)
 
`【Dig常用选项】`
>-c选项，可以设置协议类型（class），包括IN(默认)、CH和HS。
>
>-f选项，dig支持从一个文件里读取内容进行批量查询，这个非常体贴和方便。文件的内容要求一行为一个查询请求。来个实际例子吧：
>
>-4和-6两个选项，用于设置仅适用哪一种作为查询包传输协议，分别对应着IPv4和IPv6。
>
>-t选项，用来设置查询类型，默认情况下是A，也可以设置MX等类型，来一个例子：
 
 ![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/3.jpg)
 
>-q选项，其实它本身是一个多余的选项，但是它在复杂的dig命令中又是那么的有用。-q选项可以显式设置你要查询的域名，这样可以避免和其他众多的参数、选项相混淆，提高了命令的可读性，来个例子：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/4.jpg)
> 
>-x选项，是逆向查询选项。可以查询IP地址到域名的映射关系。举一个例子：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/5.jpg)

`【跟踪dig全过程】`
dig非常著名的一个查询选项就是+trace，当使用这个查询选项后，dig会从根域查询一直跟踪直到查询到最终结果，并将整个过程信息输出出来

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/6.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/7.jpg)

`【精简dig输出】`
1. 使用+nocmd的话，可以节省输出dig版本信息。

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/8.jpg)

Dig可以用来查域传送漏洞,前面介绍了dig的使用，若将查询类型设定为axfr，就能得到域传送数据。这也是我们要用来测试DNS域传送泄露的命令.

## 3、Nslookup用法
`nslookup`是站长较为常用的工具之一，它甚至比同类工具dig的使用人数更多，原因是它的运行环境是windows，并且不需要我们再另外安装什么东西。dig是在`linux`环境里运行的命令，不过也可以在windows环境里使用，只是需要安装dig windows版本的程序。
`Nslookup`命令以两种方式运行:非交互式和交互式。

本文第一次提到“交互式”的概念，简单说明：交互式系统是指执行过程中允许用户输入数据和命令的系统。而非交互式系统，是指一旦开始运行，不需要人干预就可以自行结束的系统。因此，`nslookup`以非交互式方式运行，就是指运行后自行结束。而交互式，是指开始运行后，会要求使用者进一步输入数据和命令。

- 类型：
>A 地址记录
>
>AAAA 地址记录 
>
>AFSDB Andrew文件系统数据库服务器记录 
>
>ATMA ATM地址记录 
>
>CNAME 别名记录 
>
>HINFO 硬件配置记录，包括CPU、操作系统信息 
>
>ISDN 域名对应的ISDN号码 
>
>MB 存放指定邮箱的服务器 
>
>MG 邮件组记录 
>
>MINFO 邮件组和邮箱的信息记录 
>
>MR 改名的邮箱记录 
>
>MX 邮件服务器记录 
>
>NS 名字服务器记录 
>
>PTR 反向记录 
>
>RP 负责人记录 
>
>RT 路由穿透记录 
>
>SRV TCP服务器信息记录 
>
>TXT 域名对应的文本信息 
>
>X25 域名对应的X.25地址记录

- 例如：
1.设置类型为ns

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/9.jpg)
 
2.下面的例子查询baidu.com使用的DNS服务器名称:

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/10.jpg)
 
3.下面的例子展示如何查询baidu.com的邮件交换记录：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/11.jpg)
 
4.查看网站cname值。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/12.jpg)
 
5.查看邮件服务器记录（-qt=MX）
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/13.jpg)
 
6.同样nslookup也可以验证是否存在域传送漏洞，步骤如下：

>1) nslookup进入交互式模式
>
>2) Server 设置使用的DNS服务器
>
>3) ls命令列出某个域中的所有域名

## 4、fierce工具
***

在进行了基本域名收集以后，如果能通过主域名得到所有子域名信息，再通过子域名查询其对应的主机IP，这样我们能得到一个较为完整的信息。除了默认使用，我们还可以自己定义字典来进行域名爆破。
* 使用`fierce`工具，可以进行域名列表查询：`fierce -dns domainName`
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/14.jpg)
 
>输出结果表明，程序首先进行了域传送测试，域传送通过一条命令就能获取服务器上所有的域名信息。如果一次就能简单获取服务器上所有记录域名信息,就不再暴力破解。
>
>但从结果上看，“`Unsucessful in zone transfer`”,
域传送测试是失败了。接着执行暴力破解，测试的数量取决于字典中提供的字符串数量，上例中没有指定字典，在默认情况下在Kali中使用`/usr/share/fierce/hosts.txt`。一个内部网络的DNS域名服务器可以提供大量信息，这些信息可以在以后评估网络漏洞。
 
## 5、theHarvester的使用
***
`theHarvester`是一个社会工程学工具，它通过搜索引擎、PGP服务器以及SHODAN数据库收集用户的email，子域名，主机，雇员名，开放端口和banner信息。

>
>-d  服务器域名  
>
>-l  限制显示数目      
>
>-b  调用搜索引擎（baidu,google,bing,bingapi,pgp,linkedin,googleplus,jigsaw,all）
>
>-f  结果保存为HTML和XML文件
>
>-h  使用傻蛋数据库查询发现主机信息

- 实例1：
`theHarvester -d sec-redclub.com -l 100 -b baidu`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/15.jpg)
 
- 实战2：
  + 输出到html文件中，可以更清晰的看到搜索的网站信息的模型。
>theHarvester  -d sec-redclub.com -l 100 -b baidu -f myresults.html

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/16.jpg)
 
## 6、DNS枚举工具DNSenum
***
`DNSenum`是一款非常强大的域名信息收集工具。它能够通过谷歌或者字典文件猜
测可能存在的域名，并对一个网段进行反向查询。它不仅可以查询网站的主机地址
信息、域名服务器和邮件交换记录，还可以在域名服务器上执行axfr请求，然后通
过谷歌脚本得到扩展域名信息，提取子域名并查询，最后计算C类地址并执行whois
查询，执行反向查询，把地址段写入文件。本小节将介绍使用DNSenum工具检查
DNS枚举。在终端执行如下所示的命令：

 ![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/17.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/18.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/19.jpg)
 
 
输出的信息显示了DNS服务的详细信息。其中，包括主机地址、域名服务地址和邮
件服务地址，最后会尝试是否存在域传送漏洞。
使用DNSenum工具检查DNS枚举时，可以使用dnsenum的一些附加选项，如下所
示。

>
>1. --threads [number]：设置用户同时运行多个进程数。
>
>-r：允许用户启用递归查询。
>
>-d：允许用户设置WHOIS请求之间时间延迟数（单位为秒）。
>
>-o：允许用户指定输出位置。
>
>-w：允许用户启用WHOIS请求。

## 7、subDomainsbrute二级域名收集
***

二级域名是指顶级域名之下的域名，在国际顶级域名下，它是指域名注册人的网上名称；
在国家顶级域名下，它是表示注册企业类别的符号。
我国在国际互联网络信息中心（Inter NIC） 正式注册并运行的顶级域名是CN，这也是我国的一级域名。
在顶级域名之下，我国的二级域名又分为类别域名和行政区域名两类。
类别域名共7个，包括用于科研机构的ac；
国际通用域名com、top；
用于教育机构的edu；用于政府部门的gov；
用于互联网络信息中心和运行中心的net；
用于非盈利组织的org。
而行政区域名有34个，分别对应于我国各省、自治区和直辖市。（摘自百度百科）

 
>以上为工具默认参数，如果是新手，请直接跟主域名即可，不用进行其它设置。

![](https://github.com/redBu1l/Redclub-Launch/blob/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/20.jpg?raw=true)
 
`Python subDomainsbrute.py sec-redclub.com`
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/21.jpg)

就可以直接运行，等待结果，最后在工具文件夹下面存在txt文件，直接导入扫描工具就可以进行扫描了。

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/22.jpg)
 
## 8、layer子域名检测工具
***
layer子域名检测工具主要是windows一款二级域名检测工具，利用爆破形式。
工具作者：[http://www.cnseay.com/4193/](http://www.cnseay.com/4193/)

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/23.jpg)
 
域名对话框直接输入域名就可以进行扫描了，工具显示比较细致，有域名、解析ip、cnd列表、web服务器和网站状态，这些对于一个安全测试人员，非常重要。如下操作：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/24.jpg)


 
会显示大部分主要二级域名。



## 9、Nmap
***
`Nmap`是一个网络连接端口扫描软件，用来扫描网上电脑开放的网络连接端口。确定哪些服务运行在哪些连接端口，并且推断计算机运行哪个操作系统。它是网络管理员必用的软件之一，以及用以评估网络系统安全。
`功能:`

>
>1、	主机发现
>
>2、	端口扫描
>
>3、	版本侦测
>
>4、	OS侦测
>

几种部署方式：
* Kail集成环境
* 单独安装（使用yum工具直接安装）
* PentestBox环境
* Windows版等等
Nmap的参数和选项繁多，功能非常丰富。我们先来看一下Nmap的通用命令格式：
（详细教程及下载方式参见：[]()http://nmap.org/）
Nmap<扫描选项><扫描目标>

主机发现的原理与Ping命令类似，发送探测包到目标主机，如果收到回复，那么说明目标主机是开启的。Nmap支持十多种不同的主机探测方式，比如发送`ICMP ECHO/TIMESTAMP/NETMASK`报文、发送TCPSYN/ACK包、发送`SCTP INIT/COOKIE-ECHO`包，用户可以在不同的条件下灵活选用不同的方式来探测目标机。

`主机发现的基本用法`

>
>1. -sL: List Scan 列表扫描，仅将指定的目标的IP列举出来，不进行主机发现。 
>
>2. -sn: Ping Scan 只进行主机发现，不进行端口扫描。 
>
>3. -Pn: 将所有指定的主机视作开启的，跳过主机发现的过程。 
>
>4. -PS/PA/PU/PY[portlist]: 使用TCPSYN/ACK或SCTP INIT/ECHO方式进行发现。 
>
>5. -PE/PP/PM: 使用ICMP echo, timestamp, and netmask 请求包发现主机。
>
>6. -PO[protocollist]: 使用IP协议包探测对方主机是否开启。
>
>7. -sP:Ping 指定范围内的 IP 地址
>
>8. -n/-R: -n表示不进行DNS解析；-R表示总是进行DNS解析。 
>
>9. --dns-servers <serv1[,serv2],...>: 指定DNS服务器。 
>
>10. --system-dns: 指定使用系统的DNS服务器 
>
>11. --traceroute: 追踪每个路由节点 

扫描局域网`192.168.80.1/24`范围内哪些IP的主机是活动的。
- 命令如下：`nmap –sn 192.168.80.1/24`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/25.jpg)
 
>由图可知：`192.168.80.1`、`192.168.80.254`、`192.168.80.166`三台主机处于存活状态。

扫描局域网1192.168.80.100-2001范围内哪些IP的主机是活动的。

- 命令如下：1nmap –sP 192.168.80.100-2001

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/26.jpg)
 
端口扫描是Nmap最基本最核心的功能，用于确定目标主机的TCP/UDP端口的开放情况。默认情况下，Nmap会扫描1000个最有可能开放的TCP端口。Nmap通过探测将端口划分为6个状态：

>
>open：端口是开放的。
>
>closed：端口是关闭的。
>
>filtered：端口被防火墙IDS/IPS屏蔽，无法确定其状态。
>
>unfiltered：端口没有被屏蔽，但是否开放需要进一步确定。
>
>open|filtered：端口是开放的或被屏蔽。
>
>closed|filtered ：端口是关闭的或被屏蔽。

- 端口扫描方面非常强大，提供了很多的探测方式：

>
>TCP SYN scanning
>
>TCP connect scanning
>
>TCP ACK scanning
>
>TCP FIN/Xmas/NULL scanning
>
>UDP scanning
>
>其他方式
>
>-sS/sT/sA/sW/sM:指定使用 TCP SYN/Connect()/ACK/Window/Maimon scans的方式来对目标主机进行扫描。 

* -sU: 指定使用UDP扫描方式确定目标主机的UDP端口状况。 
* -sN/sF/sX: 指定使用`TCP Null`, `FIN`, `and Xmas` scans秘密扫描方式来协助探测对方的TCP端口状态。 
* --scanflags <flags>: 定制TCP包的flags。 
* -sI <zombiehost[:probeport]>: 指定使用idle scan方式来扫描目标主机（前提需要找到合适的zombie host）
* -sY/sZ: 使用SCTP INIT/COOKIE-ECHO来扫描SCTP协议端口的开放的情况。 
* -sO: 使用IP protocol 扫描确定目标机支持的协议类型。
* -b <FTP relay host>: 使用FTP bounce scan扫描方式 
* -p指定端口扫描

`在此，我们以主机192.168.80.166为例。命令如下：`

* `nmap -sS -p0-65535 -T4 192.168.80.166`
参数`-sS`表示使用TCP SYN方式扫描TCP端口；`-p0-65535`表示扫描所有端口；`-T4`表示时间级别配置4级；
 
 ![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/27.jpg)
 
>扫描特定端口是否开放

* `nmap -p21,80,445,3306 192.168.80.166`

 ![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/28.jpg)
 
`简要的介绍版本的侦测原理。版本侦测主要分为以下几个步骤：`
1. 首先检查`open`与`open|filtered`状态的端口是否在排除端口列表内。如果在排除列表，将该端口剔除。
2. 如果是TCP端口，尝试建立TCP连接。尝试等待片刻（通常6秒或更多，具体时间可以查询文件`nmap-services-probes`中`Probe TCP NULL q||`对应的totalwaitms）。通常在等待时间内，会接收到目标机发送的“WelcomeBanner”信息。nmap将接收到的`Banner`与`nmap-services-probes`中`NULL` probe中的签名进行对比。查找对应应用程序的名字与版本信息。
3. 如果通过“Welcome Banner”无法确定应用程序版本，那么nmap再尝试发送其他的探测包（即从nmap-services-probes中挑选合适的probe），将probe得到回复包与数据库中的签名进行对比。如果反复探测都无法得出具体应用，那么打印出应用返回报文，让用户自行进一步判定。
4. 如果是UDP端口，那么直接使用`nmap-services-probes`中探测包进行探测匹配。根据结果对比分析出UDP应用服务类型。
	如果探测到应用程序是SSL，那么调用openSSL进一步的侦查运行在SSL之上的具体的应用类型。
5. 如果探测到应用程序是SunRPC，那么调用`brute-force RPC grinder`进一步探测具体服务。
6. -sV: 指定让Nmap进行版本侦测 
7. --version-intensity <level>: 指定版本侦测强度（0-9），默认为7。数值越高，探测出的服务越准确，但是运行时间会比较长。 
8. --version-light: 指定使用轻量侦测方式 (intensity 2) 
9. --version-all: 尝试使用所有的probes进行侦测 (intensity 9) 
10. --version-trace: 显示出详细的版本侦测过程信息。 

>对主机192.168.80.166进行版本侦测。

* 命令如下：`nmap -sV -p0-65535 -T4 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/29.jpg)
 
Nmap使用TCP/IP协议栈指纹来识别不同的操作系统和设备。在RFC规范中，有些地方对TCP/IP的实现并没有强制规定，由此不同的TCP/IP方案中可能都有自己的特定方式。Nmap主要是根据这些细节上的差异来判断操作系统的类型的。
`具体实现方式如下：`
Nmap内部包含了2600多已知系统的指纹特征（在文件nmap-os-db文件中）。将此指纹数据库作为进行指纹对比的样本库。分别挑选一个open和closed的端口，向其发送经过精心设计的TCP/UDP/ICMP数据包，根据返回的数据包生成一份系统指纹。将探测生成的指纹与nmap-os-db中指纹进行对比，查找匹配的系统。如果无法匹配，以概率形式列举出可能的系统。
1. -O: 指定Nmap进行OS侦测。 
	--osscan-limit: 限制Nmap只对确定的主机的进行OS探测（至少需确知该主机分别有一个open和closed的端口）。 
2. --osscan-guess: 大胆猜测对方的主机的系统类型。由此准确性会下降不少，但会尽可能多为用户提供潜在的操作系统。 

> 命令：`nmap –O 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/30.jpg)
 
3. -vv详细显示扫描状态
`nmap -p21,80,445,3306 -vv 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/31.jpg)
 
4. --script 使用nse脚本，也可自行编写nse脚本，nmap有580多个脚本

`nmap --script=auth 192.168.80.166`
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/32.jpg)
 
5. --script=brute 对弱口令进行暴力破解
`nmap --script=brute 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/33.jpg)
 
6. --script=default 使用默认nse脚本搜集应用的信息
`nmap --script=default 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/34.jpg)
 
7. --script=vuln 检测常见漏洞
`nmap --script=vuln 192.168.80.166`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/35.jpg)
 
`优势：`
* 功能灵活强大，支持多种目标，大量计算机的同时扫描；
* 开源，相关帮助文档十分详细；
* 流行，由于其具有强大的扫描机探测功能，，已被成千上万安全专家使用。
`劣势：`
* Nmap参数众多，难以一一记忆；

## 10、DirBuster

***
DirBuster是一款路径及网页暴力破解的工具,可以破解出一直没有访问过或者管理员后台的界面路径。
Java运行环境+DirBuster程序包
* 双击运行`DirBuster.jar`
* 在URL中输入目标URL或者主机IP地址

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/36.jpg)
 
* 在`file with list of dirs/files` 栏后点击browse，选择破解的字典库为`directory-list-2.3-small.txt`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/37.jpg)
 
* 将File extension中填入正确的文件后缀，默认为`php`，如果为`jsp`、`asp`、`aspx`页面，需要填入`jsp`、`asp`、`aspx`
* 同样可以选择自己设置字典，线程等等
* 其他选项不变，点击右下角的start，启动目录查找

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E5%9B%9B%E8%8A%82/%E5%B0%BA%E5%AF%B8/38.jpg)
 
* 观察返回结果，可点击右下角的report，生成目录报告
优点：
* 敏感目录发掘能力强
* OWASP安全机构极力推荐
`缺点：`
* 探测目录依赖字典文件


