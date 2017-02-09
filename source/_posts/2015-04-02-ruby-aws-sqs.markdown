---
layout: post
title: "ruby-aws-sqs(send_message)"
date: 2015-04-02 21:11:27 +0800
comments: true
categories: 
---
##如何在Ruby中使用AWS的SQS
>SQS(Simple Queue Service)是一个伸缩且可靠的消息传递框架，可以使用SQS简单地创建、存储和获取信息，可以使用它构建基于AWS的应用程序。使用SQS是构建松耦合的Web应用程序的好方法，只需要根据使用量付费，整个队列框架在Amazon数据中心的安全环境中运行。

##SQS提供以下的特性：
* 可靠性
SQS能够跨多个数据中心冗余地存储消息，保证他们随时可用。

* 简单性
访问和使用SQS的编程模型非常的简单，而且可以可以通过多种编程语言使用SQS。

* 安全性
SQS提供很高的安全水平，只允许授权的用户消费消息。

* 可伸缩性
可以使用SQS创建基于队列的应用程序，这些程序可以读写数量不限的消息。

* 低成本
SQS以非常低廉的费率满足您的消息传递需求。

##设计考虑因素
* SQS不保证队列中消息的次序
* SQS不保证删除队列中的消息
* SQS不保证在查询是返回队列中的所有消息


##用Ruby使用SQS的简单Demo
* 设置环境变量(sqs_config.rb)

``` 
require 'aws-sdk'

module Sqs
  include App::Config
  credential = Aws::Credentials.new(config.sqs_access_key, config.sqs_secret_access_key)
  Aws.config.update({
                     credentials: credential,
                     region: config.sqs_region })
  
  SQS = Aws::SQS::Client.new
  def send_message(msg)
    SQS.send_message(queue_url: config.sqs_endpoint, message_body: msg)
  end
end 
```


* 将环境变量提取出来，单独设置

```
module App
   module Config
    def sqs_access_key
      'xxxxx'
    end

    def sqs_secret_access_key
      'xxxxx'
    end

    def sqs_region
      'xxxxxxxx'
    end

    def sqs_endpoint
      'https://sqs.xxxxxxxxx.amazonaws.com/xxxxxxxx/contacts-info'
    end
  end
end
````

* 发送消息
```
include 'sqs_config'
 send_message(msg)
```
 
##参考资料
 [AWS Ruby Demo](http://blog.csdn.net/menxu_work/article/details/38296043)
   



