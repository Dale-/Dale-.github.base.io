---
title: Object From JavaScript API
date: 2016-01-02 23:37:00
tags: [JavaScript]
categories: [JavaScript]
keywords: Object JavaScript
description: JavaScript Object API
---

> 第一版只更新 Simple Case
> 后期随时补充特性

## Properties

### Object.prototype

``` bash
  var Person = function() {
    this.canTalk = true;
    this.greet = function() {
      if (this.canTalk) {
        console.log('Hi, I\'m ' + this.name);
      }
    };
  };

  var Employee = function(name, title) {
    this.name = name;
    this.title = title;
    this.greet = function() {
      if (this.canTalk) {
        console.log("Hi, I'm " + this.name + ", the " + this.title);
      }
    };
  };
  Employee.prototype = new Person();

  var Customer = function(name) {
    this.name = name;
  };
  Customer.prototype = new Person();

  var Mime = function(name) {
    this.name = name;
    this.canTalk = false;
  };
  Mime.prototype = new Person();

  var bob = new Employee('Bob', 'Builder');
  var joe = new Customer('Joe');
  var rg = new Employee('Red Green', 'Handyman');
  var mike = new Customer('Mike');
  var mime = new Mime('Mime');
  bob.greet();
  joe.greet();
  rg.greet();
  mike.greet();
  mime.greet();
```
<!-- more -->
## Methods

### Object.assign()

> Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象

``` bash
  var obj = { a: 1 };
  var copy = Object.assign({}, obj);
  console.log(copy); // { a: 1 }
```

* 合并 objects
``` bash
  var o1 = { a: 1 };
  var o2 = { b: 2 };
  var o3 = { c: 3 };

  var obj = Object.assign(o1, o2, o3);
  console.log(obj); // { a: 1, b: 2, c: 3 }
  console.log(o1);  // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
```

* 拷贝 symbol 类型的属性
``` bash
  var o1 = { a: 1 };
  var o2 = { [Symbol("foo")]: 2 };

  var obj = Object.assign({}, o1, o2);
  console.log(obj); // { a: 1, [Symbol("foo")]: 2 }
```

*Syntax*
>  Object.assign(target, ...sources)

### Object.create()

> Object.create() 方法创建一个拥有指定原型和若干个指定属性的对象

* 下面的例子演示了如何使用Object.create()来实现类式继承。这是一个单继承
``` bash
  //Shape - superclass
  function Shape() {
    this.x = 0;
    this.y = 0;
  }

  Shape.prototype.move = function(x, y) {
      this.x += x;
      this.y += y;
      console.info("Shape moved.");
  };

  // Rectangle - subclass
  function Rectangle() {
    Shape.call(this); //call super constructor.
  }

  Rectangle.prototype = Object.create(Shape.prototype);

  var rect = new Rectangle();

  rect instanceof Rectangle //true.
  rect instanceof Shape //true.

  rect.move(1, 1); //Outputs, "Shape moved."
```

*Syntax*
>  Object.create(proto[, propertiesObject])

### Object.defineProperties()

> Object.defineProperties() 方法在一个对象上添加或修改一个或者多个自有属性，并返回该对象

``` bash
  var obj = {};
  Object.defineProperties(obj, {
    "property1": {
      value: true,
      writable: true
    },
    "property2": {
      value: "Hello",
      writable: false
    }
    // 等等.
  });
  alert(obj.property2) //弹出"Hello"
```

*Syntax*
>  Object.defineProperties(obj, props)

### Object.defineProperty()

> Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个已经存在的属性，并返回这个对象

* 如果对象中不存在指定的属性，Object.defineProperty()就创建这个属性。当描述符中省略某些字段时，这些字段将使用它们的默认值。拥有布尔值的字段的默认值都是false。value，get和set字段的默认值为undefined。定义属性时如果没有get/set/value/writable，那它被归类为数据描述符。
``` bash
  var o = {}; // 创建一个新对象

  // Example of an object property added with defineProperty with a data property descriptor
  Object.defineProperty(o, "a", {value : 37,
                               writable : true,
                               enumerable : true,
                               configurable : true});
  // 对象o拥有了属性a，值为37

  // Example of an object property added with defineProperty with an accessor property descriptor
  var bValue;
  Object.defineProperty(o, "b", {get : function(){ return bValue; },
                               set : function(newValue){ bValue = newValue; },
                               enumerable : true,
                               configurable : true});
  o.b = 38;
  // 对象o拥有了属性b，值为38

  // The value of o.b is now always identical to bValue, unless o.b is redefined

  // 数据描述符和存取描述符不能混合使用
  Object.defineProperty(o, "conflict", { value: 0x9f91102,
                                       get: function() { return 0xdeadbeef; } });
  // throws a TypeError: value appears only in data descriptors, get appears only in accessor descriptors

```

