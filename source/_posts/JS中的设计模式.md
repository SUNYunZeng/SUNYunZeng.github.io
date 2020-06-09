---
title: JS中的设计模式（上）
comments: true
toc: true
date: 2020-06-01 15:33:12
categories: Web前端
tags: JavaScript
---

# 单例模式

js 程序中很多情况下只需要创建一个对象，例如全局缓存、widows 对象、线程池、以及界面中具备唯一性的窗口，此时需要利用单例模式来保证对象的唯一性。

使用代理实现 **单一职原则** ，将创建单例的 DIV 与管理代理的代码分开。

```javascript
// 创建 DIV 逻辑
CreateDiv = function(html){
    this.html = html;
    this.init();
}

CreateDiv.prototype.init = function(){
    const div = document.createElement('div');
    div.innerHtml = this.html;
    document.body.appendChild(div);
}

SingletonProxy = (function(){
    let instance = null;
    return function(html){
        return instance || instance = new CreateDiv(html)
    }
})();
```

# 策略模式

很多公司的年终奖是根据员工的工资基数和年底绩效情况来发放的。例如，绩效为 S的人年终奖有 4倍工资，绩效为A的人年终奖有 3倍工资，而绩效为 B的人年终奖是 2倍工资。假设财 务部要求我们提供一段代码，来方便他们计算员工的年终奖。

我们可以采用策略模式，对于每个等级的奖金都创建一个策略，然后再一个计算环境中调用对应策略，得到相应结果。

```javascript
var strategies = { 
    "S": function( salary ){ return salary * 4;
    },
    "A": function( salary ){ return salary * 3;
    },
    "B": function( salary ){ return salary * 2;
    } 
};

var calculateBonus = function( level, salary ){
     return strategies[ level ]( salary );
}; 

console.log( calculateBonus( 'S', 20000 ) ); // 输出：80000 
console.log( calculateBonus( 'A', 10000 ) ); // 输出：30000
```

**例子2**

在验证表单的过程中，可以对于不同的输入项设置不同的检查策略，例如手机号格式的检校等等，下面就采用粗略模式实现变化的抽离与不变的抽离。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <form action="" id='registerForm' method="POST">
        <div>请输入用户名：<input type="text" name="userName"></div>
        <div>请输入密码：<input type="text" name="password"></div>
        <div>请输入手机号码：<input type="text" name="phoneNumber"></div>
        <div><button>提交</button></div>
    </form>
</body>
<script>
    var strategies = {
        nonEmpty: function (value, errMsg) {
            if (value == '') {
                return errMsg;
            }
        },
        minLength: function (value, length, errMsg) {
            if (value.length < length) {
                return errMsg;
            }
        },
        isPhoneNumber: function (value, errMsg) {
            if (!/(^1[3|5|8][0-9]{9}$)/.test(value)) {
                return errMsg;
            }
        }
    }

    var Validator = function () {
        this.cache = [];
    }

    Validator.prototype.add = function (dom, rules) {
        let self = this;
        for (rule of rules) {
            (function (rule) {
                // 闭包保存中间变量 rule，防止多个函数应用同一个出问题
                let errMsg = rule.errMsg;
                let strategy = rule.strategy.split(':');
                self.cache.push(
                    function () {
                        let strategyKey = strategy.shift();
                        let value = dom.value;
                        let args = [value, ...strategy, errMsg];
                        return strategies[strategyKey].apply(dom, args);
                    }
                )
            })(rule)
        }
    }

    Validator.prototype.start = function () {
        for (evalFuc of this.cache) {
            let errMsg = evalFuc()
            console.log(errMsg)
            if (errMsg) {
                return errMsg
            }
        }
    }
    let registerForm = document.getElementById('registerForm');
    let ValidatorFunc = function () {

        let validator = new Validator();
        // 添加多个策略
        validator.add(registerForm.userName, [{
                'strategy': 'nonEmpty',
                'errMsg': '输入用户名不能为空'
            },
            {
                'strategy': 'minLength:2',
                'errMsg': '用户名的最短长度不能小于2'
            }
        ]);

        validator.add(registerForm.password, [{
                'strategy': 'nonEmpty',
                'errMsg': '输入密码不能为空'
            },
            {
                'strategy': 'minLength:6',
                'errMsg': '用户名的最短长度不能小于6'
            }
        ]);

        validator.add(registerForm.phoneNumber, [{
                'strategy': 'isPhoneNumber',
                'errMsg': '输入手机号码的格式不对'
            }
        ]);

        return validator.start()
    }



    registerForm.onsubmit = function () {
        let errMsg = ValidatorFunc();
        if (errMsg) {
            alert(errMsg)
        }
    }
