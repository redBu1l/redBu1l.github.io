---
title: 红日安全实验室
keywords: sample homepage
tags: [红日安全, 渗透测试, 靶场, 代码审计, 内网安全]
sidebar: mydoc_sidebar
permalink: index.html
summary: 团队招人中
---

## 红日实验室简介


随着安全业的发展，业内也有很少的公开课红日安全经过十分慎重的决定为大家讲一场基础系列的公开课，
对于红日，很多人可能都不熟悉，但是希望通过这次的公开课可以给大家带来收获。通过近期业内小伙伴提供的需求，本次业余
安全公开课代号“启程”。寓意在这里做一个 新的起点。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%201_meitu_1.jpg)

本次培训为红日安全团队制作一个攻防系列课程，本意是希望没有任何基础“小白”也可以启程。每一个公
开课都有一个故事，希望可以给听这个故事的带来收获与成长。
此次公开课干货较多，会分为几个不同方向进行讲
解，由于讲师都是一线“奋进者”，所以每一期公开课要根据讲师时间来定。另外公开课主要以线下为主，大家可以下载到本地进行观看。
小编
先给大家公开一张思维导图

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%202_meitu_2.jpg)

感觉是不是很酷，思维导图目前只是初级制定，分为几个方向，小编提前给大家透露一点点内容吧
- 基础篇
  + Web安全基础
  + 教你如何掌握最基本渗透套路
- 靶场篇
  + PHP代码审计基础
  + CTF小游戏
  + 教你如何编写一套属于自己的靶场
- 工具篇
  + 自动化漏洞扫描器编写
  + XSS自动验证截图
  + 弱口令自动验证截图
  + 教你如何编写自己漏洞扫描器进行渗透测试
- 进阶篇
  + 综合环境搭建、漏洞扫描
  + 掌握了以上知识，那么恭喜你，成为一名中级“小白”
- 运维片
  + 搭建Snort、安全运维环境
- 报告篇
  + 教你如何写一篇安全风险报告
- 高级篇
  + 综合实战
  + 教你从前端+审计+后端+内网整体性有一个安全概念

具体讲解还要根据每一块内容来定，基础篇Orian讲师根据工具安全基础篇信息泄漏又做了一个小的思维导图
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%203_meitu_3.jpg)

基础讲解会以这样思维导图进行讲解。其它部分也会以基础部分进行讲解。
开讲前先给大家简单透露总结一下.
## 环境搭建
以前讲解渗透测试部分，都是利用虚拟机来搭建渗透测试平台，如果换机器，则环境需要重新搭建，程序大而且也不方便，此次培训平台采用docker方式搭建，让大家学会“简单学习”。快速摆脱环境困难问题。
思路：渗透测试主要讲解Web漏洞部分和主机漏洞部分。
Docker搭建Web漏洞部分也有公开案例，大家可以参考。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%204_meitu_5.jpg)

后续搭建部分教大家学习dockfile编写。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%205_meitu_6.jpg)

### Web其它漏洞

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%206_meitu_7.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%207_meitu_8.jpg)

这些会根据都是网上公开docker环境，有这些前辈贡献，我们才可以更简单去学习。我们讲解到某一块内容以后，我们都要根据公开漏洞进行讲解，最重要是根据docker的简单性和移植性，搭建我们自己的渗透测试环境。除了Web漏洞环境，安全工具也比较重要，所以平时有比较难安装，且依赖环境重要的工具，我们可以采取docker进行安装，比如`巡风`、`cobra`等业界不错工具。可以利用docker快速安装，进行快速漏洞挖掘和使用。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%208_meitu_9.jpg)

### cobra

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%209_meitu_10.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2010_meitu_11.jpg)

讲解完相应内容，会进行搭建docker环境。网上公开渗透测试环境，我们也尝试用docker进行搭建，方便搭建学习。另外也会从网上找一些公开漏洞给大家尝试搭建docker环境。有一下两点
- 摆脱环境困难搭建问题
- 平时只是看，而不去实操
- 两条命令，环境从现
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2011_meitu_12.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2012_meitu_13.jpg)

环境搭建包含但不限于以下环境
- WAF、日志安全
- 前端安全
- 代码审计
- 渗透测试
- 内网安全
- 运维安全

尝试用docker搭建一个系列。另外大家期待的CTF小靶场后期也在编写当中，
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2013_meitu_14.jpg)
## 运维方向
搭建蜜罐、Snort工具使用、日志分析
密码和snort可用来充当企业IPS/IDS操作。日志操作主要利用Windows搭建一套cms系统，然后利用hacker方式攻击服务器和操作服务器，把日志提取出来，以这种类型给大家分析，还有其它类型，小编暂不多透露。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2014_meitu_15.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2015_meitu_16.jpg)
## 内网方向
利用docker搭建一部分虚拟机以外，还需要本地进行搭建学习。个人搭建环境，利用虚拟机搭建一个网络。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2016_meitu_17.jpg)

公开内网学习环境，利用TEST LAB V8进行实例讲解。只需一个VPN即可解决。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2017_meitu_18.jpg)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2018_meitu_19.jpg)

课程学习方向记录，我们申请一个网站来记录文档，方便大家学习。
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2019_meitu_20.jpg)

其它学习路径，我们也有自己的博客和论坛，方便大家提问和讨论。

[http://sec-redclub.com/](http://sec-redclub.com/)
[http://bbs.sec-redclub.com/hr/forum.php](http://bbs.sec-redclub.com/hr/forum.php)
![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2020_meitu_21.jpg)

路给大家选好了，学习不学习就靠大家了。后续我们团队也会发表安全视频其它Papers和以及大量学习资料，也会在知识星球和大家探讨。
星球亮点如下：
- "老司机"为小白解答视频问题
- 多人组团进行学习，解决各类”疑难病症“
- 拜师学艺
老师也都不容易，也算给各位老师一点资助，让他们有动力创造出更好的视频和文章。
### 星球二维码

![](https://raw.githubusercontent.com/redBu1l/Redclub-Launch/master/%E6%94%BB%E9%98%B2%E7%AC%AC%E4%B8%80%E8%8A%82/%E5%9B%BE%E7%89%87%2021_meitu_22.jpg)

微信交流群
红日-启程


{% include links.html %}
