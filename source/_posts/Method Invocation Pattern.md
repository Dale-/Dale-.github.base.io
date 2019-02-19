---
title: Method Invocation Pattern
layout: post
date: 2016-6-27 18:05:41
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: JavaScript
description: 调用方式
---

## 方法调用（Method Invocation Pattern）

在面向对象程序设计中，当函数（Function）作为对象属性时被称为方法（Method）。 方法被调用时this会被绑定到对应的对象。在JavaScript中有两种语法可以完成方法调用： a.func()和a['func']()。
```bash
  var obj = {
      val: 0,
      count: function(){
          this.val ++;
          console.log(this.val);
      }
  };

  obj.count();    // 1
  obj.count();    // 2
```
值得注意的是，this到obj的绑定属于极晚绑定（very late binding）， 绑定发生在调用的那一刻。这使得JavaScript函数在被重用时有极大的灵活性。

<!-- more -->

## 函数调用（Function Invocation Pattern）

当函数不是对象属性时，它就会被当做函数来调用，比如add(2,3)。 此时this绑定到了全局对象global。

在那些 JavaScript 的优秀特性一文中曾提到， JavaScript的编译（预处理）需要global对象来链接所有全局名称。
其实this绑定到global是JavaScript的一个设计错误（可以说是最严重的错误）， 它导致了对象方法不能直接调用内部函数来做一些辅助工作， 因为内不函数里的this的绑定到了global。 所以如果要重新设计语言，方法调用的this应该绑定到上一级函数的this。

然而共有方法总是需要调用内部辅助函数，于是产生了这样一个非常普遍的解决方案：
```bash
  man.love = function(){
      var self = this;
      function fuck(){
          self.happy++;
      }
      function marry(){
          self.happy--;
      }
      fuck() && marry();
  }
```

有些场景下用Function.prototype.bind会更加方便：

```bash
  man.love = function(){
      function fuck(girl1, girl2, ...){
          this.happy++;
      }
      fuck.bind(this)();
      ...
  }
```
## 构造函数调用（Constructor Invocation Pattern）

JavaScript中，那些用来new对象的函数成为构造函数。

JavaScript采用原型继承方式。这意味着一个对象可以从另一个对象直接继承属性， JavaScript是class free的~ 但JavaScript为了迎合主流的基于类声明的继承方式， 同时也给出了构造函数机制：使用new关键字，便会创建一个对象， 根据prototype属性创建原型链，并以该对象为this执行指定的（构造）函数。

```bash
  function Man(name, age){
      this.sex = 'man';
      this.name = name;
      this.age = age;
  }
  Man.prototype.fuck = function(girl1, girl2, ...){}
  var man = new Man();
  man.fuck();
```

当构造函数有很多参数时，它们的顺序很难记住，所以通常使用对象进行传参：

```bash
  var man = new Man({
      name: 'bob',
      age: 18
  });
```
## apply调用（Apply Invocation Pattern）

apply模式相比之前的模式，没有那么糟糕。apply方法允许，通过传递参数数组给函数来手动调用函数，明确设置this参数。因为函数是一等公民，他们也是对象，因此也可以运行方法（函数）。事实上，每一个function都指向Function.prototype,因此方法可以很容易扩展函数。apply方法就是一个函数扩展方法-我的猜想-它定义在Function.prototype中。

apply有两个参数：第一个参数是this参数绑定的对象，第二个参数是一个数组，它被映射为第一个对象的参数：

```bash
  var add = function(num1, num2) {
        return num1+num2;
  }

  array = [3,4];
  add.apply(null,array); //7
```

上面的例子中，this为空（该函数不是一个对象，所以它不需要）和数组为num1与num2。第一个参数可以更有趣：
```bash
  var obj = {
      data:'Hello World'
  }

  var displayData = function() {
      alert(this.data);
  }

  displayData(); //undefined
  displayData.apply(obj); //Hello World
```

上面的例子使用apply绑定this到obj。结果产生一个this.data值。apply的实际应用价值,就是能明确分配一个值给this.。要是没有这个功能，我们可以直接使用（）来调用函数。

JavaScript还有种调用方法是call，类似apply方法,不过它传递的不是一个参数数组,而是一个参数列表。如果JavaScript可以实现的函数重载，我认为call应该是apply方法的重载。因此，人们谈论的apply和call其实是一样的。

## 总结

总结下this. 1. 对于obj.fun()此类方法函数调用,哪个对象实例(obj)调用this所在的函数(fun),this指向那个对象实例

1. 对于函数调用,this指向全局对象.(其实可以看做上一种,是window对象的方法函数,全局对象调用,则指向全局对象).避免这种语言设计不合理的一个小技巧就是,进入函数后就申明一个变量_this,赋值this给它(_this),保存下来.

2. 对于构造函数调用,若该函数返回一个简单类型（number, string, boolean, null or undefined），忽略return值，返回this（指向新的对象）;如果该函数返回一个object实例（简单类型以外的任何类型），那么将返回该对象。

3. apply和call调用,this指向apply和call第一参数.即手动设置this的值.
