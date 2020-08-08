---
title: JS中的设计模式（下）
comments: true
toc: true
date: 2020-06-10 12:57:36
categories: Web前端
tags: JavaScript
---

# 命令模式

命令模式中的命令（command）指的是一个执行某些特定事情的指令。

命令模式最常见的应用场景是：有时候需要向某些对象发送请求，但是并不知道请求的接收者是谁，也不知道被请求的操作是什么。此时希望用一种松耦合的方式来设计程序，使得请求发送者和请求接收者能够消除彼此之间的耦合关系。

例如前端开发分为界面开发以及代码逻辑开发两部分，界面开发的同学设计了一个刷新按钮，点击可以执行刷新操作。

```html
<button id="refreshBtn">刷新</button>
<script>
let refreshBth = document.getElementById('refreshBtn');
</script>
```

但是这时候负责开发实际代码逻辑的同学由于业务比较复杂，因此还没来得及开发完成，此时界面开发的同学不知道如何调用实际的刷新操作。

此时可以利用命令模式，统一规定了命令的执行需要调用 execute 方法。

```javascript
let setCommand = function(button, command){
    button.onclick = function(){
        command.execute();
    }
}
```

此时负责代码逻辑的同学直接实现带 execute 功能的对象或函数即可。

```javascript
let ButtonRefresh = {
    refresh: function(){
        console.log('按钮刷新')
    }
}

let RefreshCommand = function(receiver){
    return function(){
        receiver.refresh();
    }
}


let refreshCommand = new RefreshCommand(ButtonRefresh);

setCommand(refreshBth, refreshCommand);
```

**宏命令**

宏命令即执行一系列的名利，例如点外面时候的 下单 -> 做餐 -> 包装 -> 配送 ->取餐 一系列的命令。

```javascript
let OrderCommand = {
    execute: function(){
        console.log('下单啦')
    }
}

let CookCommand = {
    execute: function(){
        console.log('做餐啦')
    }
}

let PackCommand = {
    execute: function(){
        console.log('包装啦')
    }
}

let DeliveryCommand = {
    execute: function(){
        console.log('配送啦')
    }
}

let PickCommand = {
    execute: function(){
        console.log('取餐啦')
    }
}

let MacroCommand = function(){
    return {
        commandList: [],
        add: function(command){
            this.commandList.push(command)
        },
        execute: function(){
            for(command of this.commandList){
                command.execute();
            }
        }
    }
}

let macroCommand = MacroCommand()
macroCommand.add(OrderCommand)
macroCommand.add(CookCommand)
macroCommand.add(PackCommand)
macroCommand.add(DeliveryCommand)
macroCommand.add(PickCommand)

macroCommand.execute()

// 下单啦
// 做餐啦
// 包装啦
// 配送啦
// 取餐啦
```

# 组合模式

上述一系列的命令可以组合成一些复杂的命令，例如命令直接存在树状结构，此时可以采用组合模式进行命令的组合。

```javascript
let MacroCommand = function(){
    return {
        commandList: [],
        add: function(command){
            this.commandList.push(command)
        },
        execute: function(){
            for(command of this.commandList){
                command.execute();
            }
        }
    }
}

let  macroCommand1 = MacroCommand()
let  macroCommand2 = MacroCommand()
let  macroCommand3 = MacroCommand()
let  macroCommand4 = MacroCommand()

macroCommand1.add(macroCommand2)
macroCommand2.add(macroCommand3)
macroCommand2.add(macroCommand4)

macroCommand1.execute()
```

**文件扫描系统**

文件系统时典型的树状结构，可以通过文件夹与文件的组合，模拟树状结构。

```javascript
class Folder{
    constructor(){
        this.files = []
    }

    add(){
        this.files.push(...arguments)
    }

    scan(){
        for(let file of this.files){
            file.scan()
        }
    }
}

class File{
    constructor(name){
        this.name = name
    }
    add(){
        throw new Error('文件不能添加文件')
    }
    scan(){
        console.log(this.name)
    }
}

let folder1 = new Folder()
let folder2 = new Folder()
let folder3 = new Folder()

let file1 = new File('文件1')
let file2 = new File('文件2')
let file3 = new File('文件3')
let file4 = new File('文件4')

folder1.add(folder2)
folder2.add(folder3)
folder2.add(file1, file2)
folder3.add(file3, file4)

folder1.scan()

// 文件3
// 文件4
// 文件1
// 文件2
```