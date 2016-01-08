---
layout: post
title: "use elasticsearch to rails"
date: 2016-01-08 13:49:51 +0800
comments: true
categories: 
---

### Elasticsearch introduce
If you build a website, maybe you want to add search to your application, but it is different to run a search function, you must solve a lot of questions, for example search speed, real-time. so elasticsearch is a cool tool, it can solve all questions of above, it have a advertise, let to search sample.

This is a sample demo, it will build a rails application that will use elasticsearch

### Install Elasticsearch
* for Mac labtop:

```
brew install elasticsearch
```

* start elasticsearch in the local environment:

```
elasticsearch --config=/usr/local/opt/elasticsearch/config/elasticsearch.yml
```

Now you can visit http://localhost:9200 url, if success, it's means you already install elacticsearch successful, just attention, you need to install jdk firstly, at the same time, the jdk version must > 1.8. 

### Build a rails application
This bolg just build a sample a single table application

* build application

```
rails new blog_elasticsearch
```

* install gem package

```
bundle instal
```

* use scaffold build application

```
rails g scaffold article title:string text:text
```

* generate db table

```
rake db:migrate
```

### Use elasticsearch to application

* add below gem to your Gemfile

```
gem 'elasticsearch-model' gem 'elasticsearch-rails'
```

* when import elacticsearch, you want to search your db data, there ask you must add index to you table of you want to search. commond line is(if you want to run this command, you must run you elacticsearch in your local environment)

```
rake environment elasticsearch:import:model CLASS='Article' BATCH=100 FORCE=y
```

* add below code to your /lib/tasks/elasticsearch.rake file

```
require 'elasticsearch/rails/tasks/import'
```

* It is still available if you hope to, when you update your db data, you need to add below code your model that is you search

```
include Elasticsearch::Model
include Elasticsearch::Model::Callbacks
```

### Community
* http://makeiteasy.github.io/2015/05/19/use-elasticsearch-in-rails.html
* http://blog.bringstudio.com/zheng-he-elasticsearchdao-xian-you-railsxiang-mu/


