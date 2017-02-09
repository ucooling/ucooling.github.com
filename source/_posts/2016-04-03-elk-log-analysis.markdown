---
layout: post
title: "轻量 高效 日志分析架构ELK(elasticsearh+logstash+Kibana)"
date: 2016-04-03 09:53:55 +0800
comments: true
categories: 
---

随着互联网时代的快速发展，以及大数据时代的到来，对于大数据的收集分析处理，以及用户测试开始变的尤为重要，也是很多公司在探索的方向。
所以在开始本博文之前我觉得很有必要解释依稀什么是ELK，只有有一个清晰的认识，这样在使用的时候才会更加的得心应手。

##什么是Elasticsearch
Elasticsearch是一个基于Apacha Lucene(TM)的开源搜索引擎，在开源搜索领域它一直被认为是迄今为止最先进、性能最好、功能最全的搜索引擎。Elasticsearch是使用Java语言开发并且使用Lucene作为核心来实现的以及使用简单的RESTful来隐藏Lucene的复杂性，让全文搜索变的很简单。
其实通俗的我们可以用关系型数据库来和Elasticsearch作比较。他提供的很多功能都和关系型数据库比较类似。

* 索引(Index) 
ES用来存储数据的逻辑区域，类似于关系型数据库中的Database.

* 类型(Type)
面向查询是，一个Index钟可能会有若干个Type，这个Type可以跟关系型数据库中的Table类似.

* 文档(Document)
Document是具体的存储实体，存储形式为JSON，类似于关系数据库中的某一条记录。它可以被存放在Index中或者Type中，不过当存放在Index中时必须保证它被存放在某个Type类型中，即组成一个Document Type. 

* 映射(Mapping)
Mapping是实际字段的映射关系，可以理解成Table的定义描述，可以理解成Table的定义描述，即Table Schema

综上所述： 三者的关系就是 index > type > document

ES最主要的最主要的功效就是检索查询，所以能否挖掘ES的强大之处就在于能否用好其自带的Query DSL, 这里的Query也可以类比到关系型数据库中的SQL语句，在某种意义上他们都是属于DSL的范畴，前者Query DSL是为了解决ES的检索问题而设立的，后者SQL则是为了处理关系型数据库的查询。

ES的官网提供了很详细的[文档](http://es.xiaoleilu.com/index.html)供我们学习

