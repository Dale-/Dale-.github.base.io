---
title: Array From JavaScript API
date: 2016-01-01 09:09:08
tags: [JavaScript]
categories: [JavaScript]
keywords: Array JavaScript
description: JavaScript Array API
---

> 第一版只更新 Simple Case
> 后期随时补充特性

## Properties

### Array.length

``` bash
  var items = ["shoes", "shirts", "socks", "sweaters"];
  items.length;

  // returns 4
```

### Array.prototype

``` bash
  if (!Array.prototype.first) {
    Array.prototype.first = function() {
      return this[0];
    }
  }
```

## Methods

### Array.from() (ES6规范)

> 将一个 ArrayLike 对象或者 Iterable 对象转换成一个 Array（性能较差）

``` bash
  Array.from("foo");
  // ["f", "o", "o"]
```

> ArrayLike对象指具有数组某些行为的对象，表达出来的特征就是具有`length`属性

*Syntax*
> Array.from(arrayLike[, mapFn[, thisArg]])

[Array.from()](https://docs.angularjs.org/guide/directive)
<!-- more -->

### Array.isArray()

``` bash
  Array.isArray([1, 2, 3]);  // true
  Array.isArray({foo: 123}); // false
  Array.isArray("foobar");   // false
  Array.isArray(undefined);  // false
```

*Syntax*
> Array.isArray(obj)

### Array.of()

> 将它的任意类型的多个参数放在一个数组里并返回

``` bash
  Array.of(7);       // [7]
  Array.of(1, 2, 3); // [1, 2, 3]

  Array(7);          // [ , , , , , , ]
  Array(1, 2, 3);    // [1, 2, 3]
```

 Array.of() 和 Array 构造函数不同的是：在处理数值类型的参数时，Array.of(42) 创建的数组只有一个元素，即 42, 但 Array(42) 创建了42个元素，每个元素都是undefined。

*Syntax*
> Array.of(element0[, element1[, ...[, elementN]]])

### Array.prototype.concat()

> concat() 方法将传入的数组或非数组值与原数组合并,组成一个新的数组并返回

``` bash
  var alpha = ["a", "b", "c"];
  var numeric = [1, 2, 3];

  // 组成新数组 ["a", "b", "c", 1, 2, 3]; 原数组 alpha 和 numeric 未被修改
  var alphaNumeric = alpha.concat(numeric);
```

* 连接三个数组
``` bash
  var num1 = [1, 2, 3];
  var num2 = [4, 5, 6];
  var num3 = [7, 8, 9];

  // 组成新数组[1, 2, 3, 4, 5, 6, 7, 8, 9]; 原数组 num1, num2, num3 未被修改
  var nums = num1.concat(num2, num3);
```

*Syntax*
> var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])

### Array.prototype.copyWithin()

> copyWithin() 方法会浅拷贝数组的部分元素到同一数组的不同位置，且不改变数组的大小，返回该数组

``` bash
  [1, 2, 3, 4, 5].copyWithin(-2);
  // [1, 2, 3, 1, 2]

  [1, 2, 3, 4, 5].copyWithin(0, 3);
  // [4, 5, 3, 4, 5]

  [1, 2, 3, 4, 5].copyWithin(0, 3, 4);
  // [4, 2, 3, 4, 5]

  [1, 2, 3, 4, 5].copyWithin(0, -2, -1);
  // [4, 2, 3, 4, 5]
```

*Syntax*
> array.copyWithin(target[, start[, end]])

### Array.prototype.entries()(ES6规范)

> copyWithin() 方法返回一个 Array Iterator 对象，该对象包含数组中每一个索引的键值对

``` bash
  var array = ["a", "b", "c"];
  var eArray = array.entries();

  console.log(eArray.next().value); // [0, "a"]
  console.log(eArray.next().value); // [1, "b"]
  console.log(eArray.next().value); // [2, "c"]
```

*Syntax*
> array.entries()

### Array.prototype.every()

> every() 方法测试数组的所有元素是否都通过了指定函数的测试

``` bash
  function isBigEnough(element, index, array) {
    return (element >= 10);
  }
  var passed = [12, 5, 8, 130, 44].every(isBigEnough);
  // passed is false
  passed = [12, 54, 18, 130, 44].every(isBigEnough);
  // passed is true
```

*Syntax*
> array.every(callback[, thisArg])

### Array.prototype.fill()

> 使用 fill() 方法，可以将一个数组中指定区间的所有元素的值, 都替换成或者说填充成为某个固定的值

