# 简单语句

标签（空格分隔）： JavaScript语法

---

## 语句

执行某个操作

```
Statement : // 语句
  EmptyStatement // 空语句
  Block // 块语句
  VariableStatement // 变量语句
  ExpressionStatement // 表达式语句
  LabelledStatement // label语句
  ContinueStatement // continue语句
  BreakStatement // break语句
  ReturnStatement // return语句
  ThrowStatement // throw语句
  DebuggerStatement // debugger语句
  IfStatement // if语句
  SwitchStatement // switch语句
  IterationStatement // 循环语句
  WithStatement // with语句
  TryStatement // try语句
```

## 空语句

```javascript
EmptyStatement : ;

/**
 * 不执行任何操作
 */
```

## 块语句

```javascript
Block : { StatementList(opt) }

/**
 * 依次执行语句
 * 有异常抛出，抛出异常并停止往下执行
 * 无异常抛出，继续往下执行
 */
```

示例

```javascript
{
  delete o.a // 抛出ReferenceError
  let a = 1 // 不执行
}

{
  let a = 1 // 继续执行
  delete o.a // 抛出ReferenceError
}

{
  let a = 1
  a++ // 1
}

{1;;;;;} // 1
```

## 变量语句

```
VariableStatement : // 变量语句
  var VariableDeclarationList ; // var 变量声明列表;

VariableDeclarationList : // 变量声明列表
  VariableDeclaration // 单个变量声明
  VariableDeclarationList , VariableDeclaration // 多个变量声明

VariableDeclaration : // 单个变量声明
  Identifier Initialiser(opt) // 标识符 变量初始化

Initialiser : // 变量初始化
  = AssignmentExpression // = 赋值语句
```

### 变量声明

```javascript
VariableDeclaration : Identifier

/**
 * 声明变量
 */
```

示例

```javascript
var a
```

### 变量初始化

```javascript
VariableDeclaration : Identifier Initialiser

/**
 * 声明变量并赋值
 */
```

示例

```javascript
var a = 1
```

### 多个变量声明

```javascript
VariableDeclarationList : VariableDeclarationList , VariableDeclaration

/**
 * 依次声明变量
 */
```

示例

```javascript
var c,
    d = 2
```

### 严格模式限制

```javascript
/**
 * 严格模式下，标识符为eval或arguments时抛出SyntaxError
 */
if (strict) {
  if (<identifier-name-string> === 'eval' || <identifier-name-string> === 'arguments') {
    throw SyntaxError
  }
}
```

示例

```javascript
'use strict'
let eval = 1 // 抛出SyntaxError
```

## 表达式语句

```javascript
ExpressionStatement : [lookahead ∉ {{, function}] Expression ;

/**
 * 计算Expression并返回结果
 */
```

示例

```javascript
1; // 1

let a
a = 1 // 1

let o = {}
o.a = 1 // 1
delete o.a // true

let foo = function () {}
foo() // undefined
```
