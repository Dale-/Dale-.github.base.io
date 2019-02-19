---
title: Angular Directive
layout: post
date: 2017-02-7 23:20:21
comments: true
tags: [AngularJS]
categories: [AngularJS]
keywords: Angular Directive
description: Angular指令
update: 2017-02-8 10:27:21
---

[指令](https://docs.angularjs.org/guide/directive)就是一些附加在HTML元素上的自定义标记（例如：属性，元素，或css类），它告诉AngularJS的HTML编译器 ($compile) 在元素上附加某些指定的行为，甚至操作DOM、改变DOM元素，以及它的各级子节点。

Angular内置了一整套指令，如ngBind, ngModel, 和ngView。 就像你可以创建控制器和服务那样，你也可以创建自己的指令来让Angular使用。 当Angular 启动器引导你的应用程序时， HTML编译器就会遍历整个DOM，以匹配DOM元素里的指令。

<!-- more -->

## Matching Directives

### Restrict匹配方式
* E - 匹配元素名称
``` bash
<my-directive></my-directive>
```
* A - 匹配属性名
``` bash
<div my-directive="exp"></div>
```
* C - 匹配类名
``` bash
<div class="my-directive:exp;"></div>
```
* M - 注释
``` bash
<!-- directive:hello -->
<div></div>
```

<p data-height="265" data-theme-id="dark" data-slug-hash="ammBgY" data-default-tab="html" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/ammBgY/">[Angular]  - Matching Directives</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### Attention

当需要创建带有自己的模板的指令时，使用元素名称的方式创建指令

当需要为已有的HTML标签增加功能时，使用属性的方式创建指令

## Transclude

* transclude使得这个选项的指令，所包含的内容能够访问指令外部的作用域
* 指令内部嵌套的内容，放到具有ng-transclude的标签中

<p data-height="265" data-theme-id="dark" data-slug-hash="xEEOBN" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/xEEOBN/">[Angular]  - Transclude</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Replace

<p data-height="265" data-theme-id="dark" data-slug-hash="mAAPxa" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/mAAPxa/">[Angular]  - Replace</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Template

当你有大量代表客户信息的模板。这个模板在你的代码中重复了很多次，当你改变一个地方的时候， 你不得不在其他地方同时改动，这时候，你就要使用指令来简化你的模板。

### template

<p data-height="265" data-theme-id="dark" data-slug-hash="NRRjBW" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/NRRjBW/">[Angular]  - Directive TemplateCache</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### templateUrl

![](/images/angular-directive/directive-templateUrl.png)

### templateCache
* [ng](https://docs.angularjs.org/api/ng)模块中的服务

在模板第一次被引用时,它会被载入到模板缓存中以便快速的检索。你可以直接使用一个script标签来添加一个模板，或者直接通过 $templateCache 服务来达到相同的效果。

【Set】通过 script 标签添加:
``` bash
<script type="text/ng-template" id="templateId.html">
<p>This is the content of the template</p>
</script>
```

【Set】通过 $templateCache 服务添加:
``` bash
var myApp = angular.module('myApp', []);
myApp.run(function($templateCache) {
$templateCache.put('templateId.html', 'This is the content of the template');
});
```

【Get】通过 component 得到:
``` bash
myApp.component('myComponent', {
template: 'templateId.html'
});
```

【Get】通过 Javascript 得到:
    ``` bash
$templateCache.get('templateId.html')
```

<p data-height="265" data-theme-id="dark" data-slug-hash="NRRjBW" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/NRRjBW/">[Angular]  - Directive TemplateCache</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Scope

### 隔离作用域

<p data-height="265" data-theme-id="0" data-slug-hash="mAAXZL" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/mAAXZL/">[Angular]  - Directive Isolate Scope</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>


### scope的绑定策略

* @ : 字符串

``` bash
把当前属性作为字符串传递
你还可以绑定来自外层scope的值
在属性值中插入{{}}
```

 <p data-height="265" data-theme-id="dark" data-slug-hash="EgNrXo" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/EgNrXo/">[Angular]  - Directive : Scope@</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
 <script async src="//assets.codepen.io/assets/embed/ei.js"></script>

* = : 双向绑定

``` bash
与父scope中的属性进行双向绑定
```

 <p data-height="265" data-theme-id="dark" data-slug-hash="dpOAdw" data-default-tab="html,result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/dpOAdw/">[Angular]  - Directive : Scope=</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
 <script async src="//assets.codepen.io/assets/embed/ei.js"></script>

 * &lt; : 单向绑定

 ``` bash
 与父scope中的属性进行单向绑定
 ```

 <p data-height="265" data-theme-id="0" data-slug-hash="xgddra" data-default-tab="html,result" data-user="DaleDu" data-embed-version="2" data-pen-title="[Angular]  - Directive : Scope>" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/xgddra/">[Angular]  - Directive : Scope></a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
 <script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

 * ? : 可选择的属性

``` bash
如果绑定了一个没有被赋值的表达式 或者属性不存在，这时将会抛出一个异常
为了避免这种现象，我们可以使用=？来设置属性为可选择
```
<p data-height="265" data-theme-id="0" data-slug-hash="QGzVpL" data-default-tab="html,result" data-user="DaleDu" data-embed-version="2" data-pen-title="[Angular]  - Directive : Scope=?" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/QGzVpL/">[Angular]  - Directive : Scope=?</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

* & : 函数

``` bash
传递一个来自父scope的函数，稍后调用
```

 <p data-height="265" data-theme-id="dark" data-slug-hash="bwBJVq" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/bwBJVq/">[Angular]  - Directive : Scope&</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## require

### What is require

>  谈require选项之前，应该先说说controller选项，controller选项允许指令对其他指令提供一个类似接口的功能，只要别的指令（甚至是自己）有需要，就可以获取该controller，将其作为一个对象，并取得其中的所有内容。而require就是连接两个指令的锁链，它可以选择性地获取指令中已经定义好的controller，并作为link函数的第四个参数传递进去，link函数的四个参数分别为scope，element，attr和someCtrl，最后一个就是通过require获取的controller的名字，对于controller的名字，可以在指令中用controllerAs选项进行定义，这是发布控制器的关键.

### require前缀

* 无修饰: 如果不进行修饰，比如require:'thisDirective'，那么require只会在当前指令中查找控制器

* ^: 如果想要指向上游的指令，那么就是用^进行修饰，比如require:'^parentDirective'，如果没有找到，那就会抛出一个错误

* ?: 如果使用？前缀，就意味着如果在当前指令没有找到控制器，就将null作为link的第四个参数

* ?^: 如果将？和^结合起来，我们就可以既指定上游指令，又可以在找不到时，不抛出严重的错误

## Interaction: Controller & Directive
<p data-height="265" data-theme-id="dark" data-slug-hash="RGGOEk" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/RGGOEk/">[Angular]  - Interaction: Controller & Directive</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Interaction: Directive & Directive
<p data-height="265" data-theme-id="dark" data-slug-hash="jrVERX" data-default-tab="result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/jrVERX/">[Angular]  - Interaction: Directive & Directive</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Execution Stage

### 加载阶段
> * 加载angular.js，找到ng-app指令，确定应用边界

### 编译阶段
> *  遍历DOM，找到所有指令
> *  根据指令代码中的template、replace、transclude转换DOM结构
> *  如果存在compile函数则调用

### 链接阶段
> *  对每条指令运行link函数
> *  link函数一般用来操作DOM、绑定事件监听器

## Directive Demo

### Expander

<p data-height="265" data-theme-id="dark" data-slug-hash="xERZaK" data-default-tab="js,result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/xERZaK/">[Angular]  - Directive Demo : Expander</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### Accordion

<p data-height="265" data-theme-id="dark" data-slug-hash="wzgKVW" data-default-tab="js,result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/wzgKVW/">[Angular]  - Directive Demo : Accordion</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### Use Angular-ui

<p data-height="265" data-theme-id="dark" data-slug-hash="gwgPrA" data-default-tab="js,result" data-user="DaleDu" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DaleDu/pen/gwgPrA/">[Angular]  - Directive Demo : Use Angular UI</a> by Dale Du (<a href="http://codepen.io/DaleDu">@DaleDu</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

参考文档

[Directive AngularJS官网](docs.angularjs.org/guide/directive)

[Directive AngularJS中文网](http://docs.ngnice.com/guide/directive)