``` bash
  [1, 2, 3].fill(4)            // [4, 4, 4]
  [1, 2, 3].fill(4, 1)         // [1, 4, 4]
  [1, 2, 3].fill(4, 1, 2)      // [1, 4, 3]
  [1, 2, 3].fill(4, 1, 1)      // [1, 2, 3]
  [1, 2, 3].fill(4, -3, -2)    // [4, 2, 3]
  [1, 2, 3].fill(4, NaN, NaN)  // [1, 2, 3]
  Array(3).fill(4);            // [4, 4, 4]
```

*Syntax*
> array.fill(value[, start = 0[, end = this.length]])

### Array.prototype.filter()

> filter() 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组

``` bash
  function isBigEnough(element) {
    return element >= 10;
  }
  var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
  // filtered is [12, 130, 44]
```

*Syntax*
> var newArrary = array.filter(callback[, thisArg])

### Array.prototype.find()

> 如果数组中某个元素满足测试条件，find() 方法就会返回那个元素的第一个值，如果没有满足条件的元素，则返回 undefined

``` bash
  var inventory = [
      {name: 'apples', quantity: 2},
      {name: 'bananas', quantity: 0},
      {name: 'cherries', quantity: 5}
  ];

  function findCherries(fruit) {
      return fruit.name === 'cherries';
  }

  console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }
```

*Syntax*
> array.find(callback[, thisArg])

### Array.prototype.findIndex()(ES6规范)

> findIndex()方法用来查找数组中某指定元素的索引, 如果找不到指定的元素, 则返回 -1

``` bash
  function isPrime(element, index, array) {
      var start = 2;
      while (start <= Math.sqrt(element)) {
          if (element % start++ < 1) return false;
      }
      return (element > 1);
  }

  console.log( [4, 6, 8, 12].findIndex(isPrime) ); // -1, 没找到质数元素
  console.log( [4, 6, 7, 12].findIndex(isPrime) ); // 2
```

*Syntax*
> array.findIndex(callback[, thisArg])

### Array.prototype.forEach()

> findIndex()方法用来查找数组中某指定元素的索引, 如果找不到指定的元素, 则返回 -1

``` bash
  function logArrayElements(element, index, array) {
      console.log("a[" + index + "] = " + element);
  }
  [2, 5, 9].forEach(logArrayElements);
  // logs:
  // a[0] = 2
  // a[1] = 5
  // a[2] = 9
```

*Syntax*
> array.forEach(callback[, thisArg])

### Array.prototype.includes()

> includes() 方法用来判断当前数组是否包含某指定的值，如果是，则返回 true，否则返回 false

``` bash
  [1, 2, 3].includes(2);     // true
  [1, 2, 3].includes(4);     // false
  [1, 2, 3].includes(3, 3);  // false
  [1, 2, 3].includes(3, -1); // true
  [1, 2, NaN].includes(NaN); // true
```

*Syntax*
> var boolean = array.includes(searchElement[, fromIndex])

### Array.prototype.indexOf()

> indexOf()方法返回给定元素能找在数组中找到的第一个索引值，否则返回-1

``` bash
  var array = [2, 5, 9];
  array.indexOf(2);     // 0
  array.indexOf(7);     // -1
  array.indexOf(9, 2);  // 2
  array.indexOf(2, -1); // -1
  array.indexOf(2, -3); // 0
```

*Syntax*
> array.indexOf(searchElement[, fromIndex = 0])

### Array.prototype.join()

> join() 方法将数组中的所有元素连接成一个字符串

``` bash
  var a = ['Wind', 'Rain', 'Fire'];
  var myVar1 = a.join();      // myVar1的值变为"Wind,Rain,Fire"
  var myVar2 = a.join(', ');  // myVar2的值变为"Wind, Rain, Fire"
  var myVar3 = a.join(' + '); // myVar3的值变为"Wind + Rain + Fire"
  var myVar4 = a.join('');    // myVar4的值变为"WindRainFire"
```

*Syntax*
> str = array.join([separator = ','])

### Array.prototype.join()[ES6规范]

> 数组的 keys() 方法返回一个数组索引的迭代器

``` bash
  var array = ["a", "b", "c"];
  var iterator = array.keys();

  console.log(iterator.next()); // { value: 0, done: false }
  console.log(iterator.next()); // { value: 1, done: false }
  console.log(iterator.next()); // { value: 2, done: false }
  console.log(iterator.next()); // { value: undefined, done: true }
```

*Syntax*
> array.keys()

### Array.prototype.lastIndexOf()

> lastIndexOf() 方法返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 fromIndex 处开始

