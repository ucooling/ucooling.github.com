---
layout: post
title: "How to use sinatra"
date: 2015-05-21 22:16:31 +0800
comments: true
categories: 
---

这两天是公司的Hack Day,非常有意义的活动，对于本次的idea不做赘述，这篇博客记录一下在整个的Hack过程中如何使用sinatra来搭建一个后台的服务。
##什么是sinatra
Sinatra是一款非常轻量的Web框架，基于Ruby语言来开发的，其目的是以最小的精力为代价快速创建Web应用为目的的DSL（领域专属语言）。
Sinatra最大的特点就是非常轻量、快速。整个源码也只有1000行稍微多点。
##How to use Sinatra
* 首先需要保证你的系统已经安装了Ruby，至于怎么搭建Ruby环境，其实很简单，如果你使用的是Mac的话，首先你需要安装RVM，使用Rvm来管理你的Ruby，然后再安装Ruby，具体不做细说。
* 创建项目的的目录结构

![目录结构](/images/sinatra_1.png)

* 在Gemfile文件中引入需要使用的Gem包

```
gem 'rake'
gem 'rack', '~>1.5.2'
gem 'json'
gem 'rack-hal_browser'
gem 'mysql2' # STENCIL_OPTIONAL: Database
gem 'sinatra'
gem 'sinatra-activerecord'
gem 'sinatra-contrib'
```
* 运行bundle install
* 创建Rakefile文件
里边引入下面这段代码：

```
require "sinatra/activerecord/rake"

namespace :db do
  task :load_config do
    require "./app"
  end
end

task :default => ['migrate']
```
上边这段代码的目的是引入sinatra自己的rake任务，方便我们去做数据库迁移，如果还想自己去定义其他的rake任务可以继续往改文件里边添加
* 连接数据库，创建config/database.yml文件
在该文件中做如下的配置：

```
development:
  adapter: mysql2
  host: 127.0.0.1
  username: root
  password:
  database: developer-SEC-server
```
本设置比较简单，只是创建了一个开发的数据库，你也可以创建你自己的test环境和产品环境
* 创建db/migrate目录结构
* 创建自己models目录结构
在该文件中你可以新建models，可以跟数据库表一一对应，看自己的需求
* 新建app.rb文件
这个文件是sinatra中的关键文件，在该文件中你可以定义自己对外提供的接口。

```
require 'sinatra'
require 'sinatra/json'
require "sinatra/activerecord"
require "./models/user"

class DeveloperSECApp < Sinatra::Base

	register Sinatra::ActiveRecordExtension

	post '/:login' do
		response.headers['Access-Control-Allow-Origin'] = '*'
		response.headers['Access-Control-Allow-Methods'] = 'POST'
		user = User.find_by("developer_key=?", params['developer_key'])
		@developer_key = params['developer_key']
		if user.password == params['password']
			puts params['password']
			status 200

		else
			puts params['password']
			status 400
		end
	end
end
```
上边的这段代码实现了一个简单地登录认证。

整个上边的过程，只是对sinatra的一个简单地demo，如果要在实际的项目中使用的话可以写出很复杂的应用，跟使用rails没有太大的区别，只是这个gem包更加的轻量，开发更快。

