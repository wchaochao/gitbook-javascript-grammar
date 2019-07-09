# 关系表达式

标签（空格分隔）： JavaScript语法

---

## 比较算法

```javascript
comparison(x, y)

/**
 * 将x, y转换为原始值类型
 * x, y都为String类型
 *     逐一比较x, y的字符
 * x, y不都为String类型，转换为Number类型
 *     有一值为NaN，返回false
 *     其他情况比较大小
 */
```

## 小于

```javascript
RelationalExpression : RelationalExpression < ShiftExpression

/**
 * 执行比较算法comparison(RelationalExpression, ShiftExpression)
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

## 大于

```javascript
RelationalExpression : RelationalExpression > ShiftExpression

/**
 * 执行比较算法comparison(RelationalExpression, ShiftExpression)
 */
```

示例

```javascript
'123' > '123' // false
'123a' > '123' // true
'123' > '123a' // false
'abcd' > 'abd' // false
'abfd' > 'abd' // true

undefined > null // false
null > false // false
+0 > -0 // false
-Infinity > 1 // false
Infinity > 1 // true
1 > Infinity // false
1 > -Infinity // true
1 > 2 // false
2 > 1 // true
```

## 小于等于

```javascript
RelationalExpression : RelationalExpression <= ShiftExpression

/**
 * 执行比较算法comparison(RelationalExpression, ShiftExpression)
 */
```

示例

```javascript
'123' <= '123' // true
'123a' <= '123' // false
'123' <= '123a' // true
'abcd' <= 'abd' // true
'abfd' <= 'abd' // false

undefined <= null // false
null <= false // true
+0 <= -0 // true
-Infinity <= 1 // true
Infinity <= 1 // false
1 <= Infinity // true
1 <= -Infinity // false
1 <= 2 // true
2 <= 1 // false
```

## 大于等于

```javascript
RelationalExpression : RelationalExpression >= ShiftExpression

/**
 * 执行比较算法comparison(RelationalExpression, ShiftExpression)
 */
```

示例

```javascript
'123' >= '123' // true
'123a' >= '123' // true
'123' >= '123a' // false
'abcd' >= 'abd' // false
'abfd' >= 'abd' // true

undefined >= null // false
null >= false // true
+0 >= -0 // true
-Infinity >= 1 // false
Infinity >= 1 // true
1 >= Infinity // false
1 >= -Infinity // true
1 >= 2 // false
2 >= 1 // true
```

## instanceof

```javascript
RelationalExpression : RelationalExpression instanceof ShiftExpression

/**
 * ShiftExpression不为函数抛出TypeError
 * RelationalExpression不为对象返回false
 * ShiftExpression为函数，RelationalExpression为对象
 *     ShiftExpression在RelationalExpression的原型链上，返回true
 *     ShiftExpression不在RelationalExpression的原型链上，返回false
 */
```

示例

```javascript
1 instanceof 2 // TypeError
1 instanceof ({}) // TypeError
1 instanceof Number // false
let a = new Number(1)
a instanceof Number // true
a instanceof Object // true
```

## in

```javascript
RelationalExpression : RelationalExpression in ShiftExpression

/**
 * ShiftExpression不为Object时抛出TypeError
 * RelationalExpression转换为String类型
 * ShiftExpression为Object
 *     RelationalExpression是ShiftExpression本身及其原型链上的属性，返回true
 *     RelationalExpression不是ShiftExpression本身及其原型链上的属性，返回false
 */
```

示例

```javascript
1 in 1 // TypeError
'a' in ({a: 1}) // true
'toString' in ({a: 1}) // true
1 in [,1,] // true
0 in [,1,] // false
```
