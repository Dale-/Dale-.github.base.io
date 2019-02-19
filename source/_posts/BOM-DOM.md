---
title: BOM And DOM
layout: post
date: 2016-12-21 10:43:18
comments: true
tags: [Event]
categories: [Event]
keywords: Event BOM DOM
description: Event BOM & DOM
---

## DOM (Document Object Model) 文档对象模型

一套HTML和XML提供的API（应用程序编程接口，DOM描述了一个层次化的节点，允许添加、移除、修改页面中的某个部分。

### 节点层次

DOM可以将任意HTML或者XML文档描绘成一个由多层节点构成的结构。
![](/images/bom-dom/document.png)
<!-- more -->

### 节点类型
> * Document : 整个文档树的顶层节点
> * DocumentType : doctype标签（比如<!DOCTYPE html>）
> * Element : 网页的各种HTML标签
> * Attribute : 网页元素的属性（比如class = "right"）
> * Text : 标签之间或标签包含的文本
> * Comment: 注释
> * DocumentFragment : 文档的片段

### 获得元素的子节点
```bash
var box = document.getElementById('box');
// 用childNodes 获取到的是当前一层的所有子节点，包括文本和注释节点
var childs = box.childNodes;
console.log(childs.length); // 9
```

> 每个节点有一个childNodes属性，其中保存着一个NodeList对象,NodeList是一种类数组对象，
  用于保存一组有序的节点，可以通过为止来访问这些节点。请注意，虽然可以通过方括号[]的
  语法来访问NodeList的值，而且这个对象也有length属性，但它并不是Array的实例。
  NodeList对象的独特之处在于，它实际上是基于DOM结构动态执行查询的结果，
  因此DOM结构的变化能够自动反应在NodeList对象中。我们常说，NodeList是由生命，
  有呼吸的的对象，而不是我们第一次访问它们的某个瞬间拍摄下来的一张快照。

> 每个节点都有nodeName和nodeValue属性，
  分别用来获取节点的名字和节点的值，对于文档节点和元素节点，
  获取到的nodeValue永远都是null，对于注释节点文本节点获取到的就是它们本身。


![](/images/bom-dom/node.jpg)

* 1、根据id来获取元素节点 （Object类型的）
在document中找Id为box的元素节点
```bash
var oDiv = document.getElementById("box");
alert(oDiv);
```
* 2.根据class来获取所有的指定class值的元素节点
```bash
var oUls = document.getElementsByClassName("list");
alert(oUls);
//切记！根据className获取到是集合！不是元素节点。要遍历集合才可以拿到集合中元素节点。
for (var i = 0; i < oUls.length; i++) {
    console.log( oUls[i] );//记得加上[i],即中括号里面加上下标，[下标]
}
```
* 3.根据标签名来获取所有该类型的元素节点。
```bash
var oLis = document.getElementsByTagName("li");
alert( oLis );
for (var i = 0; i < oLis.length; i++) {
    console.log( oLis[i] );
}

// 在第一个ul的子标签中查找所有tagName为li的元素节点
var oLi1s = oUls[0].getElementsByTagName("li")
for (var i = 0; i < oLi1s.length; i++) {
    console.log( oLi1s[i] );
}
```
* 4.根据name属性值来获取所有元素节点（表单元素中有）
```bash
var oInputs = document.getElementsByName("fm");
alert( oInputs );
for (var i = 0; i < oInputs.length; i++) {
    console.log( oInputs[i] );
}

var oImgs = document.getElementsByTagName("img");
console.log( oImgs.length );//集合中只有一个元素。但是要想获取这一个元素的时候还是要通过下标法获取 通过遍历的方式，然后下标法获取
console.log( oImgs[0] );
```

### 获取元素节点属性的属性值

* 方法一

1.获取属性的属性值 元素节点.属性名
```bash
console.log( $("inp").placeholder );
```
2.设置属性的属性值 元素节点.属性名 = 新属性值
```bash
$("inp").placeholder = "哈哈哈哈哈哈啊";
```

* 方法二