*Syntax*
>  Object.defineProperty(obj, prop, descriptor)

### Object.freeze()

> Object.freeze() 方法可以冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象

* 冻结对象的所有自身属性都不可能以任何方式被修改。任何尝试修改该对象的操作都会失败，可能是静默失败，也可能会抛出异常（严格模式中）
* 数据属性的值不可更改，访问器属性（有getter和setter）也同样（但由于是函数调用，给人的错觉是还是可以修改这个属性）。如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象
``` bash
  var obj = {
    prop: function (){},
    foo: "bar"
  };

  // 可以添加新的属性,已有的属性可以被修改或删除
  obj.foo = "baz";
  obj.lumpy = "woof";
  delete obj.prop;

  var o = Object.freeze(obj);

  assert(Object.isFrozen(obj) === true);

  // 现在任何修改操作都会失败
  obj.foo = "quux"; // 静默失败
  obj.quaxxor = "the friendly duck"; // 静默失败,并没有成功添加上新的属性

  // ...在严格模式中会抛出TypeError异常
  function fail(){
    "use strict";
    obj.foo = "sparky"; // 抛出TypeError异常
    delete obj.quaxxor; // 抛出TypeError异常
    obj.sparky = "arf"; // 抛出TypeError异常
  }

  fail();

  // 使用Object.defineProperty方法同样会抛出TypeError异常
  Object.defineProperty(obj, "ohai", { value: 17 }); // 抛出TypeError异常
  Object.defineProperty(obj, "foo", { value: "eit" }); // 抛出TypeError异常
```

*Syntax*
>  Object.freeze(obj)

### Object.getOwnPropertyDescriptor()

> Object.getOwnPropertyDescriptor() 返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

``` bash
  var o, d;

  o = { get foo() { return 17; } };
  d = Object.getOwnPropertyDescriptor(o, "foo");
  // d is { configurable: true, enumerable: true, get: /*访问器函数*/, set: undefined }

  o = { bar: 42 };
  d = Object.getOwnPropertyDescriptor(o, "bar");
  // d is { configurable: true, enumerable: true, value: 42, writable: true }

  o = {};
  Object.defineProperty(o, "baz", { value: 8675309, writable: false, enumerable: false });
  d = Object.getOwnPropertyDescriptor(o, "baz");
  // d is { value: 8675309, writable: false, enumerable: false, configurable: false }
```

*Syntax*
>  Object.getOwnPropertyDescriptor(obj, prop)

### Object.getOwnPropertyDescriptors()

> Object.getOwnPropertyDescriptors() 方法用来获取一个对象的所有自身属性的描述符

* 浅拷贝一个对象

Object.assign() 方法只能拷贝源对象的可枚举的自身属性，同时拷贝时无法拷贝属性的特性们，而且访问器属性会被转换成数据属性，也无法拷贝源对象的原型，该方法配合 Object.create() 方法可以实现上面说的这些。

``` bash
  Object.create(
    Object.getPrototypeOf(obj),
    Object.getOwnPropertyDescriptors(obj)
  );
```

*Syntax*
>  Object.getOwnPropertyDescriptors(obj)

### Object.getOwnPropertyNames()

> Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性）组成的数组

