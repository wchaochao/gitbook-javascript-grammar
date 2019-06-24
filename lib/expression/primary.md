# 主值表达式

标签（空格分隔）： JavaScript语法

---

## this

```javascript
PrimaryExpression : this

/*
 * 返回当前上下文的ThisBinding
 *     全局上下文，this为全局对象
 *     函数上下文
 *         严格模式，this为函数的thisArg
 *             单纯调用函数，thisArg为undefined
 *             调用对象方法，thisArg为该对象
 *             new构造，thisArg为实例对象
 *             调用call()、apply()、bind()，thisArg为绑定的this值
 *         非严格模式，thisArg转换为对象，不能转换时为全局对象
 *     eval上下文
 *         间接调用，this为全局对象
 *         直接调用，this为调用上下文的ThisBinding
 */
```

示例

```javascript
// 全局this
console.log(this) // window对象

// 函数this
// 单纯调用函数
function func () {
  'use strict';
  return this
}
console.log(func()) // undefined

// 调用对象方法
var obj = {
  name: 'a',
  getName () {
    return this.name
  }
}
console.log(obj.getName()) // 'a'

// new构造
function Rect (width, height) {
  this.width = width
  this.heigth = height
}
console.log(new Rect(10, 20)) // {width: 10, height: 20}

// call调用
function func () {
  return Array.prototype.slice.call(arguments)
}
console.log(func(1, 2, 3)) // [1, 2, 3]
```

## 标识符

```javascript
PrimaryExpression : Identifier

/*
 * 沿着作用域链，获取标识符的引用
 */
```

示例

```javascript
console.log(typeof a) // undefined

let b = 1

function foo (b) {
  console.log(b)
}

foo(2) // 2
console.log(b) // 1
```

## 字面量

```
PrimaryExpression : Literal

Literal ::
  NullLiteral // Null字面量
  BooleanLiteral // 布尔字面量
  NumericLiteral // 数字字面量
  StringLiteral // 字符串字面量
  RegularExpressionLiteral // 正则字面量
```

示例

```javascript
// Null字面量
null

// 布尔字面量
true
false

// 数字字面量
10
0x12ab
10.12
1.23e+3

// 字符串字面量
'abc'
'\n'

// 正则字面量
/a/i
```

## 数组字面量

创建一个数组，填充元素

```
PrimaryExpression : ArrayLiteral
```

### 全空位

```javascript
ArrayLiteral : [ Elision(opt) ]

/*
 * 创建一个数组
 *    length属性为空位个数
 */
```

示例

```javascript
let arr = [,,,]
console.log(arr) // [empty × 3]
```

### 一个 前置空位 + 元素

```javascript
ElementList : Elision(opt) AssignmentExpression

/*
 * 创建一个数组
 *     设置数组第一个不为空位的元素值，索引为空位个数，值为AssignmentExpression值
 */
```

示例

```javascript
let arr = [,,1]
console.log(arr) // [empty × 2, 1]
```

### 多个 前置空位 + 元素

```javascript
ElementList : ElementList , Elision(opt) AssignmentExpression

/*
 * 根据元素列表创建一个数组
 *     设置数组最后一个不为空位的元素值，索引为ToUint32(数组长度 + 空位个数)，值为AssignmentExpression值
 */
```

示例

```javascript
let arr = [,,1,,2]
console.log(arr) // [empty × 2, 1, empty, 2]
```

### 元素列表 + 后置空位

```javascript
ArrayLiteral : [ ElementList , Elision(opt) ]

/*
 * 根据元素列表创建一个数组
 *    设置该数组的length属性为ToUint32(数组长度 + 空位个数)
 */
```

示例


```javascript
let arr = [,,1,,2,,]
console.log(arr) // [empty × 2, 1, empty, 2, empty]
```

## 对象字面量

创建一个对象，填充属性

```
PrimaryExpression : ObjectLiteral
```

### 空对象

```javascript
ObjectLiteral : { }

/*
 * 创建一个对象并返回
 */
```

示例

```javascript
let o1 = {}
```

### 单个属性

```javascript
PropertyNameAndValueList : PropertyAssignment

/*
 * 创建一个对象
 *     设置该对象的属性为指定属性
 */
```

示例

```javascript
// 数据属性
let o = {
  a: 1
}

// 访问器属性
let o = {
  get p () { return 1 },
  set p (val) { console.log(val) }
}
```

### 多个属性

```javascript
/*
 * 根据属性列表创建一个对象
 *     指定属性可以设置，设置属性
 *     指定属性不可设置，抛出SyntaxError
 *         属性重复且满足下列条件时不可设置
 *            属性的描述符发生改变
 *            属性同为数据属性，且在严格模式
 *            属性同为访问器属性，且[[Get]]或[[Set]]发生变化
 */
```

示例

```javascript
// 属性不重复
let o = {
  a: 1,
  get p () { return 1 },
  set p (val) { console.log(val) }
}

// 数据属性重复，严格模式下抛出Syntax错误
let o = {
  a: 1,
  b: 2,
  a: 2
}

// 访问器属性重复，抛出Syntax错误
let o = {
  a: 1,
  get p () { return 1 },
  get p () { return 2 }
}

// 属性描述符类型发送改变，抛出Syntax错误
let o = {
  a: 1,
  get a () { return 1 }
}
```

## 分组表达式

```javascript
PrimaryExpression : (Expression)

/*
 * 优先运算Expression
 */
```

示例

```javascript
(1 + 1) * 2 // 2 * 2

1 + 1 * 2 // 1 + 2
```
