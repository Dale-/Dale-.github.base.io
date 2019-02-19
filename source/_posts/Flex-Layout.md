---
title: Flex Layout
layout: post
date: 2017-09-19 21:19:32
comments: true
tags: [CSS]
categories: [CSS]
keywords: CSS
description: flex layout
---

## Flex

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

The Flexbox Layout (Flexible Box) module (currently a W3C Last Call Working Draft) aims at providing a more efficient way to lay out, align and distribute space among items in a container, even when their size is unknown and/or dynamic (thus the word "flex").

The main idea behind the flex layout is to give the container the ability to alter its items' width/height (and order) to best fill the available space (mostly to accommodate to all kind of display devices and screen sizes). A flex container expands items to fill available free space, or shrinks them to prevent overflow.

Most importantly, the flexbox layout is direction-agnostic as opposed to the regular layouts (block which is vertically-based and inline which is horizontally-based). While those work well for pages, they lack flexibility (no pun intended) to support large or complex applications (especially when it comes to orientation changing, resizing, stretching, shrinking, etc.).

**Note**: Flexbox layout is most appropriate to the components of an application, and small-scale layouts, while the Grid layout is intended for larger scale layouts.

```CSS
.box{
  display: flex;
}
```

![](/images/flex/flex-container.png)

<!-- more -->  

* flex container : Flex容器
* flex item : Flex项目
* main axis : 水平的主轴
* cross axis : 垂直的交叉轴
* main start : 主轴的开始位置（与边框的交叉点）
* main end : 主轴的结束位置（与边框的交叉点）
* cross start : 交叉轴的开始位置
* cross end : 交叉轴的结束位置
* main size : 单个项目占据的主轴空间
* cross size : 占据的交叉轴空间

## 容器属性

### flex-direcion

> flex-direcion属性决定主轴的方向（项目排列的方向）

> This establishes the main-axis, thus defining the direction flex items are placed in the flex container. Flexbox is (aside from optional wrapping) a single-direction layout concept. Think of flex items as primarily laying out either in horizontal rows or vertical columns.

```CSS
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

* row（默认值）：主轴为水平方向，起点在左端。
* row-reverse：主轴为水平方向，起点在右端。
* column：主轴为垂直方向，起点在上沿。
* column-reverse：主轴为垂直方向，起点在下沿。

![](/images/flex/flex-direction.png)

***

### flex-wrap

> 默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

> By default, flex items will all try to fit onto one line. You can change that and allow the items to wrap as needed with this property.

```CSS
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

* nowrap（默认）：不换行。
* wrap：换行，第一行在上方。
* wrap-reverse：换行，第一行在下方。

#### nowrap(默认)：不换行
![](/images/flex/flex-no-wrap.png)

#### wrap：换行，第一行在上方
![](/images/flex/flex-wrap.png)

#### wrap-reverse：换行，第一行在下方
![](/images/flex/flex-wrap-reverse.png)

### flex-flow

> flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

> This is a shorthand flex-direction and flex-wrap properties, which together define the flex container's main and cross axes. Default is row nowrap.

```CSS
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```


***

### justify-content

> justify-content属性定义了项目在主轴上的对齐方式。

> This defines the alignment along the main axis. It helps distribute extra free space left over when either all the flex items on a line are inflexible, or are flexible but have reached their maximum size. It also exerts some control over the alignment of items when they overflow the line.

```CSS
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

#### flex-start（默认值）：左对齐
![](/images/flex/justify-content-flex-start.png)

#### flex-end：右对齐
![](/images/flex/justify-content-flex-end.png)

#### center： 居中
![](/images/flex/justify-content-center.png)

#### space-between：两端对齐，项目之间的间隔都相等。
![](/images/flex/justify-content-space-between.png)

#### space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
![](/images/flex/justify-content-space-around.png)

***

### align-items

> align-items属性定义项目在交叉轴上如何对齐。

> This defines the default behaviour for how flex items are laid out along the cross axis on the current line. Think of it as the justify-content version for the cross-axis (perpendicular to the main-axis).

```CSS
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

#### flex-start：交叉轴的起点对齐。
![](/images/flex/align-items-flex-start.png)

#### flex-end：交叉轴的终点对齐。
![](/images/flex/align-items-flex-end.png)

#### center：交叉轴的中点对齐。
![](/images/flex/align-items-center.png)

#### baseline: 项目的第一行文字的基线对齐。
![](/images/flex/align-items-baseline.png)

#### stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
![](/images/flex/align-items-stretch.png)

***

### align-content

> align-content属性定义了多根轴线（多行）的对齐方式。如果项目只有一根轴线，该属性不起作用。

> This aligns a flex container's lines within when there is extra space in the cross-axis, similar to how justify-content aligns individual items within the main-axis.

> Note: this property has no effect when there is only one line of flex items.

```CSS
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

#### flex-start：与交叉轴的起点对齐。
![](/images/flex/align-content-flex-start.png)

#### flex-end：与交叉轴的终点对齐。
![](/images/flex/align-content-flex-end.png)

#### center：与交叉轴的中点对齐。
![](/images/flex/align-content-center.png)

#### space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
![](/images/flex/align-content-space-between.png)

#### space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
![](/images/flex/align-content-space-between.png)

#### stretch（默认值）：轴线占满整个交叉轴。
![](/images/flex/align-content-stretch.png)

## 项目属性

### order

> order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

> By default, flex items are laid out in the source order. However, the order property controls the order in which they appear in the flex container.

```CSS
.item {
  order: <integer>;
}
```

![](/images/flex/flex-order.png)

***

### flex-grow

> flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

> This defines the ability for a flex item to grow if necessary. It accepts a unitless value that serves as a proportion. It dictates what amount of the available space inside the flex container the item should take up.

> If all items have flex-grow set to 1, the remaining space in the container will be distributed equally to all children. If one of the children has a value of 2, the remaining space would take up twice as much space as the others (or it will try to, at least).

```CSS
.item {
  flex-grow: <number>; /* default 0 */
}
```

![](/images/flex/flex-grow.png)

***

### flex-shrink

> flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

> This defines the ability for a flex item to shrink if necessary.

```CSS
.item {
  flex-shrink: <number>; /* default 1 */
}
```
![](/images/flex/flex-shrink.png)

***

### flex-basis

> flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

> This defines the default size of an element before the remaining space is distributed. It can be a length (e.g. 20%, 5rem, etc.) or a keyword. The auto keyword means "look at my width or height property" (which was temporarily done by the main-size keyword until deprecated). The content keyword means "size it based on the item's content" - this keyword isn't well supported yet, so it's hard to test and harder to know what its brethren max-content, min-content, and fit-content do.

```CSS
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

![](/images/flex/flex-basis.png)

***

### align-self

> align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

> This allows the default alignment (or the one specified by align-items) to be overridden for individual flex items.

> Please see the align-items explanation to understand the available values.

```CSS
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![](/images/flex/flex-align-self.png)

## source

[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
