---
title: Loops And Iteration In JavaScript
layout: post
date: 2017-04-23 21:32:36
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: loops iteration
description: how to use different type of loops and iteration in JavaScript
---

## 遍历

### for
``` bash
  for (let index = 0, length = array.length; index < length; index++) {
    if (index == 2) {
      // return;    // 函数执行被终止
      // break;     // 循环被终止
      continue;     // 循环被跳过
    }
  }
```

> Attention

> * 如果使用var在for循环中声明index变量，那么在循环结束后index依然存在作用域（函数作用域）中，这种情况下可以使用函数自执行的方法将其隔离开来()();当然如果选用let声明，循环后index不会保留，因为let是块级作用域。

> * 避免使用 for (let i = index; i < array.length; i++)的方式，这种方式每次的length每次都会被计算，效率比上面例子低。

> * 跳出循环：return、break、continue都支持

<!-- more -->

### for in
`for(let item in array|object){}` 可以用来遍历数组和对象

* 遍历数组时，item表示索引值
* 遍历对象时，item表示key值

```bash
  for (let key in object) {
    if (key == 2) {
      return;       // 函数执行被终止
      // break;     // 循环被终止
      // continue;  // 循环被跳过
    }
  }
```

> Attention

> * key的作用域问题，同上

> * for..in非常适合遍历自变量对象,key是对象的每一个属性,object是要遍历的对象（键值对形式）

> * 支持数组、对象甚至是函数等。

### forEach
`array.forEach(arg => {})}`

```bash
  array.forEach(function(value, index) {
    if (value === 'javascript') {
      return;       // 循环被跳过
      // break;     // 报错
      // continue;  // 报错
    }
  })
```

> Attention

> * 回调函数中有2个参数，分别表示值和索引。

> * forEach无法遍历对象，无法使用break,continue跳出循环

> * 第一个参数：每个元素值，第二个参数：数组的下标，第三个参数：当前数组对象

> * 可以添加第二个参数，为一个数组，而毁掉函数中的this会指向这个数组。没有第二个参数，this会指向window.
```bash
  var newArray = [];
  newArray.forEach(function(value, index) {
    this.push(value);
  }, newArray)
```

### for..of（ES6新增）

* 第一个变量：数组中的每一个元素
* 第二个变量：待遍历的数组对象

```bash
  for( var element of array ) {
  console.log( element.name );
  }
```

```bash
  var list = new Map().set('a', 1).set('b', 2).set('c', 3);
  for (var [key, value] of list) {
    console.log(key + '=>' + value);
  }
```

### keys（ES6新增）
```bash
  for (let index of ['a', 'b'].keys()) {
      console.log(index);
  }
  // 0
  // 1
```

### values（ES6新增）
```bash
  for (let element of ['a', 'b'].values()) {
    console.log(element);
  }
  // 'a'
  // 'b'
```

### entries（ES6新增）
```bash
  for (let [index, element] of ['a', 'b'].entries()) {
      console.log(index, element);
  }
  // 0 "a"
  // 1 "b"
```

> Attention

> * 除了Map结构外，不能取到键名。不能用来遍历普通对象。

> * 在遍历取值上与for..in形成互补，一个在遍历中取键名，另一个取值。另一个优点是，它可以遍历任何部署了Iterator接口的数据结构，甚至是JavaScript的数据类型（自定义的数据结构）。

## Array方法

`var array = [1, 2, 3, 4, 5];`
![](/images/iteration/iteration.png)

### forEach
``` bash
  array.forEach(function(item, index) {
    console.log(item);
  })
```
![](/images/iteration/forEach.png)

> 场景： 让数组中的每一项，做一件事

### map

> Mapping is a fundamental functional programming technique for operating on all of the elements in an array and producing another array of the same length with transformed contents.

``` bash
  var newArray = array.map(function(item, index) {
    return item * 2;
  })
```
![](/images/iteration/map.png)

> 场景

* 数组中的每项做转换（计算比较常用），结果生成了一个新数组，并且新数组的长度和原数组一致。
* 第一个参数：value, 第二个参数key值,第三个参数当前对象

![](/images/iteration/mapV.png)

> A Simple Case

``` bash
  var animals = ["cat","dog","fish"];
  var lengths = [];
  var item;
  var count;
  var loops = animals.length;
  for (count = 0; count < loops; count++){
    item = animals[count];
    lengths.push(item.length);
  }
  console.log(lengths); //[3, 3, 4]
```

> Use Map

``` bash
  var animals = ["cat","dog","fish"];
  var lengths = animals.map(function(animal) {
    return animal.length;
  });
  console.log(lengths); //[3, 3, 4]
```

> Refractory

* 我们声明了一个lengths变量，并且我们把一个匿名函数的值直接赋值给它。
* 我们上面使用的方式要比原始的更简短，并且声明更少的变量
1. 更少的变量意味着在全局作用域中更少的污染，更少有机会与其他使用相同名称的变量产生冲突
2. 当你深入函数式变成后，你会倾向于使用常量或者不可变变量，这样永远都不会存在太晚使用的问题

``` bash
  var animals = ["cat","dog","fish"];
  function getLength(word) {
    return word.length;
  }
  console.log(animals.map(getLength)); //[3, 3, 4]
```

### filter
``` bash
  var newArray = array.map(function(item, index) {
    return item > 3;
  })
```
![](/images/iteration/filter.png)

> 场景：按照规则筛选出符合条件的项，结果也生成了一个新数组，但新数组的长度小于或等于原数组。

![](/images/iteration/filterV.png)

### find
``` bash
  var newArray = array.find(function(item) {
    return item > 3;
  })
```

> 场景：按照规则查找出符合条件的项，返回符合条件的第一个元素。

### reduce
``` bash
  var newArray = array.reduce(function(prev, next) {
    return prev + next;
  })
```
![](/images/iteration/reduce.png)

> 场景： 让数组中的前项和后项做某种计算，并累计最终值，最后生成的结果可以是任何类型的变量：一个新数组，一个新对象，一个新布尔值…

![](/images/iteration/reduceV.png)

> A Simple Case

``` bash
  var animals = ["cat","dog","fish"];
  var total = 0;
  var item;
  for (var count = 0, loops = animals.length; count < loops; count++){
    item = animals[count];
    total += item.length;
  }
  console.log(total); //10
```

> Use Map

``` bash
  var animals = ["cat","dog","fish"];
  var total = animals.reduce(function(sum, word) {
    return sum + word.length;
  }, 0);
  console.log(total);
```

> Refractory

``` bash
  var animals = ["cat","dog","fish"];
  var addLength = function(sum, word) {
    return sum + word.length;
  };
  var total = animals.reduce(addLength, 0);
  console.log(total);
```

### every
``` bash
  var newArray = array.every(function(item, index) {
    return item > 0;
  })
```
![](/images/iteration/every.png)

> 场景：检测是否数组中的所有项都符合规则

### some
``` bash
  var newArray = array.some(function(item, index) {
    return item > 1;
  })
```
![](/images/iteration/some.png)

> 场景：数组中有一项符合规则，则返回true

参考文章：[Using Map and Reduce in Functional JavaScript](https://www.sitepoint.com/map-reduce-functional-javascript/)
