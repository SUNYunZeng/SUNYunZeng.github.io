---
title: 自己实现 React-Redux (四)：connect 和 mapState2Props
comments: true
toc: true
date: 2020-05-17 10:24:05
categories: 教程
tags: React
---

# 子组件直接利用 context 的问题

第三节我们实现了子组件利用 context 属性，通过统一的修改方式对全局 state 进行修改，同时利用发布者-订阅者模式实现了数据更新的监听及界面重新渲染。

但是仍然存在两个问题：

1. 子组件都是利用 context 属性来取得 store 对象，进而对全局状态 state 进行修改，这样会导致**很多重复性代码**。

2. 每个子组件都依赖了 context 属性，导致**子组件与父组件形成了强耦合**，不利于组件的复用。例如，前面的 Context 组件需要父组件定义 context 属性，否则主题颜色将无法设置。


**解决方法**：

1. 对于重复使用的代码，可以凝练提取出一个可复用的代码结构。这里可以通过高阶组件（或者装饰器模式）将 context 的控制与相关事件监听代理起来。

2. 对于强耦合问题，可以通过设置 Pure Component，即在高阶组件内部的被装饰组件尽量设计成纯组件。**纯组件**是其状态仅依赖于父组件传入的 props 以及 自己的 state，对于外界依赖极低，因此复用性极高。

# 抽离出公共的逻辑代码

我们可以设计一个 connect 函数，该函数的目标是创建一个高阶组件。connect 函数接收两个参数，一个是 **mapState2Props** 函数，一个是原始的被修饰组件 **OriginComponent。**


+ mapState2Props函数负责从父组件接收 state 并将其转化为 props 传递给被修饰组件 OriginComponent，实现 OriginComponent 与父组件 context 之间的解耦以及公共逻辑代码的抽离。

+ OriginComponent 是原始的子组件，例如 Context 组件。

新建 `react-redux.js` 文件，里面的内容如下：

```javascript
// src/react-redux.js
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export const connect = (mapState2Props)=>(OriginComponent)=>{
    class Connect extends Component {
        static contextTypes = {
            store: PropTypes.object
        }

        constructor(){
            this.state = {
                allProps: {}
            }
        }

        componentWillMount(){
            this._updateProps()
        }

        _updateProps(){
            const {store} = this.context.store
            const stateProps = mapState2Props(store.getState())
            this.setState({
                allProps: {
                    ...stateProps,
                    ...this.props
                }
            })
            // 绑定订阅者监听函数
            store.subscribe(this._updateProps.bind(this))
        }

        render() {
            return (
                <div>
                    <OriginComponent {...this.state.allProps}/>
                </div>
            )
        }
    }

    return Connect
}
```

然后再 `src/Content.js` 及 `src/Header` 文件中利用 Connect 高阶函数实现全局变量管理：

**src/Header**
```javascript

import React, { Component } from 'react'
import PropTypes from 'prop-types'
import {connect} from './react-redux'

class Header extends Component {

    static propTypes = {
        themeColor: PropTypes.string
    }


    render() {
        return (
            <div>
                <h1 style={{color: this.props.themeColor}}>动手实现React-Redux</h1>
            </div>
        )
    }
}

const mapState2Props = (state)=>{
    return {
        themeColor: state.themeColor
    }
}

Header = connect(mapState2Props)(Header)

export default Header;
```

**src/Content**
```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import ThemeSwitch from './ThemeSwitch'
import {connect} from './react-redux'

class Content extends Component {

    static propTypes = {
        themeColor: PropTypes.string
    }

    render() {
        return (
            <div>
                <p style={{color: this.props.themeColor}}>React-Redux 的内容</p>
                <ThemeSwitch/>
            </div>
        )
    }
}

const mapState2Props = (state)=>{
    return {
        themeColor: state.themeColor
    }
}

Content = connect(mapState2Props)(Content)

export default Content
```

下一节将对 *ThemeSwitch* 组件进行重构