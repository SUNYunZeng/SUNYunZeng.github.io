---
title: 自己实现 React-Redux (五)：实现 mapDispatch2Props
comments: true
toc: true
date: 2020-05-17 12:35:51
categories: 教程
tags: React
---

# dispatch 的代理工作

在 `src/ThemeSwitch.js` 中，ThemeSwitch 组件通过 store 的 dispatch 方法实现了数据的更改，这也导致了组件的强耦合。

我们的思路是让高阶组件 Connect 代替 ThemeSwitch 来实现 dispatch 功能，同时通过 props 将方法传递给 ThemeSwitch 组件。

修改 `src/react-redux.js`文件：

```javascript
export const connect = (mapState2Props, mapDispatch2Props)=>(OriginComponent)=>{
class Connect extends Component {

    componentWillMount(){
        this._updateProps()
    }

    _updateProps(){
        const {store} = this.context

        let stateProps = mapState2Props? mapState2Props(store.getState(), this.props) : {}
        let dispatchProps = mapDispatch2Props? mapState2Props(store.dispatch, this.props) : {}

        this.setState({
            allProps: {
                ...stateProps,
                ...dispatchProps,
                ...this.props
            }
        })
        // 绑定订阅者监听函数
        store.subscribe(this._updateProps.bind(this))
    }
/*以下代码略*/
}
```

# 定义 mapDispatch2Props 函数

在 `src/ThemeSwitch.js` 文件中定义 mapDispatch2Props 函数:

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import {connect} from './react-redux'

class ThemeSwitch extends Component {
    static propTypes = {
        themeColor: PropTypes.string,
        handleThemeSwitchClick: PropTypes.func
    }

    render() {
        return (
            <div>
                <button 
                style={{color: this.props.themeColor}}
                onClick={this.props.handleThemeSwitchClick.bind(this, 'red')}
                >Red</button>
                <button 
                style={{color: this.props.themeColor}}
                onClick={this.props.handleThemeSwitchClick.bind(this, 'blue')}
                >Blue</button>
            </div>
        )
    }
}

const mapState2Props = (state)=>{
    return {
        themeColor: state.themeColor
    }
}

const mapDispatch2Props = (dispatch)=>{
    return {
        handleThemeSwitchClick : function(color){
            dispatch({
            type: 'CHANGE_COLOR',
            themeColor: color
        })}
    }
}

ThemeSwitch = connect(mapState2Props, mapDispatch2Props)(ThemeSwitch)

export default ThemeSwitch;
```

这样 ThemeSwitch 组件的代码就变得非常整洁，而且扩展性也大大提高。