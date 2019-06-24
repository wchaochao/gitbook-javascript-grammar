# 其他表达式

标签（空格分隔）： JavaScript语法

---

## 位运算表达式

```
BitwiseANDExpression : // 按位与
  BitwiseANDExpression & EqualityExpression

BitwiseXORExpression : // 按位异或
  BitwiseXORExpression ^ BitwiseANDExpression

BitwiseORExpression : // 按位或
  BitwiseORExpression | BitwiseXORExpression
```

### 算法

```javascript
A : A @ B // @为&, ^, |

/*
 * A、B转换为Int32类型
 * 执行相应的位运算，结果为Int32类型
 */
```

示例

```javascript
// 按位与
1 & 0 // 0
1 & -1 // 1
7 & 4 // 4

// 按位异或
1 ^ 0 // 1
1 ^ -1 // -2

// 按位或
1 | 0 // 1
1 | -1 // -1
```

## 逻辑表达式

```
LogicalANDExpression: // 逻辑与
  LogicalANDExpression && BitwiseORExpression
  
LogicalORExpression : // 逻辑或
  LogicalORExpression || LogicalANDExpression
```

### 逻辑与

```javascript
LogicalANDExpression : LogicalANDExpression && BitwiseORExpression

/**
 * LogicalANDExpression转换为false，返回LogicalANDExpression的值
 * LogicalANDExpression转换为true，返回BitwiseORExpression的值
 */
```

示例

```javascript
let o
o && o.a // undefined
'' && 1 // ''
1 && 'a' && NaN && true // NaN

if (a && a.b) {
  // ...
}
```

### 逻辑或

```javascript
LogicalORExpression : LogicalORExpression || LogicalANDExpression

/**
 * LogicalORExpression转换为true，返回LogicalORExpression的值
 * LogicalORExpression转换为false，返回LogicalANDExpression的值
 */
```

示例

```javascript
let o = {a: 1}
o || o.a // {a: 1}
'' || 1 // 1
1 || 'a' || NaN || true // 1

function saveText(text) {
  text = text || '';
  // ...
}
```

## 条件表达式

```
ConditionalExpression :
  LogicalORExpression ? AssignmentExpression : AssignmentExpression
```

### 算法

```javascript
ConditionalExpression : LogicalORExpression ? AssignmentExpression : AssignmentExpression

/**
 * LogicalORExpression转换为true，返回第一个AssignmentExpression的值
 * LogicalORExpression转换为false，返回第二个AssignmentExpression的值
 */
```

示例

```javascript
true ? 1 : 0 // 1
false ? 1 : 0 // 0
```

## 赋值表达式

```
AssignmentExpression :
  LeftHandSideExpression = AssignmentExpression // 简单赋值
  LeftHandSideExpression AssignmentOperator AssignmentExpression // 复合赋值

AssignmentOperator : one of
  *=  /=  %=  +=  -=  <<=  >>=  >>>=  &=  ^=  |=
```

### 简单赋值

```javascript
AssignmentExpression : LeftHandSideExpression = AssignmentExpression

/*
 * 设置LeftHandSideExpression值为AssignmentExpression，并返回AssignmentExpression的值
 *     LeftHandSideExpression为未声明变量时
 *          严格模式，抛出ReferenceError
 *          非严格模式，设置全局属性
 *     LeftHandSideExpression为原始值属性时
 *          严格模式，抛出ReferenceError
 *          非严格模式，静默失败
 */
```

示例

```javascript
let a = 1
let b
b = true

// 严格模式，抛出ReferenceError
// 非严格模式，window.c = 1
c = 1

true.a = 1 // 严格模式抛出ReferenceError
```

### 复合赋值

```javascript
AssignmentExpression : LeftHandSideExpression @= AssignmentExpression

/* 
 * 对LeftHandSideExpression、AssignmentExpression执行@操作，结果为r
 * 设置LeftHandSideExpression值为r，并返回r
 */
```

示例

```javascript
let a = 32
a *= 2 // 64

let a = 32
a /= 2 // 16

let a = 32
a %= 10 // 2

let a = 32
a += 2 // 34

let a = 32
a -= 2 // 30

let a = 32
a <<= 2 // 128

let a = 32
a >>= 2 // 8

let a = 32
a >>>= 2 // 8

let a = 32
a &= 0 // 0

let a = 32
a ^= 0 // 32

let a = 32
a |= 0 // 32
```

## 逗号表达式

```
Expression :
  Expression , AssignmentExpression
```

### 算法

```javascript
Expression : Expression , AssignmentExpression

/* 
 * 依次运算Expression、AssignmentExpression，并返回AssignmentExpression的值
 */
```

示例

```javascript
1, true // true
```
