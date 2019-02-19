---
title: Function From JavaScript API
date: 2017-01-17 09:09:08
tags: [JavaScript]
categories: [JavaScript]
keywords: Function JavaScript
description: JavaScript Function API
---

> 第一版只更新 Simple Case
> 后期随时补充特性

## Properties

### Function.length

> length 属性指明函数的形参个数

``` bash
  console.log(Function.length); /* 1 */

  console.log((function()        {}).length); /* 0 */
  console.log((function(a)       {}).length); /* 1 */
  console.log((function(a, b)    {}).length); /* 2 etc. */
  console.log((function(...args) {}).length); /* 0, rest parameter is not counted */
```

### Function.prototype

> Function.prototype 属性存储了 Function 的原型对象
Function对象继承自 Function.prototype 属性。因此，Function.prototype 不能被修改

> 属性

* Function.length

获得函数的接收参数个数。

* Function.prototype.constructor

声明函数的原型构造方法

> 方法

* Function.prototype.apply()

在一个对象的上下文中应用另一个对象的方法；参数能够以数组形式传入。

* Function.prototype.bind()

bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入bind(）
方法的第一个参数作为this,传入bind()方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照
顺序作为原函数的参数来调用原函数。

* Function.prototype.call()

在一个对象的上下文中应用另一个对象的方法；参数能够以列表形式传入。

## Method
### Function.prototype.apply()

> apply() 方法在指定 this 值和参数（参数以数组或类数组对象的形式存在）的情况下调用某个函数

``` bash
  Function.prototype.construct = function (aArgs) {
    var oNew = Object.create(this.prototype);
    this.apply(oNew, aArgs);
    return oNew;
  };

  // 注意: 上面使用的Object.create()方法相对来说比较新。另一种可选的方法是使用闭包，请考虑如下替代方法

  Function.prototype.construct = function(aArgs) {
  var fConstructor = this, fNewConstr = function() {
    fConstructor.apply(this, aArgs);
  };
  fNewConstr.prototype = fConstructor.prototype;
  return new fNewConstr();
  };

  function MyConstructor () {
    for (var nProp = 0; nProp < arguments.length; nProp++) {
        this["property" + nProp] = arguments[nProp];
    }
}

  var myArray = [4, "Hello world!", false];
  var myInstance = MyConstructor.construct(myArray);

  console.log(myInstance.property1);                // logs "Hello world!"
  console.log(myInstance instanceof MyConstructor); // logs "true"
  console.log(myInstance.constructor);              // logs "MyConstructor"

```

 Array.of() 和 Array 构造函数不同的是：在处理数值类型的参数时，Array.of(42) 创建的数组只有一个元素，即 42, 但 Array(42) 创建了42个元素，每个元素都是undefined。

 > Attention

 该函数的语法与call()方法的语法几乎完全相同，唯一的区别在于，call()方法接受的是一个参数列表，
 而apply()方法接受的是一个包含多个参数的数组（或类数组对象）。

*Syntax*
> fun.apply(thisArg[, argsArray])