``` bash
  var array = ["a", "b", "c"];
  var iterator = array.keys();

  console.log(iterator.next()); // { value: 0, done: false }
  console.log(iterator.next()); // { value: 1, done: false }
  console.log(iterator.next()); // { value: 2, done: false }
  console.log(iterator.next()); // { value: undefined, done: true }
```

*Syntax*
> array.lastIndexOf(searchElement[, fromIndex = array.length - 1])

### Array.prototype.map()

> map() 方法返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组

``` bash
  var numbers = [1, 4, 9];
  var roots = numbers.map(Math.sqrt);
  /* roots的值为[1, 2, 3], numbers的值仍为[1, 4, 9] */
```

*Syntax*
> array.map(callback[, thisArg])

### Array.prototype.pop()

> pop() 方法删除一个数组中的最后的一个元素，并且返回这个元素

``` bash
  var myFish = ["angel", "clown", "mandarin", "surgeon"];

  console.log("myFish before: " + myFish); // "myFish before: angel,clown,mandarin,surgeon"

  var popped = myFish.pop();

  console.log("myFish after: " + myFish); // "myFish after: angel,clown,mandarin"
  console.log("Removed this element: " + popped); // "Removed this element: surgeon"
```

*Syntax*
> array.pop()

### Array.prototype.push()

> push() 方法添加一个或多个元素到数组的末尾，并返回数组新的长度（length 属性值）

``` bash
  var numbers = [1, 2, 3];
  numbers.push(4);

  console.log(numbers); // [1, 2, 3, 4]

  numbers.push(5, 6, 7);

  console.log(numbers); // [1, 2, 3, 4, 5, 6, 7]
```

*Syntax*
> array.push(element1, ..., elementN)

### Array.prototype.reduce()

> reduce() 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值

``` bash
  var sum = [0, 1, 2, 3].reduce(function(a, b) {
  return a + b;
  }, 0);
  // sum is 6

  var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
  return a.concat(b);
  }, []);
  // flattened is [0, 1, 2, 3, 4, 5]
```

*Syntax*
> array.reduce(callback,[initialValue])

### Array.prototype.reduceRight()

> reduceRight() 方法接受一个函数作为累加器（accumulator），让每个值（从右到左，亦即从尾到头）缩减为一个值。（与 reduce() 的执行方向相反）

* 求一个数组中所有值的和
``` bash
  var total = [0, 1, 2, 3].reduceRight(function(a, b) {
      return a + b;
  });
  // total == 6
```

* 扁平化（flatten）一个元素为数组的数组
``` bash
  var flattened = [[0, 1], [2, 3], [4, 5]].reduceRight(function(a, b) {
      return a.concat(b);
  }, []);
  // flattened is [4, 5, 2, 3, 0, 1]
```

*Syntax*
> array.reduceRight(callback[, initialValue])

> reduce 与 reduceRight 之间的区别
``` bash
  var a = ["1", "2", "3", "4", "5"];
  var left  = a.reduce(function(previous, current)      { return previous + current; });
  var right = a.reduceRight(function(previous, current) { return previous + current; });

  console.log(left);  // "12345"
  console.log(right); // "54321"
```

### Array.prototype.reverse()

> reverse() 方法颠倒数组中元素的位置。第一个元素会成为最后一个，最后一个会成为第一个

* 颠倒数组中的元素
``` bash
  var myArray = ['one', 'two', 'three'];
  myArray.reverse();

  console.log(myArray) // ['three', 'two', 'one']
```

*Syntax*
>  array.reverse()

### Array.prototype.shift()

> shift() 方法删除数组的`第一个`元素，并返回这个元素。该方法会改变数组的长度

* 移除数组中的一个元素
``` bash
  var myFish = ['angel', 'clown', 'mandarin', 'surgeon'];

  console.log('调用 shift 之前: ' + myFish);
  // "调用 shift 之前: angel,clown,mandarin,surgeon"

  var shifted = myFish.shift();

  console.log('调用 shift 之后: ' + myFish);
  // "调用 shift 之后: clown,mandarin,surgeon"

  console.log('被删除的元素: ' + shifted);
  // "被删除的元素: angel"
```

*Syntax*
>  array.shift()

### Array.prototype.slice()

> slice() 方法会浅复制（shallow copy）数组的一部分到一个新的数组，并返回这个新数组

``` bash
  // Our good friend the citrus from fruits example
  var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
  var citrus = fruits.slice(1, 3);

  // puts --> ["Orange","Lemon"]
```

*Syntax*
>  array.slice([begin[, end]])

### Array.prototype.some()

> some() 方法测试数组中的某些元素是否通过了指定函数的测试