1.获取元素节点属性的属性值 元素节点.getAttribute("属性名")
```bash
console.log( $("inp").getAttribute("placeholder") );
```
2.设置元素节点属性的属性值 元素节点.setAttribute("属性名","新属性值");
```bash
$("inp").setAttribute("value","王小二");
// 特殊之处：可以获取设置自定义的属性的属性值
console.log( $("inp").my );
console.log( $("inp").getAttribute("my") );
console.log( $("inp").value );
```
3.移除属性 元素节点.removeAttribute("属性名")
```bash
$("inp").removeAttribute("value");
```

* 元素节点常用的属性
```bash
// 注意：！获取class的属性值 需要根据className来获取
console.log( oDiv.className );
//更改class属性的属性值
oDiv.className = "mydiv";
console.log(oDiv.className);
// innerHTML指的是该元素节点开始标签与结束标签之间的HTML代码。
console.log( oDiv.innerHTML );
// innerText指的是该元素开始标签与结束标签之间的文本。
console.log( oDiv.innerText );
// outerHTML指的是开始标签与结束标签再加上innerHTML
console.log( oDiv.outerHTML );
//直接给innerHTML赋值（可以是一串html代码），既然外面有双引号，里面就要用单引号。
oDiv.innerHTML = "<input type='checkbox'/>男<input type='checkbox'/>女<input type='checkbox'/>未知";
```

* 获取样式表中的属性

1. 语法：元素节点.style.属性名
```bash
console.log( $("box").style.width );
console.log( $("box").style );
```

2.设置行间样式表的属性的属性值
语法：元素节点.style.属性名 = 属性值。
类似与background-color --> backgroundColor
font-size-->fontSize
```bash
$("box").style.backgroundColor = "green";
$("box").style.color = "red";
$("box").style.fontSize = "40px";//必须带单位

```

## BOM (Browser Object Model) 浏览器对象模型
> BOM的核心对象是window，它表示浏览器的一个实例, 同时window也是JavaScript的全局对象。
  window对象处于JavaScript结构的最顶层，对于每个打开的窗口，系统都会自动为其定义window对象。
  BOM主要负责来对浏览器窗口进行操作和窗口之间的交互。

![](/images/bom-dom/bom.png)

### 主要包括
* navigator 导航器对象
* history 历史对象
* screen 显示器对象
* location 对象
* 对话框
* 定时器
* Document

![](/images/bom-dom/bom-window.png)

## BOM & DOM 总结

### BOM和DOM结构关系示意图
![](/images/bom-dom/constructor.jpeg)

### BOM与DOM区别
|     | 标准 | 核心对象 | 相关内容 |
| :---| :---| :------ | :------ |
| BOM | 无 | window | 浏览器 |
| DOM | 有，W3C | document | 网页（HTML文档）|

## 事件的传播

### 事件冒泡
当一个DOM元素上的事件被触发的时候（如：按钮点击事件），同样的事件将会在那个元素的所有父元素中被触发，从这个事件会从原始元素开始一直传递到DOM树的上一层，这一过程被称为`事件冒泡`。

### 事件捕获
浏览器获取事件（如：按钮点击事件）是从DOM的最顶层开始的，事件发生后会从DOM的根开始向下传递，直到目标元素，目标元素的所有父亲元素都会被依次遍历，浏览器获取事件的过程被称为`事件捕获`。

> 事件捕获发生在事件冒泡之前。 事件捕获是从上级元素到下级元素，即：从最不精确的对象（document）开始触发，然后到最精确的目标元素的顺序触发。事件冒泡是从下级元素到上级元素，即：从最特定的事件目标到最不特定的事件目标（document）的顺序触发。

### 事件处理程序

* HTML事件处理程序

```bash
  <div>
    <input onclick="alert('Be clicked')">
  </div>
```

* DOM0级事件处理程序

```bash
  <div>
    <input id="button">
  </div>
  <script>
    button = document.getElementById('button');
    button.onClick = function() {
      alert('DOM0 Trigger');
    }
  </script>
```

* DOM2级事件处理程序
定义了2个方法，处理指定和删除事件处理程序的操作
addEventListener()和removeEventListener()
接收参数：要处理的事件名、作为事件处理程序的函数和布尔值（true:时间捕获，false: 事件冒泡）

> 通过addEventListener事件添加的方法，只能通过removeEventListener删除（置为null无法删除掉）

### 传播的三个阶段

当一个事件发生以后，它会在不同的DOM节点之间传播(propagation)。这种传播分为三个阶段：

