---
title: Web安全Csrf漏洞
tags: [getting_started, formatting, content_types]
keywords: posts, blog, news, authoring, exclusion, frontmatter
last_updated: Feb 25, 2016
summary: "Web安全漏洞之Csrf漏洞，主要利用Web漏洞来增加管理员账号和密码，比较鸡肋的漏洞。"
sidebar: mydoc_sidebar
permalink: mydoc_posts.html
folder: mydoc
---

### 序言

今天继续给大家讲前端漏洞，今天介绍的是csrf这个漏洞与xss漏洞的恩怨情仇，别看这两个漏洞只差几个字符，但他们的区别可是天差地别。

### Csrf漏洞

**CSRF**（Cross-site request forgery）跨站请求伪造：也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。

### Csrf漏洞分析

#### 成因

其实说白了，csrf漏洞的成因就是网站的cookie在浏览器中不会过期，只要不关闭浏览器或者退出登录，那以后只要是访问这个网站，都会默认你已经登录的状态。而在这个期间，攻击者发送了构造好的csrf脚本或包含csrf脚本的链接，可能会执行一些用户不想做的功能（比如是添加账号等）。这个操作不是用户真正想要执行的。

#### 危害

攻击者盗用了你的身份，以你的名义发送恶意请求。CSRF能够做的事情包括：以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账......造成的问题包括：个人隐私泄露以及财产安全。

#### 示例

首先找到一个目标站点，csrf存在的危害主要存在于可以执行操作的地方，那么我在我搭建的一个环境中的登录后页面进行测试

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/1.png)

环境就是一个wordpress的环境，大家可以直接去官网下载

我们选择用户界面进行测试，可以看到现在只有一个用户

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/2.png)

下面我添加用户

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/3.png)
 
利用burp进行截断

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/4.png)

利用burp的自带插件来利用csrf

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/5.png)

会生成一个可以利用csrf.html

修改标注内的值，来保证添加的用户不会重复造成无法添加

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/6.png)

在浏览器中尝试

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/7.png)
 
执行按键，发现除了本来存在的第一个用户和我们通过正常手段加入的用户双增加了一个新的test1用户，这个用户就是我们利用csrf点击图片中的submit来执行的操作，因为是我们的测试没有对页面进行修改和直接触发，如果是攻击者利用JS来让用户进行直接的触发，只要打开了相应的页面就会执行这一行为。

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/8.png)

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/9.png)
 
### Csrf高级应用

利用html很不容易让人利用，漏洞触发复杂，那我们就想办法让这个触发方式变的简单起来。

利用xss漏洞来触发csrf漏洞，完成用户添加的操作。

我们首先要先了解发送的数据包内容

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/10.png)

打开上节讲到的xss平台，创建一个csrf的项目，我们来编写一下我们的代码吧

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/11.png)

把这段代码粘到项目的代码配置中去

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/12.png)
 
然后把我们的可利用代码通过留言的存储型XSS漏洞存入到我们的目标站点中去

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/13.png)
 
留言成功后的效果如下

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/14.png)

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/15.png)
 
当管理员查看留言时就执行了我们的危险代码并发送了添加用户的请求

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/16.png)
 
在查看用户列表成功的加入了test2用户

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/17.png)
 
到这里，csrf的攻击实例可以说讲的差不多了，以后就要大家自己去挖掘了。

### 利用工具介绍

Burp相信大家也都有一定的了解，截断数据包，构造csrf漏洞poc，基本是最简单方便的工具了。

在给大家介绍一个CSRFTester-1.0，这个工具，专门针对csrf漏洞，可以构造多种样式的poc

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/18.png)

双击bat文件运行

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/19.png)
 
设置浏览器8008代理，点start就可以获取到通过的数据包了

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%83%E8%8A%82/csrf/20.png)
 
最下方构造需要的poc加以利用，总的来说比burp更加专业，如果专门利用csrf可以使用这个工具。

### 总结

漏洞依靠用户标识危害网站，利用网站对用户标识的信任，欺骗用户的浏览器发送HTTP请求给目标站点，另外可以通过IMG标签会触发一个GET请求，可以利用它来实现CSRF攻击。

CSRF攻击依赖下面的假定：

* 攻击者了解受害者所在的站点

* 攻击者的目标站点具有持久化授权cookie或者受害者具有当前会话cookie

* 目标站点没有对用户在网站行为的第二授权

