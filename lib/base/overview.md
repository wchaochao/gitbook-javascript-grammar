# 概述

标签（空格分隔）： JavaScript语法

---

## 介绍

一门嵌入式、单线程、弱类型、解释型、面向对象的语言

> * 嵌入式：只提供核心语法，依赖宿主环境（如浏览器，Node）提供API
> * 单线程：有一个主线程和若干个工作线程
> * 弱类型：无类型声明，可任意转换
> * 解释型：解释执行，非编译
> * 面向对象：适合面向对象和函数式编程

## 运行

### 浏览器环境

提供的API

* `DOM`: 网页操作接口
* `BOM`: 浏览器操作接口

#### 嵌入脚本

按出现的先后顺序依次加载解析

```html
<script type="text/javascript">
  console.log('javascript');
</script>

<noscript>本页面需要浏览器支持（启用）JavaScript</noscript>
```

#### 嵌入脚本文件

一般放在`body`元素的最后面

```html
<body>
  <!--内容-->
  <script src="myscript"></script>
</body>
```

### Node环境

提供的API

* 文件操作API
* 网络通信API

使用`nodejs`执行`js`文件

### 在线运行

使用[codepen](https://codepen.io)、[jsbin](http://jsbin.com/?html,output)、[jsfiddle](https://jsfiddle.net/)等在线工具进行在线编辑和实时预览
