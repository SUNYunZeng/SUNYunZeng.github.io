---
title: 自己实现 React-Redux (六)：去除 context 依赖，实现 provider
comments: true
toc: true
date: 2020-05-20 13:58:31
categories: 教程
tags: React
---

# Index 组件中的 context 依赖

Content 组件、Header 组件以及 ThemeSwitch 组件都是依赖于 Index 父组件的 context 属性，而 context 属性在 Index 组件中存在会导致 Index 依赖于额外的属性，扩展性不强。

因此我们需要提炼出一个淡出的组件 provider 来当作 Index 的父组件，来存储全局的 context 属性，实现 Index 组件与 context 属性的解耦。

# 构建 Provider 组件

在 `src/react-redux.js` 文件中添加如下代码：

```javascript
class Provider extends Component {

    static propTypes = {
        store: PropTypes.object,
        children: PropTypes.any
    }

    static childContextTypes = {
        store: PropTypes.object
    }
    
    getChildContext(){
        return {store: this.props.store}
    }

    render() {
        return (
            <div>
                {this.props.children}
            </div>
        )
    }
}

export {Provider}
```

同时在我们的 Index 组件中就可以利用 Provider 组件包裹住 Index 组件，并向其传入 store 值。

```javascript
// src/index.js
import React, {Component} from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import Header from './Header'
import Content from './Content'
import PropTypes from 'prop-types'
import {Provider} from './react-redux'

function createStore(reducer){
  // 初始状态
  let state = null
  // 定义发布者
  const listeners = []
  // 绑定订阅者
  const subscribe = (listener)=>listeners.push(listener)
  // 获取 state
  const getState = () => state
  // 绑定处理函数
  const dispatch = (action)=>{
      state = reducer(state, action)
      // 数据更新自动通知给订阅者
      listeners.forEach(listener=>listener())
  }
  // 初始化
  dispatch({})
  // 返回状态管理对象
  return {getState, dispatch, subscribe}
}

const themeReducer = (state, action)=>{
  if(!state){
    return {
      themeColor: 'red'
    }
  }
  switch(action.type){
    case 'CHANGE_COLOR':
      return {...state, themeColor: action.themeColor}
    default:
      return state
  }
}

const store = createStore(themeReducer);

class Index extends Component {
  render() {
    return (
      <div>
        <Header/>
        <Content/>
      </div>
    )
  }
}

ReactDOM.render(
  <Provider store={store}>
    <Index/>
  </Provider>,
  document.getElementById('root')
);
```

# 总结

+ 我们首先利用 React 的“全局变量” context 实现了子组件按钮改变全局颜色样式的效果。

+ 考虑到代码的重复以及组件的可扩展性，我们构建了一个 connect 函数实现了 context 属性及相关改变 state 方法 dispatch 的代理，并通过 props 由高阶组件传递给子组件。

+ 考虑到 context 还在父组件中使其复用性底，因此又构建了代理父组件 Provider 负责存储全局变量管理对象 store，并通过 props 传递给 Index 组件。

[项目地址](https://github.com/SUNYunZeng/imitateReact-redux)