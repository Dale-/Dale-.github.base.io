---
title: Closures
layout: post
date: 2016-12-14 17:04:34
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: JavaScript Closures
description: JavaScript闭包
---

## 变量作用域

变量的作用域有两种：全局变量和局部变量。
在JavaScript中，函数内部可以直接读取全局变量。

``` bash
var n=999;
function f1(){
　　alert(n);
}
f1(); // 999
```

<!-- more -->

当然，在函数外部无法读取函数内的局部变量。

``` bash
function f1(){
　 var n=999;
}
alert(n); // error
```

> 这里有一个地方需要注意，函数内部声明变量的时候，一定要使用var命令。
  如果不用的话，你实际上声明了一个全局变量！

``` bash
function f1(){
  n=999;
}
f1();
alert(n); // 999
```

## 如何从外部读取局部变量？

在函数的内部，再定义一个函数。

``` bash
function f1(){
　　var n=999;
　　function f2(){
　　　　alert(n); // 999
　　}
}
```

> 在上面的代码中，函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1就是不可见的。这就是Javascript语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

> 既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！

``` bash
function f1(){
　　var n=999;
　　function f2(){
　　　　alert(n);
　　}
　　return f2;
}
var result=f1();
result(); // 999
```

## 什么是闭包

上一节代码中的f2函数，就是闭包。

 * Wikipedia

 > Closures (also lexical closures or function closures) are techniques for implementing lexically scoped name binding in languages with first-class functions. Operationally, a closure is a record storing a function[a] together with an environment:[1] a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or reference to which the name was bound when the closure was created.[b] A closure—unlike a plain function—allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope.
 > 闭包（也称词法闭包或函数闭包）是指一个函数或函数的引用，与一个引用环境绑定在一起。这个引用环境是一个存储该函数每个非局部变量（也叫自由变量）的表。

 * Closures

 > 闭包，不同于一般的函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量。

 > 闭包就是能够读取其他函数内部变量的函数。

 > 由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

## Well

 * 灵活
 * 方便
 * 封装

## Less Well

 * 空间浪费
 * 内存泄漏
 * 性能消耗

##  闭包的三个事实

> * JavaScript允许你引入函数以外定义的变量

``` bash
function makeNoodles() {
    var noodlesType = 'Lamian'
    function make(ingredients) {
        return noodlesType + ' with ' + ingredients;
    }
    return make('beef');
}

makeNoodles(); // 'Lamian with beef'
// 注意 make函数可以访问外部变量 noodlesType 和 ingredients
```

> * 即使外部函数已经执行结束, 内部函数依然可以引用在外部函数所定义的变量

``` bash
function makeNoodles() {
    var noodlesType = 'Lamian'
    function make(ingredients) {
        return noodlesType + ' with ' + ingredients;
    }
    return make;
}

var f = makeNoodles();
f('beef'); // 'Lamian with beef'
f('fish'); // 'Lamian with fish'
f('eggs and tomatoes'); // 'Lamian with eggs and tomatoes'
// 注意 与第一个例子不同的是, makeNoodles 函数返回 make 函数, 此时外部函数执行已经结束, 但是内部函数 make
// 依然可以访问 noodlesType 变量
```

基于上面的例子, 我们可以写的更通用.

``` bash
function makeNoodles(noodlesType) {
    function make(ingredients) {
        return noodlesType + ' with ' + ingredients;
    }
    return make;
}

var makeLamian = makeNoodles('Lamian');
makeLamian('beef'); // 'Lamian with beef'
makeLamian('fish'); // 'Lamian with fish'
makeLamian('eggs and tomatoes'); // 'Lamian with eggs and tomatoes'

var makeRiceNoodles = makeNoodles('Rice noodles');
makeRiceNoodles('beef'); // 'Rice noodles with beef'
makeRiceNoodles('fish'); // 'Rice noodles with fish'
makeRiceNoodles('eggs and tomatoes'); // 'Rice noodles with eggs and tomatoes'
```

> * 最后一个, 闭包可以更新外部函数定义的变量

``` bash
function counter() {
    var num = 0;
    return {
        increase: function() {
           num++;
        },
        currentNum: function() {
            return num;
        }
    };
}

var counter1 = counter();
counter1.increase();
counter1.increase();
counter1.currentNum(); // 2

var counter2 = counter();
counter2.currentNum(); // 0
```

##  闭包的中的`this`

* 代码片段一

``` bash
var name = "The Window";
var object = {
　　name : "My Object",
　　getNameFunc : function(){
　　　　return function(){
　　　　　　return this.name;
　　　　};
　　}
};
alert(object.getNameFunc()());
```
* 代码片段二

``` bash
var name = "The Window";
var object = {
　　name : "My Object",
　　getNameFunc : function(){
　　　　var that = this;
　　　　return function(){
　　　　　　return that.name;
　　　　};
　　}
};
alert(object.getNameFunc()());
```
