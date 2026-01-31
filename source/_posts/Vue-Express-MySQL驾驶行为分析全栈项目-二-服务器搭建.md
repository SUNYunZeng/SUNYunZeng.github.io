---
title: 'Vue + Express + MySQL驾驶行为分析全栈项目(二): 服务器搭建'
comments: true
toc: true
date: 2019-12-10 14:40:30
categories: 教程
tags: Vue
---

# 背景

自己做了一点点的小尝试：基于前馈神经网络 LSTM 的个体出行目的地预测模型，基于个体历史出行数据，模型可以实现出行目的地的实时动态预测功能。

模型其实具有实际应用功能，为了对其应用场景进行探索，拟开发一个全栈的项目，在Web客户端实现用户出行的动态预测效果，同时能够提供数据可视分析等功能。

**[项目地址](https://github.com/SUNYunZeng/AIforDriving)**

**[可视化效果](http://geoai.sunyunzeng.cn/)**

# 服务端结构设计

结构设计如下图所示：

{% asset_img server.png %}

>  **api** 文件夹存放的是网页路由查询逻辑的实现代码。
> **py-script** 存放的是python的脚本及训练好的深度学习模型。
> **tables** 是基于Sequlize的数据库表模型。
> **config-default** 定义了数据库连接信息。
> **db.js** 定义了Sequlize对象模型。
> **func.js** 定义了一些函数接口。
> **index.js** 是服务器的启动入口，定义了路口链接的运行文件。
> **table.js** 将tables文件中的表模型统一用模块接口暴露出来。

# 服务器搭建

服务端采用了基于Node.js运行的Web应用框架——Express，首选需要安装Express框架。(前提是电脑安装了Node.js，如需安装，请参考其它网上教程)

```javascript
// 进入在server目录，执行下面命令安装Express
npm install express
```

数据库采用了MySQL数据库，首先需要安装连接数据库的库函数

```javascript
npm install --save mysql
```

然后我们采用了Sequlize库函数方便管理SQL查询，接着安装Sequlize。

```javascript
npm install --save sequelize
```

我们再安装Express的中间件 body-parser 帮助我们解析JSON、Raw、文本、URL-encoded格式的请求体。

```javascript
npm install --save body-parser
```

然后再index.js编写我们的服务端启动入口。

```javascript
// node server
// use ES6

require('babel-register')({
  presets: ['env']
});

const bodyParser = require('body-parser');
const express = require('express');
const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));

// 允许跨域访问
app.all('*', function (req, res, next) {

  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  res.header('Access-Control-Allow-Methods', 'PUT,POST,GET,DELETE,OPTIONS');
  res.header('X-Powered-By', ' 3.2.1');
  res.header('Content-Type', 'application/json;charset=utf-8');

  next();
});

app.get('/', (req, res, next) => res.send('Express Server'));
app.get('/api', (req, res, next) => res.send('API Pages'));

// 监听端口
app.listen(3000, () => console.log('success listen at port:3000......'));

```

在server文件夹下用node.js启动程序，`node index`, 命令窗口会显示 'success listen at port:3000......' 字样，证明服务器启动成功！这时在浏览器中输入 `localhost:3000/api`发现浏览器输出 `API Pages`。

# 连接数据库访问数据

首先在 config-default 文件内定义数据库连接信息，如下：

```javascript
module.exports = {
  host: 'localhost',
  user: 'root',
  password: '**********',
  database: 'your_database',
  port: '3306',
  dialect: 'mysql'
};
```
然后 在db.js 文件中定义Sequlize模型：

```javascript
const Sequelize = require('sequelize');
const config = require('./config-default');

let sequelize = new Sequelize(config.database, config.user, config.password, {
  host: config.host,
  dialect: config.dialect,
  pool: {
    max: 5,
    min: 0,
    idle: 10000
  }
});

let defineModel = function (name, attributes) {
  let attrs = {};
  for (let key in attributes) {
    let value = attributes[key];
    // if the value is the mainKey
    if (typeof value === 'object' && value['type']) {
      value.allowNull = value.allowNull || false;
      attrs[key] = value;
    } else {
      attrs[key] = {
        type: value,
        allowNull: false
      };
    }
  }
  return sequelize.define(name, attrs, {
    tableName: name,
    timestamps: false,
    freezeTableName: true
  });
};

let exp = {
  defineModel: defineModel
};

module.exports = exp;
```
**Sequlize** 是一个基于Node.js的对象关系映射（Object Relational Mapping，ORM）实现，通过对SQL语言的封装，实现model与SQL的映射，让开发者可以用OOP的方式操作数据库，优雅的生成安全、可维护的SQL代码。

这里我们把Sequlize模型的定义做成了一个的模块接口，只要指定Sequlize参数，就可以生成一个Sequlize模型，这样针对一个数据库中不同的表就可以生成不同的Sequlize对象。

之后根据自己的数据库表设计定义对应的表模型，如项目中 tables 文件夹内的 User1 文件。

```javascript
const Sequelize = require('sequelize');
const db = require('../db');

module.exports = db.defineModel('user_1',{
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    allowNull: false
  },
  weekday: Sequelize.INTEGER,
  start_time: Sequelize.INTEGER,
  dis_total: Sequelize.FLOAT,
  sem_O: Sequelize.TEXT,
  sem_D: Sequelize.TEXT,
  destination: Sequelize.TEXT,
  lngs: Sequelize.TEXT,
  lats: Sequelize.TEXT,
  travel_dis: Sequelize.TEXT,
  spd: Sequelize.TEXT,
  azimuth: Sequelize.TEXT,
  norm_dict: Sequelize.TEXT,
  key_point: Sequelize.TEXT,
  sem_pt: Sequelize.TEXT,
  time: Sequelize.TEXT
});
```

因为随着项目不断深入，数据表模型也不断增多。为了统一管理，建立 tables 模块将所有表模型统一打包，用户可按需导入。

```javascript
// tables.js 文件
// scan all models defined in models:
const fs = require('fs');
const user1 = require('./tables/User1');
const user2 = require('./tables/User2');
const user3 = require('./tables/User3');
const user4 = require('./tables/User4');

module.exports = {
  'user1': user1,
  'user2': user2,
  'user3': user3,
  'user4': user4,
};
```

之后再 api 文件夹中定义不同路由的处理逻辑，这里主要是对数据库的访问。

例如 searchAll.js 文件是对整张表进行访问，其内容如下：

```javascript
const table = require('../table');
const express = require('express');
const router = express.Router();
import {jsonWrite} from '../func';

router.post('/', (req, res) => {
  let params = req.body;
  if(params['tableName'] === undefined){
    jsonWrite(res, undefined);
    return;
  }
  let user = table[params.tableName];
  (async () => {
    let traj = await user.findAll();
    jsonWrite(res, traj);
  })();
});

module.exports = router;
```

其中 jsonWrite 是我们在fun.js定义的根据请求结果返回res的函数，如下：

```javascript
const jsonWrite = function (res, ret) {
  if (typeof ret === 'undefined') {
    res.json({
      code: '1',
      msg: '操作失败'
    });
  } else {
    res.json(ret);
  }
};

export {jsonWrite};
```

然后，前端发送post请求，请求体中输入待查询数据表名就可以实现整张表的查询。

这里的服务器代码不能实时更新效果，最后可以采用 nodemon 包检测服务器代码变化，实现代码的实时监测更新。

```javascript
npm install -g  nodemon
```

然后利用 nodemon 启动 index.js入口文件，就可以实现服务器根据代码更新的自动重启，

```javascript
nodemon index
```