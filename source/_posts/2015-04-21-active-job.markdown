---
layout: post
title: "active-job"
date: 2015-04-21 23:11:02 +0800
comments: true
categories: 
---
##初探ActiveJob
ActiveJob 是 Rails 4.2 新加入的功能。这个东西在beta阶段rubyChina就已经有很多高手关注了
###简介
ActiveJob 是Rails自己开发运行后台程序的模块，常用于执行运行时间可能很长的工作（比如发送注册邮件）。

当然这种需求实际上非常普遍，所以rails 也有相应的第三方gem来解决这个需求，比如著名的Sidekiq和Resque等。**ActiveJob的出现不是为了代替他们**，而是统一了原来Resque、Sidekiq 等其他gem对后台运行程序的各种千奇百怪的写法。

本文使用的后台程序gem包是‘delayed_job_active_record’

* 使用以下的命令创建deplayed_job table

```
rails g delayed_job:active_record
rake db:migrate
```

* 要想使用ActiveJob官方文档给出了示例
首先在命令行中执行如下的命令(add_lots_of_users是需要创建的job的名字)：

```
rails g job add_lots_of_users
```

打开app/jobs里边会多出一个ruby文件，该文件就是我们的active job 文件
文件内容类似下边

```
class AddLotsOfUsersJob < ActiveJob::Base
  queue_as :default

  def perform(*args)
    # Do something later
  end
end
```

我们可以将需要跑的后台程序写到perform文件中，如：

```
def perform(*args)
    # Do something later
    sleep 10
    1000.times do |index|
      user = User.new
      user.name = "atpking#{index}"
      user.save
    end
  end  
```
  
 改后台程序可以在任何的地方调用，下面是一个简单的调用，在加载用户首页的时候去执行改后台程序
 
 ```
 class UsersController < ApplicationController
  before_action :set_user, only: [:show, :edit, :update, :destroy]

  # GET /users
  # GET /users.json
  def index
    AddLotsOfUsersJob.perform_later
    @users = User.all
  end
end
```




