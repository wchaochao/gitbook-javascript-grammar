# 类型转换

标签（空格分隔）： JavaScript语法

---

系统运行时会在需要的时候对语言类型进行自动类型转换

## ToPrimitive(input, PreferredType)

转换为原始值类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | input |
| Null | input |
| Boolean | input |
| Number | input |
| String | input |
| Object | input.[[DefaultValue]] (preferredType) |

对象转换为原始值类型，优先Number类型

* 先获取valueOf方法返回的原始值，没有就再获取toString方法返回的原始值，还没有就抛出TypeError

对象转换为原始值类型，优先String类型

* 先获取toString方法返回的原始值，没有就再获取valueOf方法返回的原始值，还没有就抛出TypeError

对象转换为原始值类型，无优先类型

* Date对象优先String类型
* 其他对象优先Number类型

示例

```javascript
let o = {
  toString () { return 'o' },
  valueOf () { return 1 }
}

ToPrimitive(o, 'String') // 'o'
ToPrimitive(o, 'Number') // 1
```

## ToBoolean(input)

转换为Boolean类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | false |
| Null | false |
| Boolean | input |
| Number | NaN、+0、-0转换为false，其他转换为true |
| String | 空字符串转换为false，其他转换为true |
| Object | true |

示例

```javascript
ToBoolean(undefined) // false
ToBoolean(null) // false
ToBoolean(NaN) // false
ToBoolean(0) // false
ToBoolean('') // false
ToBoolean({}) // true
```

## ToNumber(input)

转换为Number类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | NaN |
| Null | +0 |
| Boolean | true转换为1，false转换为+0 |
| Number | input |
| String | 字符串符合数字字符串文法，转换为对应数字，否则转换为NaN |
| Object | ToNumber(ToPrimitive(input, 'Number')) |

字符串转换为Number类型

* 忽略前置、后置空白
* 空字符串转换为0
* 十进制数字字符串转换为相应的十进制数字，忽略前导零（无穷大也是）
* 十六进制、二进制整数字符串转换为相应的十进制整数
* 转换的数字有超过20位有效数字时，将20位后的数字置为0, 20位加或不加一

对象转换为Number类型

* 对象转换为原始值类型，优先Number类型
* 将原始值转换为Number类型

示例

```javascript
ToNumber(undefined) // NaN
ToNumber(null) // +0
ToNumber(true) // 1
ToNumber(false) // +0
ToNumber(' \n ') // 0
ToNumber(' 0123.45 ') // 123.45
ToNumber(' 0x12 ') // 18
ToNumber({}) // NaN
ToNumber([1]) // 1
```

## ToInteger(input)

转换为整数

```javascript
/*
 * input转换为数字number
 *     number为NaN，返回+0
 *     number为Infinity、-Infinity、+0、-0，返回number
 *     number为其他，返回 sign(number) * floor(abs(number))
 */
let number = ToNumber(input)
if (number !== number) {
  return +0
} else if (abs(number) === Infinity || abs(number) === 0) {
  return number
} else {
  return sign(number) * floor(abs(number))
}
```

示例

```javascript
ToInteger(NaN) // +0
ToInteger(Infinity) // Infinity
ToInteger(-Infinity) // -Infinity
ToInteger(+0) // +0
ToInteger(-0) // +0
ToInteger(15.6) // 15
ToInteger(-15.6) // -15
```

## ToUInt32(input)

转换为32位无符号整数([0, 2^32 - 1])

```javascript
/*
 * 转换为数字number
 *     number为NaN、Infinity、-Infinity、+0、-0，返回+0
 *     number为其他，转换为整数，执行modulo 2^32
 */
let number = ToNumber(input)
if (number !== number || abs(number) === Infinity || abs(number) === 0) {
  return +0
} else {
  let posInt = sign(number) * floor(abs(number))
  let int32bit = posInt modulo Math.power(2, 32) // x modulo y = k, k与y同号，abs(k) < abs(y), x - k = q * y, q为整数
  return int32bit
}
```

示例

```javascript
ToUInt32(NaN) // +0
ToUInt32(Infinity) // +0
ToUInt32(-Infinity) // +0
ToUInt32(+0) // +0
ToUInt32(-0) // +0
ToUInt32(15) // 15
ToUInt32(-15) // 2^32 - 15
ToUInt32(Math.pow(2, 32) + 15) // 15
```

## ToInt32(input)

转换为32位有符号整数([-2^31, 2^31-1])

```javascript
/*
 * 转换为数字number
 *     number为NaN、Infinity、-Infinity、+0、-0，返回+0
 *     number为其他，转换为整数，执行modulo 2^32
 *         int32bit在2^31之内，直接返回
 *         int32bit在2^31之外，减去2^32变为负数
 */
let number = ToNumber(input)
if (number !== number || abs(number) === Infinity || abs(number) === 0) {
  return +0
} else {
  let posInt = sign(number) * floor(abs(number))
  let int32bit = posInt modulo Math.power(2, 32) // x modulo y = k, k与y同号，abs(k) < abs(y), x - k = q * y, q为整数
  if (int32bit >= Math.power(2, 31)) {
    int32bit -= Math.power(2, 32)
  }
  return int32bit
}
```

示例

```javascript
ToInt32(NaN) // +0
ToInt32(Infinity) // +0
ToInt32(-Infinity) // +0
ToInt32(+0) // +0
ToInt32(-0) // +0
ToInt32(15) // 15
ToInt32(-15) // -15
ToInt32(Math.pow(2, 31) + 1) // -2^31 + 1
ToInt32(-Math.pow(2, 31) - 1) // 2^31 - 1
```

