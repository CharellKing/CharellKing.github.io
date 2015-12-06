---
title: wireshark学习之ping命令
description: 从现在起，开始学习《wireshark网络分析就是这么简单》这本书；学习并掌握wireshark一直是我的心愿，最终是要了解网络的底层协议，以及解决网络问题。首篇文章以了解ping命令的原理为主题，深刻体会到了wireshark的强大之处。
date: 2015/12/05 22:28:46 +0800
layout: post
permalink: /blog/2015/12/05 wireshark学习之ping命令.html
categories:
  - 网络
tags:
  - wireshark
  - ping

---
本文章将详细讲解两种情况下ping过程：相同子网掩码下的ping过程，不同子网掩码下的ping过程。

## 相同子网掩码

现在有两台机器：
![主机](/assets/picture/2015-12-05-相同子网-主机.png)


### ping交互过程
![ping交互过程](/assets/picture/2015-12-05-相同子网ping的过程图.png "ping交互过程")

### wireshark分析

1. 由于B与A属于同一个子网，所以A直接向整个子网发送询问“谁的IP是192.168.61.103，请把你的mac地址告诉192.168.61.111”
![A询问对应IP的MAC地址](/assets/picture/2015-12-05-相同子网ping01-arp请求.png "A询问对应IP的mac地址")

2. 子网内IP为192.168.61.103的机器（B）就会直接向A做出ARP回答，否则就忽略掉询问。
![B回复询问](/assets/picture/2015-12-05-相同子网ping02-回复mac地址.png  "回复询问，告诉自己的MAC地址")

3. 由于A知道了B的MAC地址，可以直接与B进行通讯了，发送ICMP请求。
![A向B直接发出请求](/assets/picture/2015-12-05-相同子网ping03-发送ping请求.png  "直接向B发送请求")

4. B向A做出ICMP回复
![B向A直接回复请求](/assets/picture/2015-12-05-相同子网ping04-回复ping请求.png "B直接向A回复请求")

5. 自此A与B的通讯已经建立，不断重复3与4的步骤


## 不同子网掩码

将A的子网掩码改一改，还是之前的机器：
![主机](/assets/picture/2015-12-05-不同子网-主机.png)


### ping交互过程
![不同子网ping交互过程](/assets/picture/2015-12-05-不同子网ping的过程图.png "不同子网ping交互过程")

### wireshark分析

1. 请求网关将与192.168.61.103的通信请求转发给192.168.61.103的主机。
![向网关发送代转发的通信请求](/assets/picture/2015-12-05-不同子网ping01-向网关发送请求.png "向网关发送代转发的通信请求")
![请求包的详细信息](/assets/picture/2015-12-05-不同子网ping01-包的详细信息.png "请求包的详细信息")

*注意：虽然destination是192.168.61.103, 但是目的地的mac地址是TpLinkT_21:f7:86*

2. 网关将请求转发给了192.168.61.103，并且向A回复我已经将请求转发了。
![网关回复已经转发](/assets/picture/2015-12-05-不同子网ping02-网关回复.png "网关回复已经转发")

3. B就开始询问了，192.168.61.111是谁，请报告你的mac地址。
![B询问请求源的mac地址](/assets/picture/2015-12-05-不同子网ping03-反向询问mac地址.png "B询问请求源的mac地址")

4. A就回复了自己的mac地址。
![A告诉B自己的mac地址](/assets/picture/2015-12-05-不同子网ping04-A以自己的mac地址回复.png "A告诉B自己的mac地址")

5. B回复A第一次的ICMP请求
![B回复A第一次的ICMP请求](/assets/picture/2015-12-05-不同子网ping05-B直接回复第一次的ping请求.png "B回复A第一次的ICMP请求")



### 与书上不同之处

1. 书上在要求网关转发地址之前，先是用arp向网关询问网关的mac地址；可能这里已经知道了网关的mac地址，故不需要经过这一步。

2. 没有讲到网关转发请求后向A回复我已经将请求转发了（也就是第2步）。


## 两者的不同点

最大的区别在于发送arp请求的方向。

当网关配置正确时， A直接arp整个子网，询问B的mac地址；而当网关配置错误时， A需要借助网关转发通讯请求，B收到请求之后，反向的ARP整个子网，询问A的mac。
