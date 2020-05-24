---
title: 自己实现 React-Redux (一)：React.js中的 context
comments: true
toc: true
date: 2020-05-15 12:04:17
categories: 教程
tags: React
---

# Redux架构

Redux是一种前端架构，是由 Facebook 的 [Flux](https://justgetflux.com/) 框架演变而来，但避开了 Flux 的复杂性。而 [Flux](https://www.jdon.com/idea/flux.html) 是一种模式而非正式框架，通过利用单项数据流补充React的组合视图组件。

## Redux要解决的问题

React的数据流是单项数据流，如果任何一个子组件依赖了父组件的状态，就需要将状态存储到父组件中进行**状态提升**。但假设该父组件又是另一组件的子组件且状态又由另一组件决定，那么状态又需提升至另一组件。层层的状态依赖非常麻烦。

React中可以通过设置 **context**属性实现全局变量的效果，任何该组件的子组件都可以读取到 context 属性；而一旦 context 属性改变，所有依赖 context 属性的组件都会更新。

并且所有能读取到 context 的组件都可以对该属性进行修改，如果在组件关系非常复杂，到处存在这种修改父组件 context 的逻辑，那么可能产生难以预测的后果。

因此 Redux 规范了修改全局属性 **state** 的方法，便于状态改变的维护与管理。

## 核心概念

**1. 单一数据源**

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

**2. State 是只读的**

唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

**3. 使用纯函数来执行修改**

为了描述 action 如何改变 state tree ，你需要编写 reducers。

reducers是一个**纯函数**，纯函数是输出结果仅依赖于输入参数，且不会有副作用，即**函数执行过程不会产生外部可观察的变化**。

## 总结

共享的状态如果可以被任意修改的话，那么程序的行为将非常不可预料，所以需要用一种模式 Redux 来统一对数据修改进行管理。

而将 Redux 架构应用到 React 中就是 React-Redux。

# React 中的 context

前面提到，context 的属性是 react 中的全局变量，不是通过单项数据流来实现数据传递，可以**穿透**子组件进行数据传递。

由于该属性直接利用或修改非常危险，所以尽量不要直接使用该属性，就像 Javascript 里尽量避免全局变量一样。

但该属性对于后面实现 React-Redux 很有帮助，所以先简单讲解一下基本使用。

## 定义组件树

```javascript
class Parent extends Component {
  render(){
    return (
      <div>
        <Children/>,
      </div>
    )
  }
}

class Children extends Component {
  render() {
    return (
      <div>
        <p>Children的颜色</p>,
        <GrandChildren/>
      </div>
    )
  }
}

class GrandChildren extends Component {
  render() {
    return (
      <div>
        <p>GrandChildren的颜色</p>
      </div>
    )
  }
}
```

## 父组件定义 context 属性

```javascript
class Parent extends Component {

  static childContextTypes = {
    textColor: PropTypes.string
  }

  constructor(){
    super()
    this.state = {textColor: 'red'}
  }

  getChildContext(){
    return {textColor: this.state.textColor}
  }

  render(){
    return (
      <div>
        <Children/>,
        <GrandChildren/>
      </div>
    )
  }
}
```

父组件通过 **getChildContext** 方法返回对象传递 context。父组件要想 context 属性生效，必须定义 **childContextTypes** 属性，因为 context 是一个危险的特性，按照 React.js 团队的想法就是，把危险的事情搞复杂一些，提高使用门槛人们就不会去用了。如果你要给组件设置 context，那么 childContextTypes 是必写的。

## 子组件接收 context

```javascript

class GrandChildren extends Component {
  static contextTypes = {
    textColor: PropTypes.string
  }

  render() {
    return (
      <div>
        <p style={{color: this.context.textColor}}>GrandChildren的颜色</p>
      </div>
    )
  }
}
```

子组件要想读取 context，必须定义 **contextTypes** 进行 context 输入属性验证，然后可以直接使用 this.context 读取父组件属性。

{% asset_img context.png %}

# 参考资料

1. [React.js 小书](http://huziketang.mangojuice.top/books/react/)

2. [Flux架构讲解](https://www.jdon.com/idea/flux.html)

3. [Redux中文文档](https://www.redux.org.cn/)