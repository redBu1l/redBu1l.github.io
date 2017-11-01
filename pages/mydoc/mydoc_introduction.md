---
title: 信息收集
sidebar: mydoc_sidebar
permalink: mydoc_introduction.html
folder: mydoc
---


信息收集一般都是渗透测试前期用来收集，为了测试目标网站，不得不进行各种信息收集。信息收集要根据不同目标进行不同方向收集，工具部分会在下节课程进行讲解，根据个人渗透测试经验总结文章。本文只是抛砖引玉，希望可以给大家一个好的思路。如果文章中有环境搭建部分，靶场后续会在公众号中发布。视频在关注公众号以后，回复我要视频，管理员会在最快时间进行回复。
首先公开上一节中一张图，开始今天主题讲解。
## 信息收集思维导图
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%203_meitu_3.jpg)
------


#信息收集

## 1、robots.txt

<h5>当一个搜索蜘蛛访问一个站点时，它会首先检查该站点根目录下是否存在robots.txt，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。</h5>

<h5>robots.txt基本上每个网站都用，而且放到了网站的根目录下，任何人都可以直接输入路径打开并查看里面的内容，如http://127.0.0.1/robots.txt ，该文件用于告诉搜索引擎，哪些页面可以去抓取，哪些页面不要抓取</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/1.png) 

<h5>robots.txt防黑客，为了让搜索引擎不要收录admin页面而在robots.txt里面做了限制规则。但是这个robots.txt页面未对用户访问进行限制，可任意访问，导致可通过该文件了解网站的结构，比如admin目录、user目录等等。</h5>

<h5>怎样即使用robots.txt的屏蔽搜索引擎访问的功能，又不泄露后台地址和隐私目录呢？</h5>

<h5>有，那就是使用星号（*）作为通配符。举例如下：</h5>

* User-agent:*
* Disallow: /a*/

<h5>这个设置，禁止所有的搜索引擎索引根目录下a开头的目录。当然如果你后台的目录是admin，还是有可以被人猜到，但如果你再把admin改为adminzvdl呢？</h5>

## 2、网站备份压缩文件

<h5>管理员在对网站进行修改、升级等操作前，可能会将网站或某些页面进行备份，由于各种原因将该备份文件存放到网站目录下，该文件未做任何访问控制，导致可直接访问并下载。可能为.rar、zip、.7z、.tar.gz、.bak、.txt、.swp等等，以及和网站信息有关的文件名www.rar、web、rar等等</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/2.png)

## 3、Git导致文件泄露

<h5>由于目前的 web 项目的开发采用前后端完全分离的架构:前端全部使用静态文件，和后端代码完全分离，隶属两个不同的项目。表态文件使用 git 来进行同步发布到服务器，然后使用 nginx 指向到指定目录，以达到被公网访问的目的。</h5>

<h5>在运行git init初始化代码库的时候，会在当前目录下面产生一个.git的隐藏文件，用来记录代码的变更记录等等。在发布代码的时候，把.git这个目录没有删除，直接发布了。使用这个文件，可以用来恢复源代码</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/3.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/4.png) 

## 4、DS_store导致文件泄露

<h5>.DS_Store是Mac下Finder用来保存如何展示文件/文件夹的数据文件，每个文件夹下对应一个。由于开发/设计人员在发布代码时未删除文件夹中隐藏的.DS_store，可能造成文件目录结构泄漏、源代码文件等敏感信息的泄露。</h5>

<h5>我们可以模仿一个环境，利用phpstudy搭建PHP环境，把.DS_store文件上传到相关目录。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/5.png)

<h5>然后利用工具进行相关检测</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/6.png)

<h5>工具下载地址：https://github.com/lijiejie/ds_store_exp </h5>

<h5>为了让实验更真实，我们在本地搭建环境，然后建立一个文件夹为admin和一个hello文件夹，利用该工具运行完以后，查看工具文件夹查看有什么结果。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/7.png) 

