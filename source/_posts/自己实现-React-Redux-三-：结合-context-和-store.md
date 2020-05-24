---
title: 自己实现 React-Redux (三)：结合 context 和 store
comments: true
toc: true
date: 2020-05-16 20:29:25
categories: 教程
tags: React
---

# 构建 createStore 函数实现 Redux 架构

**原理**: 构造一个函数 **createStore**，该函数可以对全局变量的状态进行统一的管理。该函数传入一个全局 **state** 以及状态修改纯函数 **reducer**, reducer 定义了改变状态的逻辑并返回一个新的状态，为后面的动态渲染优化做准备。

1. 在 src/index.js 中加入 createStore 函数

```javascript
function createStore(reducer){
    // 初始状态
    let state = null
    // 定义发布者
    const listeners = []
    // 绑定订阅者
    const subscribe = (listener)=>listeners.push(listener)
    // 获取 state
    const getState = ()=>state
    // 绑定处理函数
    const dispatch = (action)=>{
        state = reducer(state, action)
        // 数据更新自动通知给订阅者
        liSteners.forEach(listener=>listener())
    }
    // 初始化 state
    dispatch({})
    // 初始化
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
```

# 子组件调用 store 并修改 state

首先父组件 src/index.js 中定义 store 上下文：

```javascript
class Index extends Component {
  static childContextTypes = {
    store: PropTypes.object
  }

  getChildContext(){
    return {store}
  }
  
  render() {
    return (
      <div>
        <Header/>
        <Content/>
      </div>
    )
  }
}
```

然后修改 src/Header.js，让它从 Index 的 context 里面获取 store，并且获取里面的 themeColor 状态来设置自己的颜色：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class Header extends Component {

    static contextTypes = {
        store: PropTypes.object
    }

    constructor(){
        super()
        this.state = {
            themeColor: ''
        }
    }

    componentWillMount(){
        this._updateThemeColor()
    }

    _updateThemeColor(){
        const {store} = this.context
        const themeColor = store.getState().themeColor;
        this.setState({themeColor})
    }

    render() {
        return (
            <div>
                <h1 style={{color: this.state.themeColor}}>动手实现React-Redux</h1>
            </div>
        )
    }
}

export default Header;
```

然后界面的标题栏变成了红色：

{% asset_img header.png %}


如法炮制 Content.js：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import ThemeSwitch from './ThemeSwitch'

class Content extends Component {

    static contextTypes = {
        store: PropTypes.object
    }

    constructor(){
        super()
        this.state = {
            themeColor: ''
        }
    }

    componentWillMount(){
        this._updateThemeColor()
    }

    _updateThemeColor(){
        const {store} = this.context
        const themeColor = store.getState().themeColor
        this.setState({themeColor})
    }

    render() {
        return (
            <div>
                <p style={{color: this.state.themeColor}}>React-Redux 的内容</p>
                <ThemeSwitch/>
            </div>
        )
    }
}

export default Content
```


还有 src/ThemeSwitch.js：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class ThemeSwitch extends Component {
    static contextTypes = {
        store: PropTypes.object
    }

    constructor(){
        super()
        this.state = {
            themeColor: ''
        }
    }

    componentWillMount(){
        this._updateThemeColor()
    }

    _updateThemeColor(){
        const {store} = this.context
        const themeColor = store.getState().themeColor
        this.setState({themeColor})
    }

    render() {
        return (
            <div>
                <button style={{color: this.state.themeColor}}>Red</button>
                <button style={{color: this.state.themeColor}}>Blue</button>
            </div>
        )
    }
}

export default ThemeSwitch;
```

效果图如下：

{% asset_img page1.png %}


然后给按钮绑定 click 监听事件，使用 store 的 dispatch 方法触发指定事件，修改 state 的颜色。同时使用 store 的 subscript 方法绑定订阅者执行函数，执行颜色更新。

在 src/ThemeSwitch.js 文件中添加代码：

```javascript
_updateThemeColor(){
    const {store} = this.context
    const themeColor = store.getState().themeColor
    this.setState({themeColor})
    store.subscribe(this._updateThemeColor.bind(this))
}

handleThemeSwitchClick(color){
    const {store} = this.context
    store.dispatch({
        type: 'CHANGE_COLOR',
        themeColor: color
    })
}
render() {
    return (
        <div>
            <button 
            style={{color: this.state.themeColor}}
            onClick={this.handleThemeSwitchClick.bind(this, 'red')}
            >Red</button>
            <button 
            style={{color: this.state.themeColor}}
            onClick={this.handleThemeSwitchClick.bind(this, 'blue')}
            >Blue</button>
        </div>
    )
}
```

给 Header.js、Content.js 文件中添加代码：

```javascript
_updateThemeColor(){
    const {store} = this.context
    const themeColor = store.getState().themeColor
    this.setState({themeColor})
    store.subscribe(this._updateThemeColor.bind(this))
}
```

现在点击按钮，就会执行修改颜色的动作 **CHANGE_COLOR**， 然后修改全局的主题颜色变量 themeColor，进而修改所有内容的颜色：

{% asset_img page2.png %}