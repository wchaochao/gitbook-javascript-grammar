# 左值表达式

标签（空格分隔）： JavaScript语法

---

## 属性访问

```javascript
MemberExpression : MemberExpression [ Expression ]
MemberExpression . IdentifierName 等价于 MemberExpression [ <identifier-name-string> ]

/*
 * MemberExpression要能转换为对象
 * Expression转换为字符串
 * 获取对象属性
 */
```

示例

```javascript
// 访问未声明变量的属性
console.log(a.toString) // 抛出ReferenceError

// 访问Undefinecd、Null的属性
console.log(null.toString) // 抛出TypeError

// 访问Boolean、Number、String的属性
console.log(true.toString)

// 访问对象的属性
var obj = {
  name: 'a',
  getName () {
    return this.name
  }
}
console.log(obj.getName)
```

## new构造

### 无参数

```javascript
NewExpression : new NewExpression

/*
 * 调用构造器的[[Construct]]方法创建对象，无参数
 */
```

示例

```javascript
function Foo () {}
let o = new Foo // Foo.[[Construct]](), 返回{}
```

### 有参数

```javascript
MemberExpression : new MemberExpression Arguments

/*
 * 调用构造器的[[Construct]]方法创建对象，参数为Arguments
 */
```

示例

```javascript
function Foo (a) {
  this.a = a
}
let o = new Foo(1) // Foo.[[Construct]](List{1}), 返回{a: 1}
```

## 函数调用

```javascript
CallExpression : MemberExpression Arguments

/*
 * 调用函数的[[Call]]方法，参数为Arguments
 *     函数为对象方法，thisArg为该对象
 *     函数不为对象方法，thisArg为undefined
 */
```

示例

```javascript
// 调用普通函数
function foo () {}
foo()

// 调用Boolean、Number、String方法
console.log(true.toString())

// 调用对象方法
let obj = {
  name: 'a',
  getName () {
    return this.name
  }
}
obj.getName()
```

## 函数表达式

```javascript
MemberExpression : FunctionExpression

/*
 * 创建函数并返回
 */
```

示例

```javascript
let fn = function foo () {}
```