<h5>此文件和我们在一个文件夹内，如果是苹果用户，把文件copy到相关服务器目录以后，都会默认带一个文件.DS_Store。首先访问test.php文件，查看环境是否成功。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/8.png) 

<h5>环境搭建成功</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/9.png)

<h5>我们利用工具进行测试，运行完如上图，运行完以后我们可以到工具目录进行查看</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/10.png)
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/11.png) 

<h5>这是一个.DS_Store文件泄漏利用脚本，它解析.DS_Store文件并递归地下载文件到本地。</h5>

## 5、SVN导致文件泄露

<h5>Subversion，简称SVN，是一个开放源代码的版本控制系统，相对于的RCS、CVS，采用了分支管理系统，它的设计目标就是取代CVS。互联网上越来越多的控制服务从CVS转移到Subversion。 </h5>

<h5>Subversion使用服务端—客户端的结构，当然服务端与客户端可以都运行在同一台服务器上。在服务端是存放着所有受控制数据的Subversion仓库，另一端是Subversion的客户端程序，管理着受控数据的一部分在本地的映射（称为“工作副本”）。在这两端之间，是通过各种仓库存取层（Repository Access，简称RA）的多条通道进行访问的。这些通道中，可以通过不同的网络协议，例如HTTP、SSH等，或本地文件的方式来对仓库进行操作。 </h5>

<h5>SVN漏洞在实际渗透测试过程中，利用到也比较多，由于一些开发管理员疏忽造成，原理类似DS_Store漏洞。我们这里不再进行搭建环境，给大家推荐工具，利用方法如下：</h5>

> 1)漏洞利用工具： Seay SVN漏洞利用工具
> 2)添加网站url

<h5>在被利用的网址后面加 /.svn/entries，列出网站目录，甚至下载整站</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/12.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/13.png) 

<h5>下载地址：https://pan.baidu.com/s/1jGA98jG</h5>

## 6、WEB-INF/web.xml泄露

<h5>WEB-INF是Java的WEB应用的安全目录。如果想在页面中直接访问其中的文件，必须通过web.xml文件对要访问的文件进行相应映射才能访问。</h5>

<h5>WEB-INF主要包含一下文件或目录：</h5>

<h5>/WEB-INF/web.xml：Web应用程序配置文件，描述了 servlet 和其他的应用组件配置及命名规则。</h5>

<h5>/WEB-INF/classes/：含了站点所有用的 class 文件，包括 servlet class 和非servlet class，他们不能包含在 .jar文件中</h5>

<h5>/WEB-INF/lib/：存放web应用需要的各种JAR文件，放置仅在这个应用中要求使用的jar文件,如数据库驱动jar文件</h5>

<h5>/WEB-INF/src/：源码目录，按照包名结构放置各个java文件。</h5>

<h5>/WEB-INF/database.properties：数据库配置文件</h5>

#### 原因：

<h5>通常一些web应用我们会使用多个web服务器搭配使用，解决其中的一个web服务器的性能缺陷以及做均衡负载的优点和完成一些分层结构的安全策略等。在使用这种架构的时候，由于对静态资源的目录或文件的映射配置不当，可能会引发一些的安全问题，导致web.xml等文件能够被读取</h5>

<h5>环境搭建：我们需要利用jsp源码给大家进行示范，所以前提需要下载一个jsp环境，这里我们选取jspstudy进行示范。下载地址：http://www.phpstudy.net/phpstudy/JspStudy.zip</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/14.png) 

<h5>另外一种方法就是直接下载webgoat然后执行文件中webgoat.bat文件即可</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/15.png) 

<h5>下载地址：http://sourceforge.net/projects/owasp/files/WebGoat/WebGoat5.2/ </h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/16.png) 

<h5>访问地址：http://localhost/, 进入此页面，证明我们tomcat已经启动，我们查看一下web.xml目录在哪里，你可以练习此靶场，靶场在后续会进行讲解。这里我们只讲解此web.xml信息泄露漏洞。如果让用户设置权限不严格，造成一些目录列出，结果是非常严重，我们通过访问web.xml文件，可以查看一些敏感信息，如下图</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/17.png) 