* 第一阶段:从window对象传导到目标节点，称为"捕获阶段"(capture phase)

* 第二阶段：在目标节点上触发，称为"目标阶段"(target phase)

* 第三阶段：从目标节点传导回window对象，称为"冒泡阶段"(bubbling phase)

```bash

<div>
  <p>Click Me</p>
</div>

<script>

  var phases = {
    1: 'capture',
    2: 'target',
    3: 'bubble'
  }

  var div = document.querySelector('div');
  var p = document.querySelector('p');

  div.addEventListener('click', callback, true);
  p.addEventListener('click', callback, true);
  div.addEventListener('click', callback, false);
  p.addEventListener('click', callback, false);

  function callback(event) {
    var tag = event.currentTarget.tagName;
    var phase = phases[event.eventPhase];
    console.log("Tag: '" + tag + "'. EventPhase: '" + phase + "'");
  }

  // 点击以后的结果
  // Tag: 'DIV'. EventPhase: 'capture'
  // Tag: 'P'. EventPhase: 'target'
  // Tag: 'P'. EventPhase: 'target'
  // Tag: 'DIV'. EventPhase: 'bubble'

</script>
```

1. 捕获阶段：事件从`<div>`向`<p>`传播时，触发`<div>`的click事件
2. 目标阶段：事件从`<div>`到达`<p>`时，触发`<p>`的click事件
3. 目标阶段：事件离开`<p>`时，触发`<p>`的click事件
4. 冒泡阶段：事件从`<p>`传回`<div>`时，再次触发`<div>`的click事件

## 事件代理
> 当我们需要对很多元素添加事件的时候，可以通过将事件添加到它们的父节点而将事件委托给父节点来触发处理函数，这主要得益于浏览器的事件冒泡机制。

```bash
  <ul id="parent">
    <li id="child-one">Item One</li>
    <li id="child-two">Item Two</li>
    <li id="child-three">Item Three</li>
    <li id="child-four">Item Four</li>
    <li id="child-five">Item Five</li>
  </ul>
```

当我们鼠标移到`<li>`上的时候，需要获取此`<li>`的相关信息并飘出悬浮窗以显示详细信息，或者当某个`<li>`被点击的时候需要触发相应的处理事件。我们通常是为每一个`<li>`都添加一些类似onMouseOver或者onClick之类的监听事件。

![](/images/bom-dom/event-delegation-1.png)

如果这个`<ul>`中的`<li>`子元素会频繁地添加或者删除，我们就需要在每次添加`<li>`的时候都调用这个`addListenersForLi`方法来为每个`<li>`节点添加事件处理函数。这就添加了复杂度和出错的可能性。
更简单的方法是使用事件代理机制，当事件被抛到更上层的父节点的时候，我们通过检查事件的目标对象(`target`)来判断并获取事件源`<li>`。下面的代码可以完成我们想要的效果：

![](/images/bom-dom/event-delegation-2.png)

为父节点添加一个`click`事件，当子节点被点击的时候，`click`事件会从子节点开始向上冒泡。父节点捕获到时间之后，通过判断`e.target.nodeName`来判断是否为我们需要处理的节点。并且通过`e.target`拿到了被点击的`<li>`节点。从而可以获取到响应的信息，并做处理。

## 事件循环
### JS的异步运行机制
1. 所有任务都在主线程上执行，形成一个执行栈(execution context stack)。
2. 主线程之外，还存在一个"任务队列"(task queue)。系统把异步任务放到"任务队列"之中，然后继续执行后续的任务。
3. 一旦"执行栈"中的所有任务执行完毕，系统就会读取"任务队列"。如果这个时候，异步任务已经结束了等待状态，就会从"任务队列"进入执行栈，恢复执行。
4. 主线程不断重复上面的第三步。

简单的说，就是在程序中（不一定是浏览器）中跑两个线程，一个负责程序本身的运行，作为主线程； 另一个负责主线程与其他线程的的通信，被称为“Event Loop 线程" 。  每当遇到异步的 setTimeOut ，setInterval 这些异步任务，交给 EventLoop 线程，然后自己往后运行，等到主线程运行完后，再去 Event Loop 线程拿结果。

![](/images/bom-dom/event-loop.png)