``` bash
  function isBigEnough(element, index, array) {
    return (element >= 10);
  }
  var passed = [2, 5, 8, 1, 4].some(isBigEnough);
  // passed is false
  passed = [12, 5, 8, 1, 4].some(isBigEnough);
  // passed is true
```

*Syntax*
>  array.some(callback[, thisArg])

### Array.prototype.sort()

> sort() 方法对数组的元素做原地的排序，并返回这个数组。 sort 排序可能是不稳定的。默认按照字符串的Unicode码位点（code point）排序

``` bash
  var fruit = ['cherries', 'apples', 'bananas'];
  fruit.sort(); // ['apples', 'bananas', 'cherries']

  var scores = [1, 10, 2, 21];
  scores.sort(); // [1, 10, 2, 21]
  // Watch out that 10 comes before 2,
  // because '10' comes before '2' in Unicode code point order.

  var things = ['word', 'Word', '1 Word', '2 Words'];
  things.sort(); // ['1 Word', '2 Words', 'Word', 'word']
  // In Unicode, numbers come before upper case letters,
  // which come before lower case letters.
```

*Syntax*
>  array.sort([compareFunction])

### Array.prototype.splice()

> splice() 方法用新元素替换旧元素，以此修改数组的内容

* 移除数组中的一个元素
``` bash
  var myFish = ["angel", "clown", "mandarin", "surgeon"];

  //从第 2 位开始删除 0 个元素，插入 "drum"
  var removed = myFish.splice(2, 0, "drum");
  //运算后的 myFish:["angel", "clown", "drum", "mandarin", "surgeon"]
  //被删除元素数组：[]，没有元素被删除

  //从第 3 位开始删除 1 个元素
  removed = myFish.splice(3, 1);
  //运算后的myFish：["angel", "clown", "drum", "surgeon"]
  //被删除元素数组：["mandarin"]

  //从第 2 位开始删除 1 个元素，然后插入 "trumpet"
  removed = myFish.splice(2, 1, "trumpet");
  //运算后的myFish: ["angel", "clown", "trumpet", "surgeon"]
  //被删除元素数组：["drum"]

  //从第 0 位开始删除 2 个元素，然后插入 "parrot", "anemone" 和 "blue"
  removed = myFish.splice(0, 2, "parrot", "anemone", "blue");
  //运算后的myFish：["parrot", "anemone", "blue", "trumpet", "surgeon"]
  //被删除元素的数组：["angel", "clown"]

  //从第 3 位开始删除 2 个元素
  removed = myFish.splice(3, Number.MAX_VALUE);
  //运算后的myFish: ["parrot", "anemone", "blue"]
  //被删除元素的数组：["trumpet", "surgeon"]
```

*Syntax*
>  array.splice(start, deleteCount[, item1[, item2[, ...]]])

### Array.prototype.toLocaleString()

> toLocaleString() 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 toLocaleString 方法转成字符串，这些字符串将使用一个特定语言环境的字符串（例如一个逗号 ","）隔开

``` bash
  var number = 1337;
  var date = new Date();
  var myArray = [number, date, "foo"];

  var str = myArray.toLocaleString();

  console.log(str);
  // 输出 "1337,2015/2/27 下午8:29:04,foo"
  // 假定运行在中文（zh-CN）环境，北京时区
```

*Syntax*
>  array.toLocaleString()

### Array.prototype.toString()

> toString() 返回一个字符串，表示指定的数组及其元素

``` bash
  var monthNames = ['Jan', 'Feb', 'Mar', 'Apr'];
  var myVar = monthNames.toString(); // assigns "Jan,Feb,Mar,Apr" to myVar.
```

*Syntax*
>  array.toString()

### Array.prototype.unshift()

> unshift() 方法在数组的开头添加一个或者多个元素，并返回数组新的 length 值

``` bash
  var array = [1, 2];

  array.unshift(0); //result of call is 3, the new array length
  //array is [0, 1, 2]

  array.unshift(-2, -1); // = 5
  //array is [-2, -1, 0, 1, 2]

  array.unshift( [-3] );
  //array is [[-3], -2, -1, 0, 1, 2]
```

*Syntax*
>  array.unshift(element1, ..., elementN)

### Array.prototype.values()

> values() 方法返回一个新的`Array Iterator`对象，该对象包含数组每个索引的值

``` bash
  var array = ['w', 'y', 'k', 'o', 'p'];
  var eArray = array.values();
  // 您的浏览器必须支持 for..of 循环
  // 以及 let —— 将变量作用域限定在 for 循环中
  for (let letter of eArray) {
    console.log(letter);
  }
```

*Syntax*
>  array.values()
