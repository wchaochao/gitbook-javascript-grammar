# 上下文

标签（空格分隔）： JavaScript语法

---

## 可执行代码

* `global code`: 全局代码，作为 ECMAScript 程序处理的源代码文本
* `function code`: 函数代码，函数体对应的源代码文本
* `eval code`: eval代码，提供给 eval 内置函数的源代码文本

### 严格模式

* 全局代码严格模式：以Use Strict Directive开头
* 函数代码严格模式
  * 函数声明、函数表达式以Use Strict Directive开头或被包含在严格模式代码中
  * new Function创建的函数以Use Strict Directive开头
* eval代码严格模式：以Use Strict Directive开头或被严格模式的代码直接调用

## 创建

当程序进入可执行代码时会创建对应的执行上下文，并被压入上下文堆栈中

* 执行全局代码，创建全局上下文
* 执行函数代码，创建函数上下文
* 执行eval代码，创建eval上下文

## 组成

* `this`: 上下文的this值
* `Scope`: 上下文的作用域
 * 作用域与父作用域形成作用域链，查找标识符时沿着作用域链查找

## 过程

* 初始化
* 声明绑定
* 执行

### 初始化

#### 全局上下文初始化

```javascript
/*
 * this值为全局对象
 * 作用域为全局作用域
 */
```

#### 函数上下文初始化

```javascript
/*
 * this值由F.[[Call]]方法的thisArg决定
 *      严格模式，this为thisArg
 *      非严格模式
 *          thisArg不能转换为Object时，this为全局对象
 *          thisArg能转换为Object时，this为ToObject(thisArg)
 * 作用域为基于F.[[Scope]]生成的新作用域
 */
```

#### eval上下文初始化

```javascript
/*
 * 无调用上下文或非直接调用
 *     this值为全局对象
 *     作用域为全局作用域
 * 有调用上下文或直接调用
 *     非严格模式，this、作用域为调用上下文的值
 *     严格模式，this为调用上下文的值，作用域为基于调用上下文词法创建的新作用域
 */
```

### 声明绑定

函数上下文中将形参、arguments绑定到上下文作用域中

```javascript
/*
 * 形参绑定
 *     未绑定时先创建绑定，设置绑定值为对应的实参或undefined
 *     已绑定时修改绑定值为对应的实参或undefined
 * arguments绑定，绑定值为arguments对象
 */
```

全局、函数、eval上下文中将函数声明、变量声明绑定到上下文作用域中

```javascript
/*
 * 函数声明绑定
 *     未绑定时创建绑定，设置绑定值为声明的函数对象
 *     已绑定时修改绑定值为声明的函数值
 * 变量声明绑定
 *     未绑定时创建绑定，设置绑定值为undefined
 *     已绑定时不做处理
 * eval代码的声明绑定可配置（configurable为true）
 */
```

示例

```javascript
let n = 1
function foo (a, b, c) {
  let s = 's'
  function bar (m, n) {}
}
foo(1, 2)

// 初始化
// 全局上下文
{
  foo: foo,
  n: undefined
}

// 函数上下文
{
  a: 1,
  b: 2,
  c: undefined,
  arguments: {
    0: 1,
    1: 2,
    length: 2
  },
  bar: bar,
  s: undefined
}
```

示例

```javascript
// 严格模式
function foo (a, b) {
  'use strict'
}
foo(1, 2, 3)

// arguments
{
  [[Class]]: 'arguments',
  [[Prototype]]: Object.prototype,
  length: 3,
  0: 1,
  1: 2,
  2: 3,
  [[ParameterMap]]: {},
  caller: [[ThrowTypeError]],
  callee: [[ThrowTypeError]]
}

// 非严格模式
function foo (a, b) {
}
foo(1, 2, 3)

// arguments
{
  [[Class]]: 'arguments',
  [[Prototype]]: Object.prototype,
  length: 3,
  0: 1,
  1: 2,
  2: 3,
  [[ParameterMap]]: {
    0: map(a),
    1: map(b)
  },
  callee: foo
}
```

## 退出

* return退出一个上下文
* 抛出错误退出一个或多个上下文

示例

```javascript
// return 退出上下文
function foo () {
  return 1
}

// throw 退出上下文
function foo () {
  throw new Error('error')
}
```

## 程序执行

* 同步: 依次执行语句
* 异步: 将异步代码交给其他线程执行，接着执行主线程后面的代码

> * 所有同步任务都在主线程上执行，形成一个执行栈
> * 主线程发起异步请求时，相应的工作线程就会去执行异步任务，主线程则继续执行后面的代码
> * 主线程之外，还存在一个"任务队列"，只要异步任务有了运行结果，就在"任务队列"之中放置一个消息。
> * 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，执行事件对应的回调。
> * 主线程把当前的事件执行完成之后,会再去读取任务队列,如此反复重复执行,就形成了事件循环

![调用栈](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-javascript-grammar/callstack.png)

## 垃圾收集

找出不再继续使用的变量，并释放其占用的内存

* 标记清除：从根对象开始标记所有可到达的变量，释放不可到达的变量内存
* 解除值的引用能让值脱离执行环境，以便垃圾收集器下次运行时将其回收

