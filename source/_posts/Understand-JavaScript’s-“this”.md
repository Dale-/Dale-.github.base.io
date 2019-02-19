---
title: '[译] Understand JavaScript’s “this”'
layout: post
date: 2018-5-28 23:07:55
comments: true
tags: [JavaScript]
categories: [JavaScript]
keywords: JavaScript
description: JavaScript scope
---

## [译] Understand JavaScript’s “this” With Clarity

原文地址：[Understand JavaScript’s “this” With Clarity, and Master It](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/#)

## JavaScript中 `this` 的基本用法

首先，我们知道在JavaScript中所有函数像对象一样，都具有属性。当一个函数被执行时，将获得 `this` 这个属性。而 `this` 实际上就是一个具有调用函数对象值的变量。

变量 `this` 通常都是指向一个对象，并拥有这个对象的值。`this` 通常被用在一个函数或者方法体内，当然它也可以被使用在函数外的全局作用域中。当我们开启严格模式时，`this` 在全局函数和匿名函数中的值为 `undefined` ，不指向任何一个对象。

`this`被在函数内使用（函数称为函数A），它包含了能够调用函数A的对象的值。我们需要 `this` 来获取调用函数A的对象的方法和属性，这在我们不知道函数名字或者函数没有名字的时候去调用对象时，显得尤为重要。实际上，`this`就是一个祖先对象，或者说是调用函数时的一个指代。

<!-- more -->

``` bash
var person = {
    firstName   :"Penelope",
    lastName    :"Barrymore",
    // Since the "this" keyword is used inside the showFullName method below, and the showFullName method is defined on the person object,​
    // "this" will have the value of the person object because the person object will invoke showFullName ()​
    showFullName:function () {
    console.log (this.firstName + " " + this.lastName);
    }
​}
​
person.showFullName (); // Penelope Barrymore
```

我们再来看看jQuery中__this__的基本用法

``` bash
   // A very common piece of jQuery code​
​
   $ ("button").click (function (event) {
   // $(this) will have the value of the button ($("button")) object​
​   // because the button object invokes the click () method​
   console.log ($ (this).prop ("name"));
   });});
```

我来解释一下上面jQuery的例子：*$(this)*是jQuery中使用*this*的语法同在JavaScript中，它被用在一个匿名函数中，这个匿名函数会执行按钮的click方法。*$(this)*有button这个对象的原因是jQuery的库中把`$(this)`和调用click方法的对象手动绑定在一起。。尽管*$(this)*将会拥有jQuery对象*$("button")*的值，但是在*$(this)*被定义的函数内，不能得到这个函数外的*$(this)*变量。

值得注意的，button是一个在HTML中的DOM元素，也是一个对象。在这个例子中button是一个jQuery对象，因为我用jQuery中的`$`包装了它。

## 理解Javascript中 `this` 的关键

如果你理解了下面关于JavaScript中this的准则，你将更清楚的理解了__"this"__这个关键字：*this*不会立刻被赋值，直到一个对象调用了*this*所在的函数。让我们称这个函数为"this Function"。

## 全局作用域中的 `this`

在全局作用域中，当code执行在浏览器端时，所有全局变量和函数都被定义在window这个对象上。因此，当我们在全局作用域中使用*this*时，它会指向的全局window对象并拥有它的值（除非在严格模式下），这是*this*就是JavaScript应用和web page的主要容器。

``` bash
var firstName = "Peter",
    lastName = "Ally";
​
function showFullName () {
// "this" inside this function will have the value of the window object​
// because the showFullName () function is defined in the global scope, just like the firstName and lastName​
console.log (this.firstName + " " + this.lastName);
}
​
var person = {
  firstName   :"Penelope",
  lastName    :"Barrymore",
  showFullName:function () {
  // "this" on the line below refers to the person object, because the showFullName function will be invoked by person object.​
  console.log (this.firstName + " " + this.lastName);
  }
}
​
showFullName (); // Peter Ally​
​
// window is the object that all global variables and functions are defined on, hence:​
window.showFullName (); // Peter Ally​
​
// "this" inside the showFullName () method that is defined inside the person object still refers to the person object, hence:​
person.showFullName (); // Penelope Barrymore
```

## `this` 最容易被误解的情景

`this` 最容易被误解的关键：当我们借用一个`this`的方法，当我们把`this`的方法赋值给一个变量，当使用了包含
`this`的方法作为回调传入，当`this`在闭包中被使用。我们将重现每个场景，并且给出相对适当的解决方案在每个案例中。

### 在我们继续重现场景前，先讨论下“上下文”这个概念
> 在JavaScript中的上线文接近英文中主语的概念"John is the winner who returned the money.""这句话的主语是John，然后我们可以说这句话的上下文就是John，因为整句话我们都聚焦在John身上。代词"Who"也是指代John。就想我们可以用分号切换一句话的主题一样，我们也可以用这个上下文切换我们调用函数的主题。

``` bash
var person = {
   firstName   :"Penelope",
   lastName    :"Barrymore",
   showFullName:function () {
// The "context"
console.log (this.firstName + " " + this.lastName);
 }
}

// The "context", when invoking showFullName, is the person object, when we invoke the showFullName () method on the person object.
// And the use of "this" inside the showFullName() method has the value of the person object,
person.showFullName (); // Penelope Barrymore

// If we invoke showFullName with a different object:
var anotherPerson = {
firstName   :"Rohit",
lastName    :"Khan"
};

// We can use the apply method to set the "this" value explicitly—more on the apply () method later.
// "this" gets the value of whichever object invokes the "this" Function, hence:
person.showFullName.apply (anotherPerson); // Rohit Khan

// So the context is now anotherPerson because anotherPerson invoked the person.showFullName ()  method by virtue of using the apply () method
```

### 1.Fix 含有`this`的函数中作为回调被使用

当我们将包含this的方法作为回调去使用时，这往往变得很难以理解。

``` bash
// We have a simple object with a clickHandler method that we want to use when a button on the page is clicked
var user = {
data:[
{name:"T. Woods", age:37},
{name:"P. Mickelson", age:43}
],
clickHandler:function (event) {
var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1

// This line is printing a random person's name and age from the data array
console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
}
}

// The button is wrapped inside a jQuery $ wrapper, so it is now a jQuery object
// And the output will be undefined because there is no data property on the button object
$ ("button").click (user.clickHandler); // Cannot read property '0' of undefined
```

上面的代码中可以知道，我们调用了$("button")这个对象的click方法，并且将user的clickHandler
方法作为回调传入click方法中，不难发现user的clickHandler方法，这时user.clickHandler方法中的
`this`已经不再指向user这个对象。因为`this`被定义在user.clickHandler中，所以`this`现在指向
那个调用user.clickHandler方法的对象。这个调用user.clickHandler的对象是button这个对象，
user.clickHandler将会在button的click方法被执行。

由于上面这个观点，当上下文改变的时候，也就是当我们在一些其他对象上执行一个方法的时候`this`关键字不在
指向原始的对象，而是指向调用的这个方法的对象。

解决`this`被作为回调这个现象的方法：
我们可以使用Bind(),Apply()或者Call()方法显示设置`this`的值，这样我们的`this.data`就会指向
user对象的data属性了。

所以，原来下面这行代码：
``` bash
    $ ("button").click (user.clickHandler);
```
我们用bind方法，将user对象和clickHandler方法绑定起来：
``` bash
	  $("button").click (user.clickHandler.bind (user)); // P. Mickelson 43
```

[View a working example of this on JSBin](http://jsbin.com/exanul/1/edit?html,js,output)

### 2.Fix `this` 在闭包中的情景

`this`在闭包中使用时另一个容易误解的例子。值得注意的是，闭包是不能得到外层函数的`this`变量通过使用`this`关键字，
因为`this`变量只能通过函数本身得到，而不是内层函数。

``` bash
var user = {
tournament:"The Masters",
data      :[
{name:"T. Woods", age:37},
{name:"P. Mickelson", age:43}
],

clickHandler:function () {
// the use of this.data here is fine, because "this" refers to the user object, and data is a property on the user object.

this.data.forEach (function (person) {
// But here inside the anonymous function (that we pass to the forEach method), "this" no longer refers to the user object.
// This inner function cannot access the outer function's "this"

console.log ("What is This referring to? " + this); //[object Window]

console.log (person.name + " is playing at " + this.tournament);
// T. Woods is playing at undefined
// P. Mickelson is playing at undefined
})
}

}

user.clickHandler(); // What is "this" referring to? [object Window]
```

`this`在一个匿名函数中时，是不能通过外层函数的`this`得到的，所以当没有使用严格模式的时候，
它被绑定在全局window对象上。

我们可以这样维持`this`在匿名函数中：

为了fix这个问题，我们需要在匿名函数的forEach中使用`this`前，使用一个通用的实践，
在我们执行forEach前，将`this`赋值给其他变量。


``` bash
var user = {
   tournament:"The Masters",
   data      :[
   {name:"T. Woods", age:37},
   {name:"P. Mickelson", age:43}
   ],

   clickHandler:function (event) {
   // To capture the value of "this" when it refers to the user object, we have to set it to another variable here:
   // We set the value of "this" to theUserObj variable, so we can use it later
   var theUserObj = this;
   this.data.forEach (function (person) {
   // Instead of using this.tournament, we now use theUserObj.tournament
   console.log (person.name + " is playing at " + theUserObj.tournament);
   })
   }

   }

   user.clickHandler();
   // T. Woods is playing at The Masters
   //  P. Mickelson is playing at The Masters
```

值得注意的，许多JavaScript的开发者喜欢用一个名字为`that`的变量接受`this`的值，像下面提到的那样。
但是与我来说`that`是一个没有意义的名字，所以我尽量将一个变量命名可以描述出`this`的指向，所以上述代码中
我使用了`var theUserObj = this`。

``` bash
// A common practice amongst JavaScript users is to use this code​
var that = this;
```

[View a working example of this on JSBin](http://jsbin.com/ibohiw/1/edit?html,js,output)

### 3.Fix `this`上的方法赋值给一个变量

当我们使用`this`变量去赋值一个方法时，`this`的值可能会超出我们想象地指向其他对象。

``` bash
// This data variable is a global variable
var data = [
{name:"Samantha", age:12},
{name:"Alexis", age:14}
];

var user = {
// this data variable is a property on the user object
data    :[
{name:"T. Woods", age:37},
{name:"P. Mickelson", age:43}
],
showData:function (event) {
var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1

// This line is adding a random person from the data array to the text field
console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
}

}

// Assign the user.showData to a variable
var showUserData = user.showData;

// When we execute the showUserData function, the values printed to the console are from the global data array, not from the data array in the user object
//
showUserData (); // Samantha 12 (from the global data array)
```

解决方案：
同1的解决方案，利用bind方法显示地设置`this`的值。

``` bash
// Bind the showData method to the user object
var showUserData = user.showData.bind (user);

// Now we get the value from the user object, because the this keyword is bound to the user object
showUserData (); // P. Mickelson 43
```

### 4.Fix 当借用方法的时候 this 的值不正确的问题

借用方法在JavaScript开发中是很常见的，作为JavaScript的开发者，我们将会经常遇到这种现象。

``` bash
// We have two objects. One of them has a method called avg () that the other doesn't have
// So we will borrow the (avg()) method
var gameController = {
	scores: [20, 34, 55, 46, 77],
	avgScore: null,
	players: [
		{name: "Tommy", playerID: 987, age: 23},
		{name: "Pau", playerID: 87, age: 33}
	]
}
var appController = {
	scores: [900, 845, 809, 950],
	avgScore: null,
	avg: function() {
		var sumOfScores = this.scores.reduce(function(prev, cur, index, array) {
			return prev + cur;
		});
		this.avgScore = sumOfScores / this.scores.length;
	}
}

//If we run the code below,
// the gameController.avgScore property will be set to the average score from the appController object "scores" array

// Don't run this code, for it is just for illustration; we want the appController.avgScore to remain null

gameController.avgScore = appController.avg();
```
avg函数中`this`这个关键字不再指向gameController这个对象，它将会指向appController对象因为它是在appController上调用的。
为了fix这个问题并且确定 appController.avg()方法内部的this指向gameController，我们使用apply()方法：

``` bash
// Note that we are using the apply () method, so the 2nd argument has to be an array—the arguments to pass to the appController.avg () method.​
appController.avg.apply (gameController, gameController.scores);
​
// The avgScore property was successfully set on the gameController object, even though we borrowed the avg () method from the appController object​
console.log (gameController.avgScore); // 46.4​
​
// appController.avgScore is still null; it was not updated, only gameController.avgScore was updated​
console.log (appController.avgScore); // null
```

gameController对象借用appController的avg方法。appController中`this`的值将会被set到gameController对象，
因为我们把gameController作为第一个参数传入了apply方法。apply方法的第一个参数，将会被显示地设置为`this`的值。

## 结语
我希望看完这篇文章后，你对JavaScript中的this关键字有足够的理解。现在你可以使用（bing, apply, and call, and setting this
to a variable）来帮助你解决各种情况下的`this`问题。

正如我们在上面看到的，`this`在很多情况下都会难以理解，比如上下文发生改变的时候，特别是在回调函数中，或者被另一个对象调用的时候，
或是当借用芳芳的时候。永远记得`this`具有调用`this`方法的对象的值。

Enjoy Coding.

[中文翻译](http://www.tuicool.com/articles/UfamIn2)

### this指向
* 全局作用域或者普通函数中`this`指向全局对象window
* 方法调用中谁调用`this`指向谁
* 在构造函数或者构造函数原型对象中`this`指向构造函数的实例
