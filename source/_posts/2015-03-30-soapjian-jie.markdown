---
layout: post
title: "SOAP jian jie"
don'tate: 2015-03-30 23:48:51 +0800
comments: true
categories: 
---

#SOAP简介
> SOAP是基于XML的简单协议，可使应用程序在HTTP之上进行信息交换。
> 对于应用程序开发来说，不同程序之间通过英特网通信非常重要。
> 目前使用比较多得还有RPC（Remote Procedure Call Protocol），它也是一种远程过程调用协议，它是通过网络从远程计算机程序上请求服务，但是这种方式存在很多的安全问题以及兼容性问题，防火墙和代理服务器通常会阻止此类流量。
> SOAP提供了一种标准的方法，使得运行在不同操作系统并使用不同技术的应用程序之间可以相互交通信。

#为什么需要SOAP
>随着计算机技术的不断发展，现在企业面临的环境越来越复杂，其信息系统大多数为平台、多系统的复杂系统，这就要求今天的企业解决方案具有广泛的兼容能力，可以支持不同的系统平台、数据格式和多种连接方式，要求在Internet环境下，实现系统的松散耦合和跨平台，而且要求提供对Web应用程序的可靠访问。
>随着异种计算环境的不断增加，各种系统间的互操作性就愈显得必要，要求系统能够无缝地进行通信和共享数据，从而在 Internet 环境下，消除巨大的信息孤岛，实现信息共享、进行数据交换，达到信息的一致性。Web services 希望实现不同的系统之间能够用"软件-软件对话"的方式相互调用，打破了软件应用、网站和各种设备之间的格格不入的状态，实现"基于WEB无缝集成"的目标。


#SOAP语法
>一条soap消息其实就是一个普通的xml，但是对于这个XML又有一些条件必须包含以下的元素：

##必须有 Envelope元素， 该元素可把XML文件标识为一条soap消息。
* 它的值始终应当是http://www.w3.org/2001/12/soap-envelope


`<?xml version="1.0"?>
<soap:Envelope
xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">
  ...
  Message information goes here
  ...
</soap:Envelope>`

> soap消息必须拥有命名空间`http://www.w3.org/2001/12/soap-envelope`相关联的一个Envelope元素，如果使用了不同的命名空间，应用程序就会发生错误，并抛弃此消息。

##可选的 Header元素， 用来定义包含的头部信息。
>该元素包含SOAP消息的应用程序专用消息（比如认证、支付等），如果Header元素被定义，那么它必须是Envelope元素的第一个子元素。

`<soap:Header>
<m:Trans
xmlns:m="http://www.w3school.com.cn/transaction/"
soap:mustUnderstand="1">234</m:Trans>
</soap:Header>`


> SOAP在默认的命名空间中定义了三个属性，这三个属性分别是：actor、mustUnderstand以及encodingStyle。这些被定义在SOAP头部的属性可定义容器如何对SOAP消息进行处理。

###actor
>通过沿着消息路径经过不同的端点，SOAP 消息可从某个发送者传播到某个接收者。并非 SOAP 消息的所有部分均打算传送到 SOAP 消息的最终端点，不过，另一个方面，也许打算传送给消息路径上的一个或多个端点。

`<soap:Header>
<m:Trans
xmlns:m="http://www.w3school.com.cn/transaction/"
soap:actor="http://www.w3school.com.cn/appml/">
234
</m:Trans>
</soap:Header>`

###mustUnderstand

> SOAP 的 mustUnderstand 属性可用于标识标题项对于要对其进行处理的接收者来说是强制的还是可选的。
假如您向 Header 元素的某个子元素添加了 "mustUnderstand="1"，则它可指示处理此头部的接收者必须认可此元素。假如此接收者无法认可此元素，则在处理此头部时必须失效。

`soap:mustUnderstand="0|1"`

`<soap:Header>
<m:Trans
xmlns:m="http://www.w3school.com.cn/transaction/"
soap:mustUnderstand="1">
234
</m:Trans>
</soap:Header>`

###encodingStyle

> SOAP 的 encodingStyle 属性用于定义在文档中使用的数据类型。此属性可出现在任何元素中，并会被应用到元素的内容及元素的所有子元素上。SOAP 消息没有默认的编码方式

`soap:encodingStyle="URI"`

  
##必选的 Body元素，包含所有的调用和响应信息。

`<?xml version="1.0"?>
<soap:Envelope
xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">

<soap:Body>
   <m:GetPrice xmlns:m="http://www.w3school.com.cn/prices">
      <m:Item>Apples</m:Item>
   </m:GetPrice>
</soap:Body>

</soap:Envelope>`

> 上边的例子是请求苹果的价格，对于上边的m:GetPrice和Item元素师应用程序的专用元素，他们并不是SOAP标准的一部分。
而对应的Soap响应应该类似于这样：

`<?xml version="1.0"?>
<soap:Envelope
xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">
<soap:Body>
   <m:GetPriceResponse xmlns:m="http://www.w3school.com.cn/prices">
      <m:Price>1.90</m:Price>
   </m:GetPriceResponse>
</soap:Body>
</soap:Envelope>`

##可选的 Fault元素， 提供有关在处理此消息所发生的错误信息,它必须定义在Body中。

> 在SOAP中Fault只能出现一次，SOAP拥有下列的子元素：

* faultcode 供识别故障的代码
* faultstring 可供人阅读的有关故障的说明
* faultactor 有关是谁引发故障的信息
* detail 存留涉及

##SOAP HTTP Binding

> SOAP 方法指的是遵守 SOAP 编码规则的 HTTP 请求/响应。
HTTP + XML = SOAP
SOAP 请求可能是 HTTP POST 或 HTTP GET 请求。
HTTP POST 请求规定至少两个 HTTP 头：Content-Type 和 Content-Length

`POST /item HTTP/1.1
Content-Type: application/soap+xml; charset=utf-8`

`POST /item HTTP/1.1
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 250`

####参考文档
[SOAP接口相关知识介绍](http://blog.csdn.net/N199109/article/details/23463225)

[SOAP接口中文版](http://download.csdn.net/detail/oponent1/4819108)

[SOAP和Web service哪些事](http://andot.iteye.com/blog/662787)

