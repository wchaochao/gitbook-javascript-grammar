# 一元表达式

标签（空格分隔）： JavaScript语法

---

## delete

```javascript
UnaryExpression : delete UnaryExpression

/*
 * UnaryExpression不为标识符或属性访问，直接返回true
 * UnaryExpression为标识符
 *     未声明
 *         严格模式，抛出SyntaxError
 *         非严格模式，返回true
 *     已声明
 *         严格模式，抛出SyntaxError
 *         非严格模式，返回false
 * UnaryExpression为属性访问
 *     属性不存在，返回true
 *     属性存在且可删除，删除属性并返回true
 *     属性存在且不可删除
 *         严格模式，抛出TypeError
 *         非严格模式，返回true
 */
```

示例

```javascript
// 不为标识符或属性访问
delete 1 // true

// 未声明标识符
delete a // 严格模式抛出SyntaxError, 非严格模式返回true

// 已声明标识符
let a = 1
delete a // 严格模式抛出SyntaxError, 非严格模式返回false

let o = {a: 1}

// 属性不存在
delete o.b // true

// 属性存在且可删除
delete o.a // true

// 属性存在且不可删除
delete 'a'.length // 严格模式抛出TypeError, 非严格模式返回false
```

## void

```javascript
UnaryExpression : void UnaryExpression

/*
 * 计算UnaryExpression的值并返回undefined
 */
```

示例

```javascript
let a = 1
void(a++) // 返回undefined, a = 2
```

## typeof

```javascript
UnaryExpression : typeof UnaryExpression

/*
 * UnaryExpression为未声明标识符，返回'undefined'
 * UnaryExpression为Undefined类型, 返回'undefined'
 * UnaryExpression为Null类型, 返回'object'
 * UnaryExpression为Boolean类型, 返回'boolean'
 * UnaryExpression为Number类型, 返回'number'
 * UnaryExpression为String类型, 返回'string'
 * UnaryExpression为Object类型, 函数返回'function', 其他返回'object'
 */
```

示例

```javascript
typeof a // 'undefined'
typeof undefined // 'undefined'
typeof null // 'object'
typeof true // 'boolean'
typeof 1 // 'number'
typeof 'a' // 'string'
typeof /a/i // 'object'
function foo () {}
typeof foo // 'function'
```

## 前置自增

```javascript
UnaryExpression : ++ UnaryExpression

/*
 * 对UnaryExpression作加一操作并返回现值
 */
```

示例

```javascript
let a = 1
++a + ++a // 2 + 3, a = 3
```

## 前置自减

```javascript
UnaryExpression : -- UnaryExpression

/*
 * 对UnaryExpression作减一操作并返回现值
 */
```

示例

```javascript
let a = 1
--a + --a // 0 + (-1), a = -1
```

## 取正

```javascript
UnaryExpression : + UnaryExpression

/*
 * UnaryExpression转换为Number类型
 */
```

示例

```javascript
+undefined // NaN
+null // +0
+true // 1
+false // 0
+' \n ' // 0
+' 0123.45 ' // 123.45
+' 0x12 ' // 18
+({}) // NaN
```

## 取负

```javascript
UnaryExpression : - UnaryExpression

/*
 * UnaryExpression转换为Number类型，再取负值
 */
```

示例

```javascript
-undefined // NaN
-null // -0
-true // -1
-false // -0
-' \n ' // -0
-' 0123.45 ' // -123.45
-' 0x12 ' // -18
-({}) // NaN
```

## 按位取反

```javascript
UnaryExpression : ~ UnaryExpression

/*
 * UnaryExpression转换为Int32类型
 * 按位取反，结果为Int32类型
 */
```

示例

```javascript
~NaN // -1
~Infinity // -1
~-Infinity // -1
~+0 // -1
~-0 // -1
~15 // -16
~-15 // 16
~(Math.pow(2, 31) + 1) // 2^31 - 2
~(-Math.pow(2, 31) - 1) // -2^31
```

## 逻辑取反

```javascript
UnaryExpression : ! UnaryExpression

/*
 * UnaryExpression转换为Boolean类型，再取反
 */
```

示例

```javascript
!undefined // true
!null // true
!0 // true
!NaN // true
!'' // true
!({}) // false
```
