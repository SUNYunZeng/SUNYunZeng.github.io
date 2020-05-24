---
title: 自己实现 React-Redux (二)：初始化工程
comments: true
toc: true
date: 2020-05-16 15:39:47
categories: 教程
tags: React
---

# 构建项目

在命令行依次输入以下命令

```java
// 利用脚手架创建新项目
npx create-react-app react-redux
```

```java
// 安装类型检查依赖
npm install --save prop-types
```

# 创建文件

安装好后在 src/ 目录下新增三个文件：Header.js、Content.js、ThemeSwitch.js。

修改 src/Header.js：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class Header extends Component {
    render() {
        return (
            <div>
                <h1>动手实现React-Redux</h1>
            </div>
        )
    }
}

export default Header;
```

修改 src/ThemeSwitch.js：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class ThemeSwitch extends Component {
    render() {
        return (
            <div>
                <button>Red</button>
                <button>Blue</button>
            </div>
        )
    }
}

export default ThemeSwitch;
```

修改 src/Content.js：

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import ThemeSwitch from './ThemeSwitch'

class Content extends Component {
    render() {
        return (
            <div>
                <p>React-Redux 的内容</p>
                <ThemeSwitch/>
            </div>
        )
    }
}

export default Content
```

修改 src/index.js：

```javascript
import React, {Component} from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import Header from './Header';
import Content from './Content';
import PropTypes from 'prop-types';

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
  <Index/>,
  document.getElementById('root')
);
```

# 启动查看界面

如下所示：

{% asset_img window.png %}