<h5>利用工具扫描，我们得知此目录下面有一些敏感文件，我们尝试去访问</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/18.png) 

<h5>首先是一些tomcat登录信息，我们尝试去访问一些其它文件，通过不断尝试目录，有发现了一个sql文件和xml文件。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/19.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/20.png) 

## 7、Zoomeye搜索引擎使用

<h5>ZoomEye 支持公网设备指纹检索和 Web 指纹检索</h5>

<h5>网站指纹包括应用名、版本、前端框架、后端框架、服务端语言、服务器操作系统、网站容器、内容管理系统和数据库等。</h5>

<h5>设备指纹包括应用名、版本、开放端口、操作系统、服务名、地理位置等</h5>

<h5>简单了解下都有哪些搜索规则</h5>


<h5> 首先，我们讲解下相关的快捷键，提高使用效率</h5>

* Shift /: 显示快捷帮助

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/21.png) 

* Esc: 隐藏快捷帮助

* Shift h: 回到首页

* Shift s: 高级搜索

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/22.png) 

* s: 聚焦搜索框

#### 搜索技巧

<h5>在设备和网站结果间切换</h5>

<h5>ZoomEye 将默认搜索公网设备，搜索结果页面左上角有公网设备和 Web 服务两个连接。因此您可以快速切换两种结果。</h5>

<h5>在输入关键字时，自动展开的智能提示下拉框最底部有两个指定搜索的选项。用方向键选定其中一个，回车即可执行搜索。</h5>

<h5>ZoomEye 使用 Xmap 和 Wmap ：两个能获取 Web 服务 和公网设备指纹的强大的爬虫引擎定期全网扫描，抓取和索引公网设备指纹。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/23.png) 

<h5>同样zoomeye也存在一个高级搜索，只需填写你想查询的内容即可，这里不在做过多的介绍。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/24.png) 

<h5>我们今天主要讲下如何使用他的语法规则去高级搜索，搜索有用信息。</h5>

#### 主机设备搜索
#### 组件名称
* app: 组件名
* ver: 组件版本

#### 例1：

<h5>搜索使用iis6.0主机： app:"Microsoft-IIS" ver"6.0"，可以看到0.6秒搜索到41，781,210左右的使用iis6.0的主机。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/25.png) 
#### 例2：

<h5>搜索使weblogic主机： app:"weblogic httpd" port:7001，可以看到0.078秒搜索到42万左右的使用weblogic的主机。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/26.png) 
#### 端口
<h5>port: 开放端口</h5>

<h5>搜索远程桌面连接：port:3389</h5>

<h5>下面我们搜索下开放ssh功能的服务器</h5>

* port:22

#### 例1：
<h5>查询开放3389端口的主机：port:3389</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/27.png)

<h5>同理查询22端口开放主机：port:22</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/28.png) 
### 操作系统

#### os: 操作系统。 
#### 例:os:linux，查询操作系统为Linux系统的服务器

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/29.png) 

<h5>同样，可以查看与Linux相关的漏洞</h5>

#### 服务
<h5>service: 结果分析中的“服务名”字段。</h5>

<h5>完整的“服务名”列表，请参阅 https://svn.nmap.org/nmap/nmap-services</h5>

#### 例1：
<h5>公网摄像头： service:”routersetup”</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/30.png) 
#### 位置
* country: 国家或者地区代码。
* city: 城市名称。

<h5>完整的国家代码，请参阅: 国家地区代码 - 维基百科</h5>

#### 例1：

<h5>搜索美国的 Apache 服务器： app:Apache country:US</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/31.png) 

#### IP 地址

<h5>ip: 搜索一个指定的 IP 地址</h5>

#### 例：

<h5>搜索指定ip信息，ip:121.42.173.26</h5>
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/32.png) 

