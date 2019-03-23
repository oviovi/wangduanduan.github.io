---
title: 如何学习网络协议？
date: 2019-01-18 21:32:08
tags:
---


> 大学时，学到网络协议的7层模型时，老师教了大家一个顺口溜：物数网传会表应。并说这是重点，年年必考，5分的题目摆在这里，你们爱背不背。
> 考试的时候，果然遇到这个问题，搜索枯肠，只能想到这7个字的第一个字，因为这5分，差点挂科。
> 后来工作面试，面试官也是很喜欢七层模型，三次握手之类的问题，但是遇到这些问题时，总是觉得很心虚。

# 1. 协议分层
四层网络协议模型中，应用层以下一般都是交给操作系统来处理。应用层对于四层模型来说，仅仅是冰山一角。海面下巨复杂的三层协议，都被操作系统给隐藏起来了，一般我们在页面上发起一个ajax请求，看见了network面板多了一个http请求，至于底层是如何实现的，我们并不关心。

![](http://assets.processon.com/chart_image/5c41d889e4b0641c83e1f059.png)

- `应⽤层`负责处理特定的应⽤程序细节。
- `运输层`运输层主要为两台主机上的应⽤程序提供端到端的通信。
- `网络层`处理理分组在⽹网络中的活动，例例如分组的选路
- `链路层`处理理与电缆(或其他任何传输媒介)的物理理接⼝口细节

下面重点讲一下运输层和网络层

## 1.1. 运输层的两兄弟
运输层有两个比较重要的协议。tcp和udp。

大哥tcp是比较`严谨认真、温柔体贴、慢热内向`的协议，发出去的消息，总是一个一个认真检查，等待对方回复和确认，如果一段时间内，对方没有回复确认消息，还会再次发送消息，如果对方回复说你发的太快了，tcp还会体贴的把发送消息的速度降低。

弟弟udp则比较可爱呆萌、调皮好动、不负责任的协议。哥哥tcp所具有的特点，弟弟udp一个也没有。但是`有的人说不清哪里好 但就是谁都替代不了`，udp没有tcp那些复杂的校验和重传等复杂的步骤，所以它发送消息非常快，而且并不保证对方一定收到。如果对方收不到消息，那么udp就会呆萌的看着你，笑着对你说：`我已经尽力了`。一般语音而视频数据都是用udp协议传输的，因为音频或者视频卡了一下并不影响整体的质量，而对实时性的要求会更高。

## 1.2. 运输层和网络层的区别

- `运输层关注的是端到端层面`，及End1到End2，忽略中间的任何点。
- `网络层关注两点之间的层面`，即hop1如何到hop2，hop2如何到hop3
- `网络层并不保证消息可靠性`，可靠性上层的传输层负责。TCP采用超时重传，分组确认的机制，保证消息不会丢失。

![](http://assets.processon.com/chart_image/5c41e125e4b056ae29f55886.png)

从下图tcp, udp, ip协议中，可以发现

- 传输层的tcp和udp都是有源端口和目的端口，但是没有ip字段
- 源ip和目的ip只在ip数据报中
- 理解各个协议，关键在于理解报文的各个字段的含义

![](http://assets.processon.com/chart_image/5c41e22fe4b056ae29f55947.png)

## 1.3. ip和端口号的真正含义

上个章节讲到运输层和网络层的区别，其中端口号被封装在运输层，ip被封装到网络成，

那么端口号和ip地址到底有什么区别呢？

- ip用来用来标记主机的位置
- 端口号用来标记该数据应该被目标主机上的哪个应用程序去处理

![](http://assets.processon.com/chart_image/5c41e3a1e4b0fa03ce9f52c9.png)

## 1.4. 数据在协议栈的流动 封装与分用

- 当发送消息时，数据在向下传递时，经过不同层次的协议处理，打上各种头部信息
- 当接受消息时，数据在向上传递，通过不同的头部信息字段，才知道要交给上层的那个模块来处理。比如一个ip包，如果没有头部信息，那么这个消息究竟是交给tcp协议来处理，还是udp来处理，就不得而知了

![](http://assets.processon.com/chart_image/5c41e531e4b056ae29f55ab3.png)

# 2. 深入阅读，好书推荐

- 《http权威指南》 `有人说这本书太厚，偷偷告诉你，其实这本书并厚，因为这本书的后面的30%部分都是附录，这本书的精华是前50%的部分`
- 《图解http》、《图解tcp/ip》`这两本图解的书，知识点讲的都是比较通俗易懂的，适合入门`
- 《tcp/ip 详解 卷1》`这本书，让你知其然，更知其所以然`
- 《tcp/ip 基础》、《tcp/ip 路由技术》`这两本书，会让你从不同角度思考协议`
- 《精通wireshark》、《wireshark网络分析实战》`如果你看了很多书，却从来没有试过网络抓包，那你只是懂纸上谈兵罢了。你永远无法理解tcp三次握手的怦然心动，与四次分手的刻骨铭心。`

