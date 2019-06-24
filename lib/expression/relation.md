# 关系表达式

标签（空格分隔）： JavaScript语法

---

## 算法

```javascript
/**
 * 比较x, y
 *     x小于y，返回true
 *     x不小于y，返回false
 *     x或y至少一值为NaN，返回undefined
 */
comparison(x, y)

/**
 * 将x, y转换为原始值类型
 * x, y都为String类型
 *     逐一比较x, y的字符
 * x, y不都为String类型，转换为Number类型
 *     有一值为NaN，返回undefined
 *     其他情况比较大小
 */
```

示例

```javascript
comparison('123', '123') // false
comparison('123a', '123') // false
comparison('123', '123a') // true
comparison('abcd', 'abd') // true
comparison('abfd', 'abd') // false

comparison(undefined, null) // undefined
comparison(null, false) // false
comparison(+0, -0) // false
comparison(-Infinity, 1) // true
comparison(Infinity, 1) // false
comparison(1, Infinity) // true
comparison(1, -Infinity) // false
comparison(1, 2) // true
comparison(2, 1) // false
```

## 小于

```javascript
RelationalExpression : RelationalExpression < ShiftExpression

/**
 * 执行比较算法comparison(RelationalExpression, ShiftExpression)
 *    结果为undefined或false, 返回false
 *    结果为true, 返回true
 */
```

示例

```javascript
'123' < '123' // false
'123a' < '123' // false
'123' < '123a' // true
'abcd' < 'abd' // true
'abfd' < 'abd' // false

undefined < null // false
null < false // false
+0 < -0 // false
-Infinity < 1 // true
Infinity < 1 // false
1 < Infinity // true
1 < -Infinity // false
1 < 2 // true
2 < 1 // false
```
