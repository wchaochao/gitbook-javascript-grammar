# 相等表达式

标签（空格分隔）： JavaScript语法

---

## 相等比较

```javascript
equality(x, y)
equality(y, x)

/*
 * 两值类型相同
 *     NaN != NaN
 *     +0 == -0
 *     其他情况比较值是否相同
 * 两值类型不同
 *     undefined == null，undefined, null与其他值进行相等比较时返回false
 *     对象转换为Primitive类型，最终全转换为Number类型进行比较
 */
```

示例

```javascript
// 类型相同
equality(NaN, NaN) // false
equality(+0, -0) // true
equality(undefined, undefined) // true
equality(null, null) // true
equality(true, true) // true
equality('1', '\x31') // true
equality(({}, {}) // false
equality(1, 0x1) // true

// 类型不同
equality(undefined, null) // true
equality(undefined, 1) // false
equality(1, '1') // true
equality('1', true) // true
equality(true, new Number(1)) // true
```

## 相等

```javascript
EqualityExpression : EqualityExpression == RelationalExpression

/**
 * 执行相等比较算法equality(EqualityExpression, RelationalExpression)
 */
```

示例

```javascript
// 类型相同
NaN == NaN // false
+0 == -0 // true
undefined == undefined // true
null == null // true
true == true // true
'1' == '\x31' // true
({}) == ({}) // false
1 == 0x1 // true

// 类型不同
undefined == null // true
undefined == 1 // false
1 == '1' // true
'1' == true // true
true == new Number(1) // true
```

## 不相等

```javascript
EqualityExpression : EqualityExpression != RelationalExpression

/**
 * 执行相等比较算法equality(EqualityExpression, RelationalExpression)
 */
```

示例

```javascript
// 类型相同
NaN != NaN // true
+0 != -0 // false
undefined != undefined // false
null != null // false
true != true // false
'1' != '\x31' // false
({}) != ({}) // true
1 != 0x1 // false

// 类型不同
undefined != null // false
undefined != 1 // true
1 != '1' // false
'1' != true // false
true != new Number(1) // false
```

## 严格相等比较

```javascript
strictEquality(x, y)
strictEquality(y, x)

/*
 * 两值类型不同，返回false
 * 两值类型相同
 *     NaN != NaN
 *     +0 == -0
 *     其他情况比较值是否相同
 */
```

示例

```javascript
strictEquality(undefined, null) // false
strictEquality(NaN, NaN) // false
strictEquality(+0, -0) // true
strictEquality(undefined, undefined) // true
strictEquality(null, null) // true
strictEquality(true, true) // true
strictEquality('1', '\x31') // true
strictEquality(({}, {}) // false
strictEquality(1, 0x1) // true
```

## 严格相等

```javascript
EqualityExpression : EqualityExpression === RelationalExpression

/**
 * 执行严格相等比较算法equality(EqualityExpression, RelationalExpression)
 */
```

示例

```javascript
undefined === null // false
NaN === NaN // false
+0 === -0 // true
undefined === undefined // true
null === null // true
true === true // true
'1' === '\x31' // true
({}) === ({}) // false
1 === 0x1 // true
```

## 严格不相等

```javascript
EqualityExpression : EqualityExpression !== RelationalExpression

/**
 * 执行严格相等比较算法equality(EqualityExpression, RelationalExpression)
 */
```

示例

```javascript
undefined !== null // true
NaN !== NaN // true
+0 !== -0 // false
undefined !== undefined // false
null !== null // false
true !== true // false
'1' !== '\x31' // false
({}) !== ({}) // true
1 !== 0x1 // false
```