``` bash
  var arr = ["a", "b", "c"];
  console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]

  // 类数组对象
  var obj = { 0: "a", 1: "b", 2: "c"};
  console.log(Object.getOwnPropertyNames(obj).sort()); // ["0", "1", "2"]

  // 使用Array.forEach输出属性名和属性值
  Object.getOwnPropertyNames(obj).forEach(function(val, idx, array) {
    console.log(val + " -> " + obj[val]);
  });
  // 输出
  // 0 -> a
  // 1 -> b
  // 2 -> c

  //不可枚举属性
  var my_obj = Object.create({}, {
    getFoo: {
      value: function() { return this.foo; },
      enumerable: false
    }
  });
  my_obj.foo = 1;

  console.log(Object.getOwnPropertyNames(my_obj).sort()); // ["foo", "getFoo"]
```

*Syntax*
>  Object.getOwnPropertyNames(obj)

### Object.getOwnPropertySymbols()

> Object.getOwnPropertySymbols() 方法会返回一个数组，该数组包含了指定对象自身的（非继承的）所有 symbol 属性键

``` bash
  var obj = {};
  var a = Symbol("a");
  var b = Symbol.for("b");

  obj[a] = "localSymbol";
  obj[b] = "globalSymbol";

  var objectSymbols = Object.getOwnPropertySymbols(obj);

  console.log(objectSymbols.length); // 2
  console.log(objectSymbols)         // [Symbol(a), Symbol(b)]
  console.log(objectSymbols[0])      // Symbol(a)
```

*Syntax*
>  Object.getOwnPropertySymbols(obj)

### Object.getPrototypeOf()

> Object.getPrototypeOf() 方法返回指定对象的原型（也就是该对象内部属性[[prototype]]的值）

``` bash
  var proto = {};
  var obj = Object.create(proto);
  Object.getPrototypeOf(obj) === proto; // true
```

> 注

* 在 ES5 中，如果参数不是一个对象类型，将抛出一个 TypeError 异常。在 ES6 中，参数被强制转换为Object。

``` bash
  > Object.getPrototypeOf("foo");
  TypeError: "foo" is not an object  // ES5 code
  > Object.getPrototypeOf("foo");
  String.prototype                   // ES6 code
```

*Syntax*
>  Object.getOwnPropertySymbols(obj)

### Object.is()

> Object.is() 方法用来判断两个值是否是同一个值

``` bash
  Object.is('foo', 'foo');     // true
  Object.is(window, window);   // true

  Object.is('foo', 'bar');     // false
  Object.is([], []);           // false

  var test = { a: 1 };
  Object.is(test, test);       // true

  Object.is(null, null);       // true

  // 特例
  Object.is(0, -0);            // false
  Object.is(-0, -0);           // true
  Object.is(NaN, 0/0);         // true
```

> Object.is() 会在下面这些情况下认为两个值是相同的：
* 两个值都是 undefined
* 两个值都是 null
* 两个值都是 true 或者都是 false
* 两个值是由相同个数的字符按照相同的顺序组成的字符串
* 两个值指向同一个对象
* 两个值都是数字并且
  * 都是正零 +0
  * 都是负零 -0
  * 都是 NaN
  * 都是除零和 NaN 外的其它同一个数字

*Syntax*
>  Object.is(value1, value2)

### Object.isExtensible()

> Object.isExtensible() 方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）

``` bash
  // 新对象默认是可扩展的.
  var empty = {};
  Object.isExtensible(empty); // === true

  // ...可以变的不可扩展.
  Object.preventExtensions(empty);
  Object.isExtensible(empty); // === false

  // 密封对象是不可扩展的.
  var sealed = Object.seal({});
  Object.isExtensible(sealed); // === false

  // 冻结对象也是不可扩展.
  var frozen = Object.freeze({});
  Object.isExtensible(frozen); // === false
```

*Syntax*
>  Object.isExtensible(obj)

### Object.isFrozen()

> Object.isFrozen() 方法判断一个对象是否被冻结（frozen）

``` bash
  // 一个对象默认是可扩展的,所以它也是非冻结的.
  assert(Object.isFrozen({}) === false);

  // 一个不可扩展的空对象同时也是一个冻结对象.
  var vacuouslyFrozen = Object.preventExtensions({});
  assert(Object.isFrozen(vacuouslyFrozen) === true);

  // 一个非空对象默认也是非冻结的.
  var oneProp = { p: 42 };
  assert(Object.isFrozen(oneProp) === false);
```

*Syntax*
>  Object.isFrozen(obj)

### Object.isSealed()

