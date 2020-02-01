---
title: 自己实现简单版Vue--3. 实现数据的双向绑定和Proxy代理
comments: true
toc: true
date: 2020-02-01 18:49:53
categories: Web前端
tags: Vue
---

# 数据的双向绑定

之前我们已经实现数据影响视图，即数据更新调用setter()方法里绑定的方法，通过Dev通知Watcher更新视图。

然后我们需要实现视图影响数据进而再影响视图。

通过为input节点利用Object.addEventListener()绑定事件监听，再调用数据更新方法更新数据。

数据更改后由于之前已经实现了数据更改后页面的自动更新，由此数据自然驱动视图。

```javascript
// 编译模版具体执行
const compileUtil = {
    // ... 省略
    model(node, expr, vm) {
        const value = this.getValue(expr, vm);
        // v-model绑定对应的 Watcher, 数据驱动视图
        new Watcher(expr, vm, (newVal)=>{
            this.updater.modelUpdater(node, newVal);
        });
        // 视图 => 数据 => 视图
        node.addEventListener('input', (e)=>{
            this.setVal(expr, vm, e.target.value);
        })
        this.updater.modelUpdater(node, value);
    }
}
```
# this.$data的代理

我们可以通过在vm对象中使用this.person.name直接修改数据，而不是通过this.$data.person.name实现。

利用this.$data的代理实现。

```html
<script>
        let vm = new MVue({
            el: '#app',
            data: {
                person: {
                    name: '海贼——王路飞',
                    age: 18,
                    fav: 'film'
                },
                msg: '最简单的插值',
                htmlStr: '<h3>这是v-html</h3>'
            },
            methods:{
                handleClick: function(){
                    console.log('这是一个处理点击事件的方法');
                    // 使用代理的方式可以直接这样修改数据
                    this.person.name = '海贼王——路飞'
                }
            }
        });
    </script>
```

```javascript
class MVue {
    constructor(options) {
        // 初始元素与数据通过options对象绑定
        this.$el = options.el;
        this.$data = options.data;
        this.$options = options;
        // 通过Compiler对象对模版进行编译，例如{{}}插值、v-text、v-html、v-model等Vue语法
        if (this.$el) {
            // ... 省略
            // 通过数据代理实现 this.person.name = '海贼王——路飞'功能，而不是this.$data.person.name = '海贼王——路飞'
            this.proxyData(this.$data);
        }
    }
     //用vm代理vm.$data
     proxyData(data){
        for(let key in data){
            Object.defineProperty(this,key,{
                get(){
                    return data[key];
                },
                set(newVal){
                    data[key] = newVal;
                }
            })
        }
    }
}
```

# 项目地址

https://github.com/SUNYunZeng/ImitateVue