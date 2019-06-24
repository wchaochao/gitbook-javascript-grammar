# 函数

标签（空格分隔）： JavaScript语法

---

## 定义

```
FunctionDeclaration : // 函数声明
  function Identifier ( FormalParameterList(opt) ) { FunctionBody }

FunctionExpression : // 函数表达式
  function Identifier(opt) ( FormalParameterList(opt) ) { FunctionBody }

FormalParameterList : // 形参列表
  Identifier // 单参数
  FormalParameterList , Identifier // 多参数

FunctionBody : // 函数体
  SourceElements(opt) // 源代码
```

### 函数声明

```javascript
FunctionDeclaration : function Identifier ( FormalParameterList(opt) ) { FunctionBody }

/**
 * 创建函数对象，形参为FormalParameterList，函数体为FunctionBody，作用域为当前作用域，严格模式为strict
 * 将创建的函数对象绑定到Identifier上
 */
```

示例

```javascript
function foo () {}
```

### 函数表达式

#### 匿名函数

```javascript
FunctionExpression : function ( FormalParameterList(opt) ) { FunctionBody }

/**
 * 创建函数对象，形参为FormalParameterList，函数体为FunctionBody，作用域为当前作用域，严格模式为strict
 * 返回该函数对象
 */
```

示例

```javascript
let foo = function () {}
```

#### 命名函数

使用命名函数方便调试

```javascript
FunctionExpression : function Identifier ( FormalParameterList(opt) ) { FunctionBody }

/**
 * 基于当前上下文创建一个新作用域
 * 创建函数对象，形参为FormalParameterList，函数体为FunctionBody，作用域为新作用域 ，严格模式为strict
 * 新作用域中绑定Identifier，值为创建的函数对象
 * 返回创建的函数对象
 */
```

示例

```javascript
let foo = function foo () {}
```

### 函数体

```javascript
FunctionBody : SourceElements(opt)

/**
 * 包含在严格模式代码中或开头有严格模式指令，则为严格模式
 * 依次执行FunctionBody中的语句
 *     有throw, 抛出错误
 *     有return, 返回值
 *     无return, 返回undefined
 */
```

示例

```javascript
// 严格模式
function foo () {
  'use strict'
}

// return
function foo () {
  return 1
}

// throw
function foo () {
  throw 1
}

// normal
function foo () {}
```

### 严格模式限制

```javascript
/**
 * 严格模式下
 * 形参列表中有eval或arguments，抛出SyntaxError
 * 形参列表有重复的形参，抛出SyntaxError
 */
```

示例

```javascript
// 参数为eval
function foo (eval) {
  'use strict'
}

// 重复参数
function foo (a, a) {
  'use strict'
}
```

## 创建

```javascript
createFunction(FormalParameterList, FunctionBody, Scope, Strict)
```

### 属性

```javascript
/**
 * 创建函数对象
 * [[Class]]属性为'Function'
 * [[Prototype]]属性为Function.prototype
 * [[Extensible]]属性为true
 * [[FormalParameters]]属性为FormalParameterList
 * [[Code]]属性为FunctionBody
 * [[Scope]]属性为Scope
 * length属性为形参个数
 * prototype属性为原型对象，有constructor属性指向F
 * 严格模式下，访问caller、arguments属性抛出TypeError
 */
```

示例

```javascript
function foo (a, b) {
  'use strict'
  console.log(a, b)
}

// foo对象
{
  [[Class]]: 'Function',
  [[Prototype]]: Function.prototype,
  [[Extensible]]: true,
  [[FormalParameters]]: List{a, b}
  [[Code]]: FunctionBody,
  [[Scope]]: Global Environment,
  length: 2,
  prototype: {
    constructor: foo
  },
  caller: thrower,
  arguments: thrower
}
```

### 方法

#### F.[[Call]] (thisArg, argList)

执行函数

```javascript
/**
 * 创建函数上下文
 *     this为thisArg
 *     作用域为基于F.[[Scope]]创建的新作用域
 * 压入上下文堆栈并初始化
 *     初始化形参、arguments、函数声明、变量声明
 * 执行上下文
 *     执行F.[[Code]]代码，返回结果
 * 退出函数上下文
 */
```

示例

```javascript
// throw
functon foo () {
  throw 1
}
foo()

// return
functon bat () {
  return 1
}
bat()

// normal
functon cat () {
  let a = 1
  console.log(a)
}
cat()
```

#### F.[[Construct]] (argList)

创建实例对象

```javascript
/**
 * 创建实例对象
 * [[Class]]属性为'Object'
 * [[Extensible]]属性为true
 * [[Extensible]]属性为F.prototype，F.prototype不为对象则为Object.prototype
 * 调用F的[[Call]]方法，thisArg为obj，参数列表为argList，结果为result
 *     result是Object类型，返回result
 *     result不是Object类型，返回obj
 */
```

示例

```javascript
functon Rect (width, height) {
  this.width = width
  this.height = height
}
let r = new Rect(20, 10)

// 实例对象
{
  [[Class]]: 'Object',
  [[Extensible]]: true,
  [[Prototype]]: Rect.[[Get]]('prototype'),
  width: 20,
  height: 10
}
```

#### F.[[HasInstance]] (V)

判断V是否是F的实例对象

```javascript
/**
 * V不为对象，返回false
 * 遍历V的原型链
 *     F.prototype在V的原型链上，返回true
 *     F.prototype不在V的原型链上，返回false
 */
```

示例

```javascript
1 instanceof Number // false
let a = new Number(1)
a instanceof Number // true
a instanceof Object // true
```

## 参数

* 实参：函数调用时传递的参数
* 形参：函数定义时声明的参数
 * 接收对应的实参，无对应实参时为undefined
* arguments: 接收所有实参的类数组对象
 * 非严格模式与形参共享实参

### arguemnts对象

```javascript
/*
 * 创建Arguments对象
 * [[Class]]属性为'Arguments'
 * [[Prototype]]属性为Object.prototype
 * length属性为实参个数
 * <index>属性为对应实参，非严格模式下与对应的形参共享值
 * [[ParameterMap]]属性为共享的形参
 * 非严格模式下，callee属性为func
 * 严格模式下，访问callee、caller属性抛出TypeError
 */
```
