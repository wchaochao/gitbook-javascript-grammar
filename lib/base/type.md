# 数据类型

标签（空格分隔）： JavaScript语法

---

## Undefined类型

未定义

### 类型值

```
undefined
```

示例

```javascript
// 变量被声明但未赋值
var i; // undefined

// 不存在的对象属性
var o = {};
console.log(o.p) // undefined

// 没有实参传入的形参
function f(x) {
  console.log(x);
}
f(); // undefined

// 函数无返回值, 默认返回undefined
function f() {}
var a = f(); // undefined
```

## Null类型

空值

### 类型值

```
null
```

## Boolean类型

布尔值

### 类型值

```
true
false
```

## Number类型

数值

### 类型值

IEEE-754 格式 64 位双精度数值

#### 组成

1位数符 + 11位阶码 + 52位尾数

* 数符S：表示正负，0为正，1为负
* 阶码E：指数部分，阶码 = 阶码真值 + 偏移量1023
* 尾数：小数部分

示例

```
十进制 -125.125
转换为二进制表示：-1111101.001
科学计数法表示：-1.111101001 * 2^6

数符S：1
阶码E：000 00000110 (阶码真值) + 011 11111111 (偏移量) = 1  00 00000101
尾数M：11110100 10000000 00000000 00000000 00000000 00000000 0000
最终值：1 + 100 00000101 + 11110100 10000000 00000000 00000000 00000000 00000000 0000
```

#### 表示值

```
V = (-1)^S*2^(E-1023)*1.M

规格化
S  +  (E!=0 && E!=2047) + 1.M
  E为2046且M全为1时为能表示的最大的数，用Number.MAX_VALUE表示，大于该数时为无穷大
  E为1023 ~ 1023 + 52时为整数，范围为[Number.MIN_SAFE_INTEGER, Number.MAX_SAFE_INTEGER]
  M为52位，总共53为有效数字，最小精度为Number.EPSILON

非规格化
S  +  (E=0) + M
  M全为0时表示0
     S为正，表示+0
     S为负，表示-0
  M不全为0时表示接近0的数
     M为1时为能表示的最接近0的数，用Number.MIN_VALUE表示，小于该数时为0
S  +  (E=2047) + (M全为0)     
  无穷大
     S为正，表示Infinity
     S为负，表示-Infinity
S  +  (E=2047) + (M不全为0)
  NaN
```

精确数字 x 对应Number类型值

* 选择Number类型值中最接近x的值
 * 只有一个，就是该值
 * 有两个，选择M为偶数的
 * 选择值为2^1024时，为正无穷
 * 选择值为-2^1024时，为负无穷
 * 选择值为+0且x大于0时，为正零
 * 选择值为+0且x小于0时，为负零

可能会存在误差

```javascript
0.1 + 0.2 === 0.3 // false

0.3 / 0.1 // 2.9999999999999996

(0.3 - 0.2) === (0.2 - 0.1) // false
```

### 字面量

整数

* 十进制：`([+-])?([1-9]\d*|0)`
* 二进制：`([+-])?0[bB][01]+`
* 八进制：`([+-])?0[0-7]+`, 严格模式禁止使用八进制
* 十六进制：`([+-])?0[xX][0-9A-Fa-f]+`

小数

* 小数：`([+-])?([1-9]\d*\.\d*|0\.\d*|\.\d+)`

特殊数

* `+0`：正零，运算时相当于`0+`
* `-0`：负零，运算时相当于`0-`
* `Infinity`：正无穷大，运算时相当于`+∞`
* `-Infinity`：负无穷大，运算时相当于`-∞`
* `NaN`：非数字，且`NaN!==NaN`

科学计数法

```javascript
100.23e3
.23e+3
100e-3
```

## String类型

字符串

### 类型值

16 位无符号整数值的有序序列

* 每个元素占有一个序列位置
* 元素的个数为字符串长度

Unicode字符转换为String类型值

* 使用`UTF-16`编码
 * 单字节字符，无影响
 * 多字节字符，需要用两个元素来表示

### 字面量

"非双引号字符"或'非单引号字符'

* 普通字符：字母、数字、符号、中文等
* 转义字符
  * `\0`: `NUL`字符
  * `\r`: 回车符
  * `\n`: 换行符
  * `\t`: 水平制表符
  * `\v`: 垂直制表符
  * `\b`: 退格符
  * `\f`: 换页符
  * `\'`: 单引号
  * `\"`: 双引号
  * `\\`: 反斜杠
  * `\三位八进制`: 对应的ASCII码
  * `\x两位十六进制`: 对应的ASCII码
  * `\u四位十六进制`: 对应的Unicode字符
  * `\其他`: 忽略\
 * 行延续符：\行结束符

示例

```javascript
// 普通字符串
'a'

// 转义序列
'\''
'\a'
'\0a'
'\x1a'
'\u001a'

// 行延续符
'ab\
c'
```

## Object类型

对象

### 类型值

各类对象

* 内置对象：ECMAScript里内置的对象，如`Object, Function, Array, Number, String, Boolean, Date, RegExp, Error`等构造函数，`Global, Math, JSON`等单体对象
* 原生对象：通过ECMAScript创建的对象
* 宿主对象：由宿主环境提供的对象，用于完善执行环境，如浏览器提供的`window, document`等对象

### 转换为引用类型

强制转换

* `Number`：`Object(num)`或`new Number(num)`
* `String`：`Object(str)`或`new String(str)`
* `Boolean`：`Object(boo)`或`new Boolean(boo)`
* `Object`：`Object(obj)`

自动转换

* `Undefined, Null`类型自动转换为`Object`类型时会抛出`TypeError`
* `Number, String, Boolean`自动转换为对应的包装对象

## 区分数据类型

使用`typeof`运算符检测原始值类型

* 返回`"undefined"`：值未定义
* 返回`"number"`：值为数字
* 返回`"string"`：值为字符串
* 返回`"boolean"`：值为布尔值
* 返回`"function"`：值为函数
* 返回`"object"`：值为对象或`null`

使用`Object.prototype.toString.call()`方法检测引用类型

```javascript
Undefined类型               "[object Undefined]"
Null类型                    "[object Null]"
Number类型或Number对象      "[object Number]"
Booelan类型或Boolean对象    "[object Boolean]"
String类型或String对象      "[object String]"
Object对象                  "[object Object]"
Array对象                   "[object Array]"
Function对象                "[object Function]"
RegExp对象                  "[object RegExp]"
Date对象                    "[object Date]"
Error对象                   "[object Error]"
Math对象                    "[object Math]"
JSON对象                    "[object JSON]"
Arguments对象               "[object Arguments]"
```

使用`instanceof`运算符检测原型链

```
语法：obj instanceof constructor
解释：判断对象是否是某个构造函数的实例
参数：obj - 对象，原始值类型返回false
     constructor - 构造函数，不为函数抛出TypeError
返回值：constructor.prototype在对象obj的原型链上，返回true
       constructor.prototype不在对象obj的原型链上，返回false
```
