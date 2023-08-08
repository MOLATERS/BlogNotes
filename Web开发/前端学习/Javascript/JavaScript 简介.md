---
title: JavaScript 简介
categories:
- 网页开发
- 前端
tags:
- JavaScript
---
什么是JavaScript(JS)

- Js是运行在浏览器中的web前段脚本语言
- 伴随Node的出现，JS可以脱离浏览器宿主运行
- JS是一种解释性动态编程语言

安装Node和VsCode [登录](https://node.js.org/)

```js
console.log("hello World");
//单行注释
128 //十进制的整数
077 //八进制整数
0xFF //十六进制整数
3.14 //浮点数
NAN //非数值
Infinity //无穷大

```

标识符规则

必须以字母下划线或美元符号

变量

使用var申明变量

```js
var a
var names=['Alice','Bob',Tom],count=names.length;
if(true){
var name=5;
console.log(name);
}

```

let 和 const

let保留C语言的特点

全局变量和局部变量