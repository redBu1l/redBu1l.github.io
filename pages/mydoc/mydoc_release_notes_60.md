---
title: 渗透测试XSS前端漏洞
tags: [getting_started]
keywords: release notes, announcements, what's new, new features
last_updated: July 16, 2016
summary: "红日安全"
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_60.html
folder: mydoc
---

### 前言

这是渗透测试方面的第一课，我们跳过了社工技术的讲解，在之前的课程讲解中已经为大家介绍了社工技术的基本方法，对于社工，我们要做的就是足够细心和耐心，尽可能的收集可利用的信息，说起来很简单，但真正落到实际操作上就需要大家付出大量的时间和精力了。

### 前端漏洞

随着WEB应用越来越复杂，用户对WEB安全也越来越重视。再加上前端工程师的工作面已逐渐扩大，开始覆盖到各种业务逻辑，因此如何应对各种WEB安全问题就显得十分重要。现在危害比较大的前端漏洞主要有xss跨站脚本漏洞，csrf跨站请求伪造漏洞，网上大量的前端攻击行为都是基于这两种漏洞上形成的。今天我们就来介绍下xss漏洞。

### xss跨站脚本漏洞

**非持久型xss攻击**：顾名思义，非持久型xss攻击是一次性的，仅对当次的页面访问产生影响。非持久型xss攻击要求用户访问一个被攻击者篡改后的链接，用户访问该链接时，被植入的攻击脚本被用户游览器执行，从而达到攻击目的。

**持久型xss攻击**：持久型xss，会把攻击者的数据存储在服务器端，攻击行为将伴随着攻击数据一直存在。

xss也可以分成三类：

**反射型**：经过后端，不经过数据库

**存储型**：经过后端，经过数据库

**DOM型**：不经过后端，DOM—based XSS漏洞是基于文档对象模型Document Objeet Model,DOM)的一种漏洞，dom - xss是通过url传入参数去控制触发的。

#### 反射型xss

新建一个xss.php文件并加入以下代码：

```php
\\XSS反射演示  
<form action="" method="get">  
	<input type="text" name="xss"/>  
	<input type="submit" value="test"/>  
</form>  
<?php  
	$xss = @$_GET['xss'];  
	if($xss!==null){  
		echo $xss;  
	}
?>
```

这段代码中首先包含一个表单，用于向页面自己发送`GET`请求，带一个名为xss的参数。 然后PHP会读取该参数，如果不为空，则直接打印出来，这里不存在任何过滤。也就是说，如果xss中存在HTML结构性的内容，打印之后会直接解释为HTML元素。

部署好这个文件，访问`http://localhost/xss.php`，直接输入一个js代码，比如`<script>alert('hack')</script>`

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/1.jpg)

之后点击test：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/2.jpg)

我们输入的HTML代码被执行了。用Firebug查看，我们输出的内容直接插入到了页面中，解释为<script>标签。

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/3.jpg)

反射型XSS的数据流向是：`浏览器` -> `后端` -> `浏览器`。

#### 存储型xss

把xss.php内容改为下述内容（同时数据库中需要配置相应的表）：

```php
\\存储XSS演示  
<form action="" method="post">  
	<input type="text" name="xss"/>  
	<input type="submit" value="test"/>  
</form>  
<?php  
	$xss=@$_POST['xss'];  
	mysql_connect("localhost","root","123");  
	mysql_select_db("xss");  
	if($xss!==null){  
		$sql="insert into temp(id,payload) values('1','$xss')";  
		$result=mysql_query($sql);  
		echo $result;  
	}
?>
```

用户输入的内容还是没有过滤，但是不直接显示在页面中，而是插入到了数据库。

新建show.php，内容为：

```php
<?php 
	mysql_connect("localhost","root","root");  
	mysql_select_db("xss");  
	$sql="select payload from temp where id=1";  
	$result=mysql_query($sql);  
	while($row=mysql_fetch_array($result)){  
		echo $row['payload'];  
	}
?>
```
 
该代码从数据库读取了之前插入的内容，并将其显示出来。

先创建一个数据库xss,创建temp表

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/4.jpg)

然后访问xss.php，像之前一样输入HTML代码

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/5.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/7.jpg)

点击test，点击之后却发现没有任何动静，但事实上，我们的数据已经插入到了数据库中。

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/6.jpg)

当我们访问show.php查询这个值的时候，代码就会被执行。
 
存储型XSS的执行位置通常不同于输入位置。我们可以看出，存储行XSS的数据流向是：
`浏览器` -> `后端` -> `数据库` -> `后端` -> `浏览器`。

#### DOM型xss

把xss.php内容改为

```php
<?php  
	error_reporting(0); //禁用错误报告  
	$name = $_GET["name"];  
?>  
<input id="text" type="text" value="<?php echo $name;?>" />  
<div id="print"></div>  
<script type="text/javascript">  
	var text = document.getElementById("text");   
	var print = document.getElementById("print");  
	print.innerHTML = text.value; // 获取 text的值，并且输出在print内。这里是导致xss的主要原因。  
</script>  
```


