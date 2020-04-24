---
title: 用JS写算法
comments: true
toc: true
date: 2020-04-24 18:13:50
categories: Web前端
tags: JavaScript
---

# 用JS去除字符串前后空格

```javascript
function trim(str){
    if(!str) return "";
    return trimRight(trimLeft(str));
}

function trimLeft(str){
    // 判断字符串（去掉换行、tab等）
    const judgeStr = new String(" \t\r\n");
    if(judgeStr.indexOf(str.charAt(0))!==-1){
        let j = 1, len = str.length;
        while(j<len && judgeStr.indexOf(str.charAt(j))!==-1){
            j++;
        }
        str = str.substring(j);
    }
    return str;
}

function trimRight(str){
    const judgeStr = new String(" \t\r\n");
    let i = str.length-1;
    if(judgeStr.indexOf(str.charAt(i))!==-1){
        while(i>=0 && judgeStr.indexOf(str.charAt(i))!==-1){
            i--;
        }
        str = str.substring(0, i+1);
    }
    return str;
}
```