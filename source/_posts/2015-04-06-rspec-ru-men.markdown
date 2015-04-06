---
layout: post
title: "Rspec 入门"
date: 2015-04-06 21:49:27 +0800
comments: true
categories: 
---

##Rspec介绍
Rspec工具是一个Ruby软件包，可以用它构建有关您的软件规范，该规范实际上是一个描述系统行为的测试

##关于TDD
很多人对于写测试的第一印象可能是：

* 写测试很无聊
* 写测试很浪费时间
* 测试很难写
* 测试对于开发的帮助不大

但是我现在想说，其实一开始接触写测试的时候我也同样觉得这种方式很浪费时间，而且有的测试确实很难写，增加了学习的成本，且收效甚微，但是当我慢慢在工作中发现他的好处的时候，我的这种想法慢慢的发生了改变，写测试其实是有很多的好处的

* 开发之前写测试可以保证你的代码的正确性，保证你做的事情是正确的。
* 当之后再对代码进行重构或者添加新的功能的时候，测试可以保证你新加的代码不会影响以前的功能
* 可以采用TDD的开发方式，在写测试的过程中可以提前对你的代码进行设计，在以后测试可以当成你代码的文档，不用再去写一些注释或者其他的一些文档。

而且现在很多公司的开发也开始越来越重视测试，足以说明测试的重要性，也是作为一个码农必须去学习和研究的知识。

##Rspec安装
在Gemfile配置文件中，按照下面的代码进行配置
```
group :development, :test do
  gem 'rspec'
end
```
执行一下命令来安装gem包
```
bundle install
```
##测试的目录结构
在你的项目目录下新建一个spec文件夹，然后你可以在你的spec目录下新建'spec_helper.rb'文件，该文件可以定义一些你在测试中的配置，以及定义一些你的整个工程的每个测试在测之前需要执行的代码，例如下图是一个简单地目录结构，以及'spec_helper.rb'的简单定义
* 目录结构

![spec目录结构](/images/spec_1.png)

* spec_helper.rb

```
ENV['RACK_ENV'] = 'test'
require 'support/*.rb'
require 'app/config'
require 'app'
require 'database_cleaner'

Dir[File.dirname(__FILE__) + '/support/**/*.rb'].each { |f| require f }

RSpec.configure do | config |
	config.before(:suite) do
		DatabaseCleaner.strategy = :truncation
	end
	config.before(:each) do
		DatabaseCleaner.start
	end
	config.after(:each) do
		DatabaseCleaner.clean
	end
end
```
##开始写测试
还是在spec目录下，你可以只是简单地定义一个XX.rb的测试文件，如果项目比较大，你也可以用目录的结构进行组织。

* 测试的简单结构如下：

```
require 'spec_helper'

before do
	......
end

after do
	......
end

describe XXX do
	it XXX do
		......
	end
end
```
>下一节我会开始介绍如何编写自己的测试。

