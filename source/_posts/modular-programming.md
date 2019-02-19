---
title: Modular Programming
layout: post
date: 2016-12-14 09:45:51
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: Modular
description: Modular Programming
---

## 模块的规范

先想一想，为什么模块很重要？

因为有了模块，我们就可以更方便地使用别人的代码，想要什么功能，就加载什么模块。

但是，这样做有一个前提，那就是大家必须以同样的方式编写模块，否则你有你的写法，我有我的写法，岂不是乱了套！
考虑到Javascript模块现在还没有官方规范，这一点就更重要了。

* 组件的复用，降低开发成本和维护成本
* 组件单独开发，方便分工合作
* 模块化遵循标准，方便自动化依赖管理，代码优化，部署

目前，AMD,CMD,CommonJS是目前最常用的三种模块化书写规范。

<!-- more -->

## CommonJS

2009年，美国程序员Ryan Dahl创造了node.js项目，将javascript语言用于服务器端编程。
![](/images/modular-programming/nodejs.jpg)
这标志"Javascript模块化编程"正式诞生。
NodeJS的模块系统，就是参照CommonJS规范实现的。

在CommonJS中，有一个全局性方法require()，用于加载模块。
假定有一个时钟模块clock.js，就可以像下面这样加载：

``` bash
var clock = require('clock');
clock.start();
```

这种写法适合服务端，因为在服务器读取模块都是在本地磁盘，加载速度很快。
但是如果在客户端，加载模块的时候有可能出现“假死”状况。比如上面的例子中clock的
调用必须等待clock.js请求成功，加载完毕。那么，能不能异步加载模块呢？

## AMD

AMD，即 (Asynchronous Module Definition)，这种规范是异步的加载模块，requireJs应用了这一规范。
先定义所有依赖，然后在加载完成后的回调函数中执行：

``` bash
require([module], callback);
```

用AMD写上一个模块：

``` bash
require(['clock'],function(clock){
  clock.start();
});
```

目前，主要有两个Javascript库实现了AMD规范：require.js和curl.js。

AMD虽然实现了异步加载，但是开始就把所有依赖写出来是不符合书写的逻辑顺序的，
能不能像commonJS那样用的时候再require，而且还支持异步加载后再执行呢？

## CMD

CMD (Common Module Definition)，
CMD则是依赖就近，用的时候再require。seaJS 是它的实现，它写起来是这样的：

``` bash
define(function(require, exports, module) {
   var clock = require('clock');
   clock.start();
});
```

用AMD写上一个模块：

``` bash
require(['clock'],function(clock){
  clock.start();
});
```
AMD和CMD最大的区别是对依赖模块的执行时机处理不同，
而不是加载的时机或者方式不同，二者皆为异步加载模块。

* 静态加载:在编译阶段进行,把所有需要的依赖打包到一个文件中
* 动态加载:在运行时加载依赖

AMD标准是动态加载的代表，而CommonJS是静态加载的代表。AMD的目的是用在浏览器上，所以是异步加载的。而NodeJS是运行在服务器上的，同步加载的方式显然更容易被人接收，所以使用了CommonJS。同样的道理，如果静态加载，那就使用同步的加载方式，如果动态加载就必须用异步的加载方式。

AMD依赖前置，js可以方便知道依赖模块是谁，立即加载；而CMD就近依赖，
需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，
牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。
