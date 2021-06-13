---
title: "认识TypeScript"
date: 2021-02-08T21:36:26+08:00
description: "TypeScript之旅"
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Herschel
authorEmoji: 🎅
pinned: false
tags:
- TypeScript
series:
-
categories:
- TypeScript
image:
---
## TypeScript的介绍
TypeScript是一种有微软开发的开源，跨平台的编程语言，最终会被编译成为JavaScript代码。

2012年10月，微软发布了首个公开版本的TypeScript，2013年6月19日，在经历到了一个预览版之后微软正式发布了正式版TypeScript.

TypeScript的作者是安德斯-海尔斯伯格，C#的首席架构师，它是开源和跨平台的编程语言。

TypeScript扩展了JavaScript的语法，所以任何现有的JavaScript程序可以运行在TypeScript环境中。

TypeScript是为大型应用的开发而设计，并且可以编译为JavaScript。

TypeScript是JavaScript的一个超集，主要提供了类型系统和对ES6+的支持，它由Microsoft开发，代码开源于Github上。

## TypeScript的特点
- 始于JavaScipt,归于JavaScript
TypeScript可以编译出纯净、简洁的JavaScript代码，并且可以运行在任何浏览器上，Node.js环境中和任何支持ECMAScript3（或更高版本）的JavaScript引擎中。

- 强大的类型系统
`类型系统`允许JavaScript开发者在开发JavaScript应用程序是使用高效的开发工具和常用操作，比如静态检查和代码重构。

- 先进的JavaScript
TypeScript提供最新的和不断发展的JavaScript特性，包括那些来自2015年的ECMAScript和未来的提案中的特性，比如异步功能和Decrators，以帮助建立健壮的组件。

## 小结
TypeScript在社区的流行度越来越高，它非常适用于一些大型项目，也非常适用于一些基础库，极大地帮助我们提升了开发效率和体验。

## TypeScript的安装
命令行如下，全局安装TypeScript
```bash
npm i -g typescript
```

安装完之后，输入以下命令查看是否安装成功：

```bash
tsc -V
```

## 类型注解
`类型注解`是一种轻量级的为函数或者变量添加的约束。

```ts
(()=>{
    function showMsg(msg:string) {
       return "hello," + msg
    }
    let msg = "迪儿"
    console.log(showMsg(msg))
 })()
```
## 接口
`接口`是一种能力， 一种约束。

```ts
(()=>{
    // define an interface
    interface IPerson {
        firstName: string
        lastName:  string
    }
    // print name
    function showFullName(person:IPerson) {
        return person.firstName + '_' + person.lastName
    }
    // define an object
    const person = {
        firstName: '东方'
        lastName:  '不败'
    }
    console.log(showFullName(person))
 })()
```
