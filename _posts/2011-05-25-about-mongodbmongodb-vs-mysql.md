---
id: 60
title: 'About  Mongodb|mongodb vs Mysql'
date: 2011-05-25T18:34:39+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=60
permalink: /2011/05/25/about-mongodbmongodb-vs-mysql/
categories:
  - 未分类
---
MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。  
　它的特点是高性能、易部署、易使用，存储数据非常方便。主要功能特性有：  
　　*面向集合存储，易存储对象类型的数据。  
　　*模式自由。  
　　*支持动态查询。  
　　*支持完全索引，包含内部对象。  
　　*支持查询。  
　　*支持复制和故障恢复。  
　　*使用高效的二进制数据存储，包括大型对象（如视频等）。  
　　*自动处理碎片，以支持云计算层次的扩展性  
　　*支持RUBY，PYTHON，JAVA，C++，PHP等多种语言。  
　　*文件存储格式为BSON（一种JSON的扩展）  
　　*可通过网络访问  
使用原理：  
所谓“面向集合”（Collenction-Oriented），意思是数据被分组存储在数据集中，被称为一个集合（Collenction)。每个集合在数据库中都有一个唯一的标识名，并且可以包含无限数目的文档。集合的概念类似关系型数据库（RDBMS）里的表（table），不同的是它不需要定义任何模式（schema)。  
　　模式自由（schema-free)，意味着对于存储在mongodb数据库中的文件，我们不需要知道它的任何结构定义。如果需要的话，你完全可以把不同结构的文件存储在同一个数据库里。  
　　存储在集合中的文档，被存储为键-值对的形式。键用于唯一标识一个文档，为字符串类型，而值则可以是各种复杂的文件类型。我们称这种存储形式为BSON（Binary Serialized dOcument Format）。  
MongoDb并不能完全替代MySQL 等的关系型数据库,MongoDB主要用于大型的网站,因为丢弃关系的约束就可以让服务器执行效率更高，避免数据库因为关系的繁杂给数据库带来的压力。  
使用MongoDB 会吃掉更多的硬盘，因为每条数据都要尽量不去联系其他collection(相当于mysql的表)。现在硬盘价格如此便宜，用的再多也无妨了。  
使用mongoDB会让程序员在处理关系上稍微多加了点工作。  
使用mongo灵活性非常强，因为你可以用mongo::command 定义一个查询，让数据库成为程序员的定制。  
总之 MongoDB很强大。我也正在铜鼓。