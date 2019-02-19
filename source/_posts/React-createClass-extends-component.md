---
title: '[译]React: createClass & extends Component'
date: 2017-05-22 18:05:49
tags: [React]
categories: [React]
keywords: react
description: React.createClass & extends React.Component
---

原文地址：[React.createClass versus extends React.Component](https://toddmotto.com/react-create-class-versus-component/#syntax-differences)

> 这其实是殊途同归的两种方式。过去我们使用React提供`React.createClass`的方式，
而后React提供了小小的语法糖可以允许我们更好基于ES6语法实现此功能:`React.Component`,
我们可以使用`extends React.Component`替代`createClass`。


> 虽然这两种方式的差别很小，但是有一些值得我们探索的，很有趣的知识，这允许我们在不同的场景中恰当的选择最适合的方式。

## Syntax

### React.createClass

这里我们把这个React class赋给一个const变量，用render方法来完成一个component最基本的定义。

```bash
  import React from 'react';

  const Contacts = React.createClass({
    render() {
      return (
        <div></div>
      );
    }
  });

  export default Contacts;
```

<!-- more -->

### React.Component

让我们来尝试一下`React.createClass`定义，使用ES6来完成这个component的定义。

```bash
  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
    }

    render() {
      return (
        <div></div>
      );
    }
  }

  export default Contacts;
```

从JavaScript的前景来说我们已经开始使用ES6 class了，使用像Babel这种的编译方式将ES6转换为ES5后供浏览器使用。针对这种变化，我们推荐`constructor`,我们需要调用`super()`方法将props传递给`React.Component`。

我们现在创建了一个名为Contacts的`class`，使用从`React.Component`继承的方式替代了直接得到一个`React.createClass`,这种方式更少的引用以及更具有`JavaScript`的偏好。在这个语法转变的过程中，是具有革命性的。

## propTypes and getDefaultProps

怎样使用和声明默认属性、它们的类型以及设置初始状态，是这两种方式最大的差别。

### React.createClass

在`React.createClass`中，`propTypes`属性是一个对象，我们可以为其中的每一个`prop`声明。`getDefaultProps`函数返回一个对象，这个对象的所有属性将会作为组件的初始化属性。

```bash
  import React from 'react';

  const Contacts = React.createClass({

    propTypes: {

    },

    getDefaultProps() {
      return {

      };
    },

    render() {
      return (
        <div></div>
      );
    }
  });

  export default Contacts;
```

### React.Component

我们将`propTypes`作为`Contacts`类的一个属性而不是在`createClass`的内部定义这个对象。我认为这比`createClass`更加友好和清晰。

现在`getDefaultProps`转变为类的一个对象属性:`defaultProps`,不在使用`get`,只是一个对象。我喜欢这种语法，因为他避免了React的语法，而变成了原生JavaScript。

```bash
  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
    }

    render() {
      return (
        <div></div>
      );
    }

  }

  Contacts.propTypes = {

  };

  Contacts.defaultProps = {

  };

  export default Contacts;
```

## State

  State的变化很有趣，现在我们使用构造函数来实现状态的初始化。

### React.createClass

  `getInitialState`只做一件事，就是返回一个包含初始化状态的对象。

```bash
  import React from 'react';

  const Contacts = React.createClass({

    getInitialState() {
      return {

      };
    },

    render() {
      return (
        <div></div>
      );
    }
  })

  export default Contacts;
```

### React.Component

  在`React.Component`中，`getInitialState`方法被抛弃了，现在我们在`constructor`中像初始化属性一样声明所有的状态，我想这种方式更加像`JavaScript`并且更少的驱动了"API"。

```bash
  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
      this.state = {

      };
    }

    render() {
      return (
        <div></div>
      );
    }
  }

  export default Contacts;
```

## "this"

  使用`React.createClass`将会自动绑定`this`的值，但是在ES6中`this`将会失效。

### React.createClass

  `onClick`声明与`this.handleClick`进行绑定。当这个方法被触发时，React将会根据正确的上下文去执行`handleClick`。

```bash
  import React from 'react';

  const Contacts = React.createClass({

    handleClick() {
      console.log(this);  // React Component instance
    },

    render() {
      return (
        <div onClick={this.handleClick}></div>
      )
    }
  })

  export default Contacts;
```

### React.Component

  在ES6类中有轻微的不同，属性不会自动绑定到React类的实例上。

```bash
  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
    }

    handleClick() {
      console.log(this); //null
    }

    render() {
      return (
        <div onClick={this.handleClick}></div>
      )
    }
  }

  export default Contacts;
```

  这里有一种方式可以帮助我们绑定正确的上下文，下面我们在行内代码中进行绑定。

```bash
  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
    }

    handleClick() {
      console.log(this); // React Component instance
    }

    render() {
      return (
        <div onClick={this.handleClick.bind(this)}></div>
      )
    }
  }

  export default Contacts;
```

    除此之外，我们还有一种在`constructor`里面绑定上下文的方法，这种方法更加优雅，即便日后改变了语法接口，也不需要去改动JSX:

```bash

  import React from 'react';

  class Contacts extends React.Component {

    constructor(props) {
      super(props);
      this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
      console.log(this); // React Component instance
    }

    render() {
      return (
        <div onClick={this.handleClick}></div>
      )
    }
  }

  export default Contacts;
```    

## Mixins

  ES6的`React.Component`不支持Mixins

### React.createClass

  `React.createClass`中我们创建组件时，添加一个叫mixins的属性，并将这个属性集合以数组的形式赋给 mixins

```bash

  import React from 'react';

  var SomeMixin = {
    doSomething() {

    }
  };

  const Contacts = React.createClass({

    mixins: [SomeMixin],

    handleClick() {
      this.doSomething(); // use mixin
    },

    render() {
      return (
        <div onClick={this.handleClick}></div>
      )
    }
  })

  export default Contacts;
```

### React.Component

  ES6类暂不支持Mixins