> Object.isSealed() 方法判断一个对象是否是密封的（sealed）

``` bash
  // 新建的对象默认不是密封的.
  var empty = {};
  Object.isSealed(empty); // === false

  // 如果你把一个空对象变的不可扩展,则它同时也会变成个密封对象.
  Object.preventExtensions(empty);
  Object.isSealed(empty); // === true

  // 但如果这个对象不是空对象,则它不会变成密封对象,因为密封对象的所有自身属性必须是不可配置的.
  var hasProp = { fee: "fie foe fum" };
  Object.preventExtensions(hasProp);
  Object.isSealed(hasProp); // === false

  // 如果把这个属性变的不可配置,则这个对象也就成了密封对象.
  Object.defineProperty(hasProp, "fee", { configurable: false });
  Object.isSealed(hasProp); // === true

  // 最简单的方法来生成一个密封对象,当然是使用Object.seal.
  var sealed = {};
  Object.seal(sealed);
  Object.isSealed(sealed); // === true

  // 一个密封对象同时也是不可扩展的.
  Object.isExtensible(sealed); // === false
```

*Syntax*
>  Object.isSealed(obj)

### Object.keys()

> Object.keys() 方法会返回一个由给定对象的所有可枚举自身属性的属性名组成的数组，数组中属性名的排列顺序和使用for-in循环遍历该对象时返回的顺序一致 (顺序一致不包括数字属性)（两者的主要区别是 for-in 还会遍历出一个对象从其原型链上继承到的可枚举属性）

``` bash
  var arr = ["a", "b", "c"];
  alert(Object.keys(arr)); // 弹出"0,1,2"

  // 类数组对象
  var obj = { 0 : "a", 1 : "b", 2 : "c"};
  alert(Object.keys(obj)); // 弹出"0,1,2"
```

*Syntax*
>  Object.keys(obj)

### Object.preventExtensions()

> Object.preventExtensions() 方法让一个对象变的不可扩展，也就是永远不能再添加新的属性

``` bash
  // Object.preventExtensions将原对象变的不可扩展,并且返回原对象.
  var obj = {};
  var obj2 = Object.preventExtensions(obj);
  assert(obj === obj2);

  // 字面量方式定义的对象默认是可扩展的.
  var empty = {};
  assert(Object.isExtensible(empty) === true);

  // ...但可以改变.
  Object.preventExtensions(empty);
  assert(Object.isExtensible(empty) === false);

  // 使用Object.defineProperty方法为一个不可扩展的对象添加新属性会抛出异常.
  var nonExtensible = { removable: true };
  Object.preventExtensions(nonExtensible);
  Object.defineProperty(nonExtensible, "new", { value: 8675309 }); // 抛出TypeError异常
```

*Syntax*
>  Object.preventExtensions(obj)

### Object.prototype.hasOwnProperty()

> hasOwnProperty() 方法会返回一个布尔值，其用来判断某个对象是否含有指定的属性

* 下面的例子检测了对象 o 是否含有自身属性 prop
``` bash
  o = new Object();
  o.prop = 'exists';

  function changeO() {
    o.newprop = o.prop;
    delete o.prop;
  }

  o.hasOwnProperty('prop');   // 返回 true
  changeO();
  o.hasOwnProperty('prop');   // 返回 false
```

* 自身属性与继承属性
``` bash
  o = new Object();
  o.prop = 'exists';
  o.hasOwnProperty('prop');             // 返回 true
  o.hasOwnProperty('toString');         // 返回 false
  o.hasOwnProperty('hasOwnProperty');   // 返回 false
```

*Syntax*
>  obj.hasOwnProperty(prop)

### Object.prototype.isPrototypeOf()

> isPrototypeOf() 方法用于测试一个对象是否存在于另一个对象的原型链上

* 考虑下面的原型链
``` bash
  function Fee() {
    // . . .
  }

  function Fi() {
    // . . .
  }
  Fi.prototype = new Fee();

  function Fo() {
    // . . .
  }
  Fo.prototype = new Fi();

  function Fum() {
    // . . .
  }
  Fum.prototype = new Fo();
```