<h5>CIDR（无类别域间路由，Classless Inter-Domain Routing）是一个在Internet上创建附加地址的方法，这些地址提供给服务提供商（ISP），再由ISP分配给客户。CIDR将路由集中起来， 使一个IP地址代表主要骨干提供商服务的几千个IP地址，从而减轻Internet路由器的负担。</h5>

#### 例1：
<h5>IP 的 CIDR 网段。 CIDR:114.114.114.114/8</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/33.png) 

#### Web应用搜索

<h5>这里只讲解Web应用的查询方法</h5>

#### 网站

#### site:网站域名。 

#### 例子：

<h5>查询有关taobao.com域名的信息，site:taobao.com</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/34.png) 
#### 标题
#### 例：
<h5>搜索标题中包含该字符的网站，title:weblogic</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/35.png) 

#### 关键词

<h5>keywords: <meta name="Keywords">定义的页面关键词。</h5> 

#### 例子： keywords:Nginx
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/36.png) 
#### 描述
<h5>desc: <meta name="description">定义的页面说明。 </h5>

#### 例子： desc:Nginx

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/37.png) 
## 8、bing搜索引擎使用

<h5>filetype:仅返回以指定文件类型创建的网页。</h5>    

<h5>若要查找以PDF格式创建的报表，请键入主题，后面加filetype:pdf</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/38.png) 

<h5>inanchor:、inbody:、intitle: 这些关键字将返回元数据中包含指定搜索条件（如定位标记、正文或标题等）的网页。为每个搜索条件指定一个关键字，您也可以根据需要使用多个关键字。    若要查找定位标记中包含msn、且正文中包含seo和sem的网页，请键入</h5>

<h5>例：inanchor:msn inbody:seo inbody:sem</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/39.png) 

<h5>site:返回属于指定网站的网页。若要搜索两个或更多域，请使用逻辑运算符OR对域进行分组。

您可以使用site:搜索不超过两层的Web域、顶级域及目录。您还可以在一个网站上搜索包含特定搜索字词的网页。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/40.png) 

<h5>url:检查列出的域或网址是否位于Bing索引中。    请键入url:sec-redclub.com</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/41.png) 

## 9、Fofa搜索

<h5>下面我们讲下另一款搜索引擎：Fofa 地址：https://fofa.so/ </h5>

<h5>首先我们了解下他的语法</h5>

<h5>可以看到有许多查询语法，这里我们只介绍几个常用的语句</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/42.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/43.png) 

<h5>引用：该专题分为数据库专题模块与工控专题模块。在数据库专题模块，包含了检索大部分的数据库服务与协议的规则；在工控专题模块，提供了在世界上广泛使用的工业控制协议的介绍与检索。在模块内部您可以通过点击相关链接的方式进行协议或服务的快速查询。</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/44.png) 

<h5>protocol="https"，搜索指定协议类型</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/45.png) 

<h5>app="phpinfo"搜索某些组件相关系统</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/46.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/47.png) 
![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/48.png) 

<h5>hos/t="sec-redclub.com/"搜索包含有特定字符的URL</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/49.png) 

<h5>title="powered by" && title!=discuz搜索网页标题中包含有特定字符的网页</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/50.png) 

<h5>title="powered by" && os==windows搜索网页标题中包含有特定字符并且系统是windows的网页</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/51.png) 

## 10、站长工具

<h5>使用站长工具Whois可以查询域名是否已经被注册，以及注册域名的详细信息的数据库（如域名所有人、域名注册商）</h5>

<h5>http://tool.chinaz.com/</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/52.png) 

<h5>seo综合查询可以查到该网站各大搜索引擎的信息,包括收录,反链及关键词排名,也可以一目了然的看到该域名的相关信息,比如域名年龄相关备案等等,及时调整网站优化等等。</h5>

<h5>http://seo.chinaz.com/</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/53.png) 

<h5>可以看到有些加密/解密功能，例如MD5、url、js、base64加解密等等</h5>

![](https://raw.githubusercontent.com/Orion1250/picture/master/picture-new/info-search/54.png) 



