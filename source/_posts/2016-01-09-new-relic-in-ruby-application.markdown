---
layout: post
title: "New Relic in ruby application"
date: 2016-01-09 16:49:56 +0800
comments: true
categories: 
---
## 背景
随着互联网、以及移动互联领域的快速发展，可以说现在互联网已经无处不在，人们对于互联网的性能也越来越挑剔，如果网站速度太慢，发生错误的频率太高，这些因素都会使用户对网站失去信心，所以必须实时去检测网站的运行状况。New Relic就是一个对网站的性能进行检测的工具，它能够检测网站的响应速度，用户的满意度，错误发生的频率等。
## 介绍
[这里](https://ruby-china.org/topics/22379)有一篇很好的介绍，我就不再复述了。

## 如何使用
本文主要是针对在Ruby项目上的使用

* 引入New Relic到你的应用

  ```
  gem 'newrelic_rpm'
  ```
  
* [注册](http://newrelic.com/)一个New Relic账号
* 下载newrelic.yml文件
* 在本地环境使用New Relic工具
  
 1 在boot.rb文件中引入下面的代码：
 
   ```
   require 'newrelic_rpm'
   ```
   
 2 配置newrelic.yml文件
   
   ```
   development:
         <<: *default_settings
         monitor_mode: true
         developer_mode: true
   ```
   
 3 配置要检测的网站
   这里后台框架使用的是Web machine, 所以它的配置是这样的。如果使用的是其它的框架，应该根据框架的不同去进行不同的设置。
   
   ```
   use NewRelic::Rack::DeveloperMode
     use Rack::HalBrowser::Redirect, :exclude => ['/trace', '/products/diagnostic',         '/service-info', '/newrelic'] # /trace is for Webmachine
   ```
   
 4 当错误发生是如何发生报警
 这里可以用两种选择，因为New Relic 他只能对于程序的所有错发发生频率报警，只有当测序发生的错误频率高于配置测报警率的时候就会报警，报警的方式可以使发邮件或者slack或者PageDuty等。他不会对某一个特殊的错误进行报警，也不会对HTTP请求进行报警。
 告诉New Relic程序发生错误的方式有两种：
 
 * 显式的在程序中抛出异常。
 * 调用New Relic 的API
 
 ```
 require 'new_relic/agent/transaction’
 NewRelic::Agent.notice_error(msg)
 ```
 
 总之，New Relic是一个非常有用的工具。这里只是简单地介绍了一下New Relic的基本功能和用法，更多高级的功能还需要大家去发掘。
 
 

  