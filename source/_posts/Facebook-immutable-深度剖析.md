---
title: Facebook-immutable-深度剖析
date: 2017-12-28 11:52:40
tags: learning
---
- [x] 理解immutable可先理解 Virtual DOM，参考下面文章：
https://github.com/livoras/blog/issues/13
### 什么是Immutable Data
Immutable Data是指一旦被创造后，就不可以被改变的数据。

通过使用Immutable Data，可以让我们更容易的去处理缓存、回退、数据变化检测等问题，简化我们的开发。
### js中的Immutable Data
在javascript中我们可以通过deep clone来模拟Immutable Data，就是每次对数据进行操作，新对数据进行deep clone出一个新数据。

deep clone
```
/**
 * learning-immutable - clone-deep.js
 * Created by mds on 15/6/6.
 */

'use strict';  
var cloneDeep = require('lodash.clonedeep');

var data = {  
    id: 'data',
    author: {
        name: 'mdemo',
        github: 'https://github.com/demohi'
    }
};

var data1 = cloneDeep(data);

console.log('equal:', data1===data); //false

data1.id = 'data1';  
data1.author.name = 'demohi';

console.log(data.id);// data  
console.log(data1.id);// data1

console.log(data.author.name);//mdemo  
console.log(data1.author.name);//demohi  
```

当然你或许意识到了，这样非常的慢。如下图，确实很慢

### 主角immutable.js登场

immutable.js是由facebook开源的一个项目，主要是为了解决javascript Immutable Data的问题，通过参考hash maps tries 和 vector tries提供了一种更有效的方式。

简单的来讲，immutable.js通过structural sharing来解决的性能问题。我们先看一段视频，看看immutable.js是如何做的 

当我们发生一个set操作的时候，immutable.js会只clone它的父级别以上的部分，其他保持不变，这样大家可以共享同样的部分，可以大大提高性能。

### 为什么你要在React.js中使用Immutable Data
熟悉React.js的都应该知道，React.js是一个UI = f(states)的框架，为了解决更新的问题，React.js使用了virtual dom，virtual dom通过diff修改dom，来实现高效的dom更新。

听起来很完美吧，但是有一个问题。当state更新时，如果数据没变，你也会去做virtual dom的diff，这就产生了浪费。这种情况其实很常见，[可以参考flummox这篇文章](http://acdlite.github.io/flummox/docs/guides/why-flux-component-is-better-than-flux-mixin/)

当然你可能会说，你可以使用PureRenderMixin来解决呀，[PureRenderMixin](https://reactjs.org/docs/pure-render-mixin.html)是个好东西，我们可以用它来解决一部分的上述问题，但是如果你留心的话，你可以在文档中看到下面这段提示。


- This only shallowly compares the objects. If these contain complex data structures, it may produce false-negatives for deeper differences. Only mix into components which have simple props and state, or use forceUpdate() when you know deep data structures have changed. Or, consider using immutable objects to facilitate fast comparisons of nested data.

PureRenderMixin只是简单的浅比较，不使用于多层比较。那怎么办？？自己去做复杂比较的话，性能又会非常差。

方案就是使用immutable.js可以解决这个问题。因为每一次state更新只要有数据改变，那么PureRenderMixin可以立刻判断出数据改变，可以大大提升性能。这部分还可以参考官方文档Immutability Helpers

总结就是：使用PureRenderMixin + immutable.js

引用自：http://boke.io/immutable-js/
 API：http://www.baizn.cn/2016/09/23/Immutable-js%E4%B8%AD%E6%96%87API%E6%96%87%E6%A1%A3-%E6%8C%81%E7%BB%AD%E6%9B%B4%E6%96%B0/
 
 immutable API：https://yq.aliyun.com/articles/69516  👍
