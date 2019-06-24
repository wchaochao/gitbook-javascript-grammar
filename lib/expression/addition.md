# 加法表达式

标签（空格分隔）： JavaScript语法

---

## 加法

```javascript
AdditiveExpression : AdditiveExpression + MultiplicativeExpression

/*
 * AdditiveExpression、MultiplicativeExpression转换为Primitive类型
 * 有一值为String类型
 *     都转换为String类型，执行字符串拼接
 * 都不为String类型
 *     都转换为Number类型，执行数字加法
 */
```

示例

```javascript
// 字符串拼接
'1' + 1 // '11'
({}) + 1 // '[object Object]1'
[1] + 1 // '11'
(function () {}) + 1 // 'function () {}1'
new String('1') + 1 // '11'
new Date() + 1 // 'Thu May 09 2019 16:15:27 GMT+0800 (中国标准时间)1'
/a/ + 1 // '/a/1'

// 数字加法
1 + undefined // NaN
1 + null // 1
1 + true // 2
1 + 1 // 2
1 + new Boolean(true) // 2
1 + new Number(1) // 2
```

## 数字加法

```javascript
additive(oneValue, anotherValue)
additive(anotherValue, oneValue)

/*
 * 有一值为NaN，返回NaN
 * 有一值为无穷大
 *     另一值为无穷大
 *         两值符号不同，返回NaN
 *         两值符号相同，返回无穷大，符号相同
 *     另一值不为无穷大，返回无穷大
 * 有一值为0
 *     另一值为0
 *         都为-0，返回-0
 *         不都为-0，返回+0
 *     另一值不为0，返回另一值
 * 两值为相反数，返回+0
 * 其他情况进行加法运算
 */
```

示例

```javascript
NaN + 1 // NaN
Infinity + (-Infinity) // NaN
Infinity + Infinity // Infinity
-Infinity + (-Infinity) // -Infinity
Infinity + 1 // Infinity
-Infinity + 1 // -Infinity
-0 + (-0) // -0
+0 + (+0) // +0
+0 + (-0) // +0
+0 + 2 // 2
2 + (-2) // +0
1 + 1 // 2
```

## 减法

```javascript
AdditiveExpression : AdditiveExpression - MultiplicativeExpression

/*
 * AdditiveExpression、MultiplicativeExpression转换为Number类型
 * a - b等价于a + (-b)
 */
```

示例

```javascript
NaN - 1 // NaN
Infinity - Infinity // NaN
Infinity - (-Infinity) // Infinity
-Infinity - Infinity // -Infinity
Infinity - 1 // Infinity
-Infinity - 1 // -Infinity
-0 - (+0) // -0
+0 - (-0) // +0
+0 - (+0) // +0
+0 - 2 // -2
2 - 2 // +0
2 - 1 // 1
```