![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/8.jpg)

DOM-XSS 的数据流向是：`URL` -->`浏览器` 

#### 总结

在易用上，存储型XSS > DOM - XSS > 反射型 XSS。

>注：反射型xss和dom-xss都需要在数据包中加入js代码才能够触发。

### 漏洞利用

通过 XSS 来获得用户 Cookie 或其他有用信息，利用平台负责接收并保存这些信息。XSS利用平台有很多种如XSS Shell, BeEF, Anehta, CAL9000。

进入搭建好的xsser.me平台首页输入用户名与密码进行登录

成功之后会显示主界面，左边是模块列表，右边是项目列表： 


![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/9.jpg)

我们点击左边“我的项目”旁边的“创建”按钮：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/10.jpg)


名称和描述可以随便取，不影响使用。输入时候点击“下一步”按钮。之后会出现“配置代码”界面：

1[](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/11.jpg)

点击下一步、就会看到这个项目的一些信息

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/11.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/12.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/13.jpg)


点击完成。然后我们会在首页看到我们的新项目，点击这个项目：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/14.jpg)

之后点击项目，进入一个页面再点击右上方的查看代码

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/15.jpg)

就可以看到使用方法：

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/16.jpg)

下面就演示下怎么利用

把`<script src="..."></script>`注入到反射型 XSS 的演示页面中。
 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/17.jpg) 
 
上面的src="xxxx"是我另外创建的一个项目的地址，把你创建好的那个项目，提供的那个地址放进去，就可以了，虽然这个页面没反应，但是xsser.me那个项目就收到消息了。
 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/18.jpg) 
 
### 测试方法

#### 工具测试

工具方面主要介绍brutexss，工具简单但效果非常好，也可以加入自己累积的测试语句和绕过语句
BruteXSS是一个非常强大和快速的跨站点脚本暴力注入。它用于暴力注入一个参数。该BruteXSS从指定的词库加载多种有效载荷进行注入并且使用指定的载荷和扫描检查这些参数很容易受到XSS漏洞。得益于非常强大的扫描功能。在执行任务时， BruteXSS是非常准确而且极少误报。 BruteXSS支持POST和GET请求，适应现代Web应用程序。
开始界面
 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/19.jpg) 
 
选择相应的数据提交方法，并输入测试的链接及相对就的参数

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/20.jpg)
 
接下来选择测试payload 就可以开始测试了，自动进行测试，并会输出对应漏洞内容；

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/21.jpg)
 
Payload可以自己去完善是这个工具的一大亮点
 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/22.jpg) 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/23.jpg)
 
在以后的日子中发现了那种绕过的方法是工具中没有的只要把payload加入到列表中就可以了。

#### 手工测试

手工测试是一个复杂的过程，我们要针对页面返回的内容去构造，这个过程我们要用到神器burpsuit了，利用工具，我们来截断数据包进行对比分析吧。
页面的效果是这样的
 
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/24.jpg) 
 
他的实际内容是这样的

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%BA%94%E8%8A%82/25.jpg)
 
我们手工就是要根据返回的内容去进行修改，如果对方闭合了`〈〉`或`‘‘`等符号我们要在里面进行修改，亦或是对方对语句中的某个点进行了限制，我们要进行绕过测试，找到他们禁用的值，这样我们才可以完成整个攻击行为。

### 危害

由于能够在生成的 Web 页面中注入代码，能想到的威胁有多么严重，就可以有多么严重的威胁。攻击者可以使用 XSS 漏洞窃取 Cookie，劫持帐户，执行 ActiveX，执行 Flash 内容，强迫您下载软件，或者是对硬盘和数据采取操作。

只要您点击了某些 URL，这一切便有可能发生。每天之中，在阅读来自留言板或新闻组的受信任的电子邮件的时侯，您会多少次地单击其中的 URL？

网络钓鱼攻击通常利用 XSS 漏洞来装扮成合法站点。可以看到很多这样的情况，比如您的银行给你发来了一封电子邮件，向您告知对您的帐户进行了一些修改并诱使您点击某些超链接。如果仔细观察这些 URL，它们实际上可能利用了银行网站中存在的漏洞，它们的形式类似于 `http://mybank.com/somepage?redirect=<script>alert(‘XSS’)</script>`，这里利用了“redirect”参数来执行攻击。

如果您足够狡猾的话，可以将管理员定为攻击目标，您可以发送一封具有如下主题的邮件：“求救！这个网站地址总是出现错误！”在管理员打开该 URL 后，便可以执行许多恶意操作，例如窃取他（或她）的凭证。

好了，现在我们已经理解了它的危害性 -- 危害用户，危害管理员，给公司带来坏的公共形象。

