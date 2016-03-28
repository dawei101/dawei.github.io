---
id: 18
title: "@property （nonatomic,retain)中的nonatom和retain是什么意思"
date: 2012-04-01T14:38:16+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=18
permalink: /2012/04/01/property-%ef%bc%88nonatomicretain%e4%b8%ad%e7%9a%84nonatom%e5%92%8cretain%e6%98%af%e4%bb%80%e4%b9%88%e6%84%8f%e6%80%9d/
categories:
  - 未分类
---
@property是一个属性访问声明，扩号内支持以下几个属性：  
1，getter=getterName，setter=setterName，设置setter与getter的方法名  
2，readwrite,readonly，设置可供访问级别  
2，assign，setter方法直接赋值，不进行任何retain操作，为了解决原类型与环循引用问题  
3，retain，setter方法对参数进行release旧值再retain新值，所有实现都是这个顺序(CC上有相关资料)  
4，copy，setter方法进行Copy操作，与retain处理流程一样，先旧值release，再Copy出新的对象，retainCount为1。这是为了减少对上下文的依赖而引入的机制。  
5，nonatomic，非原子性访问，不加同步，多线程并发访问会提高性能。注意，如果不加此属性，则默认是两个访问方法都为原子型事务访问。锁被加到所属对象实例级。  
@synthesize xxx; 为这个心属性自动生成读写函数；
