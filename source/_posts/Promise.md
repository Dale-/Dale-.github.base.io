---
title: Promise
layout: post
date: 2017-08-28 22:20:01
comments: true
tags: [ES6]
categories: [ES6]
keywords: ES6 Promise
description: Promise
---

## Promise定义

> 所谓 Promise，就是一个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理。

### 三种状态

 * Pending(进行中)
 * Fulfilled(已成功)
 * Rejected(已失败)

只有异步操作的结果可以决定当前处于哪一种状态，其他任何操作都无法改变这个状态。

### 状态的改变

  * Pending => Fulfilled
  * Pending => Rejected

![](/images/promise/promises.png)

<!-- more -->  

## 三种类型

### Constructor

> Promise类似于 XMLHttpRequest，从构造函数 Promise 来创建一个新建新promise对象作为接口。

> 要想创建一个promise对象、可以使用new来调用Promise的构造器来进行实例化。

``` JavaScript

  var promise = new Promise(function(resolve, reject) {
      // 异步处理
      // 处理结束后、调用resolve 或 reject
  });

```

### Instance Method

> 对通过new生成的promise对象为了设置其值在 resolve(成功) / reject(失败)时调用的回调函数 可以使用promise.then() 实例方法

``` JavaScript
promise.then(onFulfilled, onRejected)
```

![](/images/promise/promise-workflow.png)

#### resolve(成功)时
* onFulfilled 会被调用

#### reject(失败)时
* onRejected 会被调用

onFulfilled、onRejected 两个都为可选参数。

promise.then 成功和失败时都可以使用。 另外在只想对异常进行处理时可以采用 promise.then(undefined, onRejected) 这种方式，只指定reject时的回调函数即可。 不过这种情况下 promise.catch(onRejected) 应该是个更好的选择。

``` JavaScript
promise.catch(onRejected)
```

### Static Method

像 Promise 这样的全局对象还拥有一些静态方法。

包括 Promise.all() 还有 Promise.resolve() 等在内，主要都是一些对Promise进行操作的辅助方法。

## Promise使用

```JavaScript

  var promise = new Promise(function(resolve, reject) {
    // ... some code

    if (/* 异步操作成功 */){
      resolve(value);
    } else {
      reject(error);
    }
  });

```

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

```JavaScript

  promise.then(function(value) {
    // success
  }, function(error) {
    // failure
  });

```

Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。
