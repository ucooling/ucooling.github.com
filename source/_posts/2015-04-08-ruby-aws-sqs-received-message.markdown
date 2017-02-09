---
layout: post
title: "ruby-aws-sqs(received message)"
date: 2015-04-08 00:05:51 +0800
comments: true
categories: 
---
读取消息其实也是非常的简单，以下将从两个方面来介绍如何从AWS的SQS获取消息。
##Received message
从SQS读取消息

```
require 'aws-sdk'
require 'json'

credential = Aws::Credentials.new('access_key_id', 'secret_access_key')
Aws.config.update(credentials: credential, region: 'region')

queue = Aws::SQS::Client.new()

message = queue.receive_message('queue_url')
```

##poller message
订阅SQS的消息，当SQS获取消息，订阅者就可以或得更新的消息

```
require 'aws-sdk'
require 'json'

credential = Aws::Credentials.new('access_key_id', 'secret_access_key')
Aws.config.update(credentials: credential, region: 'region')

queue = Aws::SQS::QueuePoller.new('queue_url')


puts "enter the queue"
queue.poll do |msg|
  puts msg.body
end
```

##参考文档
[aws-sdk docs](http://docs.aws.amazon.com/sdkforruby/api/Aws/SQS/Client.html)