* 下面创建一个 Fum 实例，检测 Fi.prototype 是否存在于该实例的原型链上
``` bash
  var fum = new Fum();

  if (Fi.prototype.isPrototypeOf(fum)) {
    // do something safe
  }
```

*Syntax*
>  prototypeObj.isPrototypeOf(object)

### Object.prototype.propertyIsEnumerable()

> propertyIsEnumerable() 方法返回一个布尔值，表明指定的属性名是否是当前对象可枚举的自身属性

* 下面的例子演示了用户自定义对象和引擎内置对象上属性可枚举性的区别
``` bash
  var a = ['is enumerable'];

  a.propertyIsEnumerable(0);          // 返回 true
  a.propertyIsEnumerable('length');   // 返回 false

  Math.propertyIsEnumerable('random');   // 返回 false
  this.propertyIsEnumerable('Math');     // 返回 false
```

*Syntax*
> obj.propertyIsEnumerable(prop)

### Object.prototype.toString()

> toString() 方法返回一个表示该对象的字符串

* 每个对象都有一个 toString() 方法，当对象被表示为文本值时或者当以期望字符串的方式引用对象时，该方法被自动调用。默认情况下，toString() 方法被每个继承自Object的对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]",其中type是对象类型。以下代码说明了这一点：
``` bash
  var o = new Object();
  o.toString();           // 返回了[object Object]
```

* 覆盖(遮蔽)默认的toString方法
``` bash
  function Dog(name,breed,color,sex) {
     this.name=name;
     this.breed=breed;
     this.color=color;
     this.sex=sex;
  }

  var theDog = new Dog("Gabby","Lab","chocolate","female");

  theDog.toString(); //returns [object Object]

  Dog.prototype.toString = function dogToString() {
    var ret = "Dog " + this.name + " is a " + this.sex + " " + this.color + " " + this.breed;
    return ret;
  }
  // "Dog Gabby is a female chocolate Lab"
```

*Syntax*
> object.toString()

### Object.prototype.valueOf()

> valueOf() 方法返回指定对象的原始值

JavaScript 调用 valueOf() 方法用来把对象转换成原始类型的值（数值、字符串和布尔值）。 你很少需要自己调用此函数; JavaScript 会自动调用此函数当需要转换成一个原始值时。

默认情况下, valueOf() 会被每个对象Object继承。每一个内置对象都会覆盖这个方法为了返回一个合理的值，如果对象没有原始值，valueOf() 就会返回对象自身。


你可以在自己的代码中使用 valueOf 方法用来把内置对象的值转换成原始值。 当你创建了自定义对象时，你可以覆盖 Object.prototype.valueOf() 并调用来取代 Object 方法。

* 覆盖自定义对象的 valueOf 方法

假设你有个对象叫 myNumberType 而你想为它创建一个 valueOf 方法。 下面的代码为valueOf方法赋予了一个用户自定义函数：

``` bash
  myNumberType.prototype.valueOf = function() { return customPrimitiveValue; };
```

*Syntax*
> object.valueOf()

### Object.seal()

> Object.seal() 方法可以让一个对象密封，并返回被密封后的对象。密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象

``` bash
  var obj = {
      prop: function () {},
      foo: "bar"
    };

  // 可以添加新的属性,已有属性的值可以修改,可以删除
  obj.foo = "baz";
  obj.lumpy = "woof";
  delete obj.prop;

  var o = Object.seal(obj);

  assert(o === obj);
  assert(Object.isSealed(obj) === true);

  // 仍然可以修改密封对象上的属性的值.
  obj.foo = "quux";

  // 但你不能把一个数据属性重定义成访问器属性.
  Object.defineProperty(obj, "foo", { get: function() { return "g"; } }); // 抛出TypeError异常
```

*Syntax*
> Object.seal(obj)

### Object.setPrototypeOf() ES6规范

> 将一个指定的对象的原型设置为另一个对象或者null(既对象的[[Prototype]]内部属性)

``` bash
  let proto = {};
  let obj = { x: 10 };
  Object.setPrototypeOf(obj, proto);
  proto.y = 20;
  proto.z = 40;
  obj.x // 10
  obj.y // 20
  obj.z // 40
```

*Syntax*
> Object.setPrototypeOf(obj, prototype)