## ToUInt16(input)

转换为16位无符号整数([0, 2^16 - 1])

```javascript
/*
 * 转换为数字number
 *     number为NaN、Infinity、-Infinity、+0、-0，返回+0
 *     number为其他，转换为整数，执行modulo 2^16
 */
let number = ToNumber(input)
if (number !== number || abs(number) === Infinity || abs(number) === 0) {
  return +0
} else {
  let posInt = sign(number) * floor(abs(number))
  let int16bit = posInt modulo Math.power(2, 16)  // x modulo y = k, k与y同号，abs(k) < abs(y), x - k = q * y, q为整数
  return int16bit
}
```

示例

```javascript
ToUInt16(NaN) // +0
ToUInt16(Infinity) // +0
ToUInt16(-Infinity) // +0
ToUInt16(+0) // +0
ToUInt16(-0) // +0
ToUInt16(15) // 15
ToUInt16(-15) // 2^16 - 15
ToUInt16(Math.pow(2, 16) + 15) // 15
```

## ToString(input)

转换为String类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | `'undefined'` |
| Null | `'null'` |
| Boolean | true转换为`'true'`，false转换为`'false'` |
| Number | 按下面的规则将数字 m 转换为字符串|
| String | input |
| Object | ToString(ToPrimitive(input, 'String')) |

数字 m 转换为字符串

* NaN转换为`'NaN'`
* +0、-0转换为`'0'`
* Infinity转换`'Infinity'`
* 负数转换为`-ToString(-m)`
* 正数转换分两种情况
 * 小数点前的数字超过20位或小数点后紧跟的0超过5位，转换为`'科学技术法表示的m'`
 * 其他情况转换为`'十进制表示的m'`

对象转换为String类型

* 对象转换为原始值类型，优先String类型
* 将原始值转换为String类型

示例

```javascript
ToString(undefined) // 'undefined'
ToString(null) // 'null'
ToString(true) // 'true'
ToString(false) // 'false'
ToString(NaN) // 'NaN'
ToString(+0) // '0'
ToString(-0) // '0'
ToString(Infinity) // 'Infinity'
ToString(-Infinity) // '-Infinity'
ToString(12.345) // '12.345'
ToString(Math.pow(10, 21) + 12.34) // '1e+21'
ToString(0.0000001) // '1e-7'
ToString({}) // '[object, Object]'
```

## ToObject(input)

转换为Object类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | 抛出TypeError |
| Null | 抛出TypeError |
| Boolean | new Boolean(input) |
| Number | new Number(input) |
| String | new String(input) |
| Object | input |

示例

```javascript
ToObject(undefined) // 抛出TypeError
ToObject(null) // 抛出TypeError
ToObject(true) // Boolean{true}
ToObject(1) // Number{1}
ToObject('a') // String{'a'}
let o = {}
ToObject(o) // {}
```

## CheckObjectCoercible(input)

检测能否转换为Object类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | 抛出TypeError |
| Null | 抛出TypeError |
| Boolean | 直接返回 |
| Number | 直接返回 |
| String | 直接返回 |
| Object | 直接返回 |

示例

```javascript
CheckObjectCoercible(undefined) // 抛出TypeError
CheckObjectCoercible(null) // 抛出TypeError
CheckObjectCoercible(true) // 直接返回
CheckObjectCoercible(1) // 直接返回
CheckObjectCoercible('a') // 直接返回
CheckObjectCoercible({}) // 直接返回
```

## IsCallable(input)

是否是可调用的对象

| 输入类型 | 结果 |
| --- | --- |
| Undefined | false |
| Null | false |
| Boolean | false |
| Number | false |
| String | false |
| Object | 有[[Call]]方法，返回true，否则返回false |

示例

```javascript
IsCallable(undefined) // false
IsCallable(null) // false
IsCallable(true) // false
IsCallable(1) // false
IsCallable('a') // false
IsCallable({}) // false
IsCallable(function foo () {}) // true
```

## SameValue(x, y)

比较两个值是否相同

```javascript
/*
 * 类型不同，值不同
 * 类型相同
 *     类型为Undefined或Null, 值相同
 *     类型为Boolean, x、y同为true或false时相同
 *     类型为String, x、y为相同字符串时相同
 *     类型为Object, x、y为同一个对象时相同
 *     类型为Number
 *         x、y同为NaN时值相同
 *         x、y为同一值且不是一个+0一个-0时值相同
 */
if (Type(x) !== Type(y)) {
  return false
}

if (Type(x) === Undefined || Type(y) === Null) {
  return true
}

if (Type(x) === Boolean || Type(x) === String || Type(x) === Object) {
  return x === y
}

if (Type(x) === Number) {
  if (x === y) {
    return 1/x === 1/y // +0与-0不相同
  } else {
    return x !== x && y !== y // NaN与NaN相同
  }
}
```

示例

```javascript
SameValue(1, '1') // false
SameValue(undefined, undefined) // true
SameValue(null, null) // true
SameValue(true, true) // true
SameValue('1', '\x31') // true
SameValue({}, {}) // false
let a = {}
let b = a
SameValue(a, b) // true
SameValue(NaN, NaN) // true
SameValue(+0, -0) // false
SameValue(1, 0x1) // true
```