</script>

</html>
```

# 代理模式

代理模式是由代理者作为中间请求的中转方，来代替客户完成对代理对象的控制。

这样可以实现代理对象与客户之间的解耦，方便代码的复用。

**图片加载代理**

场景：异步加载图片非常可能非常耗时，此时图片的位置是一片空白，非常影响美观，此时在加载过程中可以利用图片代理来控制图片加载中完与加载后的逻辑控制。

```javascript
// 加载图片的操作
let myImage = (function(){
    let imgNode = document.createElement('img');
    document.body.appendChild(imgNode);
    return {
        setSrc: function(src){
            imgNode.src = src;
        }
    }
})()

// 图片加载代理
let proxyImage = (function(){
    let img = new Image;
    img.onload = function(){
        myImage.setSrc(img.src);
    }
    return {
        setSrc: function(src){
            imgNode.setSrc('http://myGif.gif');
            img.src = src;
        }
    }
})()

proxyImage.setSrc('http://realImg.png');
```

**缓存代理**

利用缓存代理调用逻辑代码并执行，由缓存代理缓存计算结果。由于很多复杂耗时的计算结果可以由缓存代理缓存，下次计算可以直接调用缓存结果。

```javascript
// 缓存代理
let createProxyFactory = (function(){
    let cache = {};
    return function(fn){
        let storeKey = Array.prototype.join.call(arguments, ',');
        if(storeKey in cache){
            return cache[storeKey]
        }
        return cache[storeKey] = fn.apply(this, arguments);
    }
})()

// 计算逻辑
let mul = function(){
    return [...arguments].reduce((prev, cur)=>{
        return prev*cur;
    })
}

let mulProxy = createProxyFactory(mul);

console.log(mulProxy(1,2,3,4)); // 24

// 直接返回缓存结果
console.log(mulProxy(1,2,3,4)); // 24
```

# 发布者-订阅者模式

发布者-订阅者模式是一种很常见的设计模式，在我们为事件绑定监听事件时，其实就运用了该模式。

例如为页面的 body 可绑定多个点击事件监听，等于注册了多个订阅者。事件真实发生时，再通知各个监听函数，执行回调函数。

在 Vue 的双向绑定实现中，就利用了发布者-订阅者模式，实现了数据更新的消息通知到各个页面更新渲染的回调函数中，实现数据变化驱动视图更新。

在两个隔离的对象之间，也可以运用发布者-订阅者模式实现消息的互通，Angular的子组件之间的通信就是利用了发布者-订阅者模式来实现。

下面构建了一个订阅者工厂函数，可以构建多个订阅者。

```javascript
let getEvent = function(){
    let client={}, _listen, _trigger, _remove;
    _listen = function(key, fn){
        if(!(key in client)){
            client[key] = []
        }
        client[key].push(fn)
    }

    _trigger = function(){
        let key = Array.prototype.shift.call(arguments)
        let fns = client[key]
        if(!fns) return false
        for(fn of fns){
            fn.apply(this, arguments)
        }
    }

    _remove = function(key, fn){
        if(!key in client) return false
        let fns = client[key]
        if(!fn){
            fns && (fns.length=0)
        }
    }

    return {
        listen: _listen,
        trigger: _trigger,
        remove: _remove
    }
}

let Event = getEvent();

Event.listen('手机降价', (value)=>console.log(`降价了 ${value} 元`))

Event.trigger('手机降价', 1000)
```