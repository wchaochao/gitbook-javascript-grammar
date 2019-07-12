# 数组

标签（空格分隔）： JavaScript语法

---

## 定义

按次序排列的一组值

```javascript
// 直接赋值
let arr = ['a', 'b', 'c']

// 先定义再赋值
let arr = [];
arr[0] = 'a';
arr[1] = 'b';
arr[2] = 'c';

// 多维数组
let a = [[1, 2], [3, 4]]
```

## 索引

[0, 2^32 - 1)之内的整数，可以使用`in`判断索引属性是否存在

```javascript
let arr = [];
arr[100] = 'a'

100 in arr // true
1 in arr // false
```

## 元素

索引属性存在时对应的索引值，可为任意类型

```javascript
let arr = []
arr[0] = undefined
arr[1] = [1, 2]
arr[2] = function () {}
```

### 修改元素

修改length内的索引值

```javascript
let arr = [1, 2, 3]
arr[1] = 0 // arr为[1, 0, 3]
```

### 添加元素

添加length外的索引值，length自动为最大索引加1

```javascript
let arr = [1, 2, 3]
arr[9] = 0 // arr为[1, 2, 3, empty × 6, 0]
arr.length // 10
```

## 空位

索引属性不存在时，称为空位

```javascript
// 数组直接量
let arr = [1,,3]
1 in arr // false

// 添加元素
let arr = [1, 2, 3]
arr[10] = 10
5 in arr // false

// 增加长度
let arr = [1, 2, 3]
arr.length = 10
5 in arr // false

// 删除索引属性
let arr = [1, 2, 3]
delete arr[1]
1 in arr // false
```

forEach等遍历方法、reduce等累计方法、for/in、Object.keys过滤空位

```javascript
let a = [, , ,]

a.forEach(function (x, i) {
  console.log(i + '. ' + x)
})
// 不产生任何输出

for (var i in a) {
  console.log(i)
}
// 不产生任何输出

Object.keys(a) // []
```

## 长度

UInt32类型，比任意索引都要大

### 非法长度

抛出RangError

```javascript
let arr = [1, 2, 3]

arr.length = -1 // 抛出RangeError
arr.length = Math.pow(2, 32) // 抛出RangeError
arr.length = 2.3 // 抛出RangeError
arr.length = 'abc' // 抛出RangeError
```

### 增加长度

length属性变化

```javascript
let arr = [1, 2, 3]
arr.length = 10 // arr为[1, 2, 3, empty × 7]
```

### 减少长度

length属性变化，并自动删除大于length - 1的索引属性

```javascript
let arr = [1, 2, 3]
arr.length = 2 // arr为[1, 2]
```

## 属性

索引属性外的普通属性

```javascript
let arr = []
arr[-1] = 'a'
arr[Math.pow(2, 32)] = 'b'

arr.length // 0
arr[-1] // 'a'
arr[4294967296] // 'b'
```

## 本质

一种特殊的对象，可以添加索引外的其它属性

```javascript
let arr = [1, 2, 3]

// 类型
typeof arr // object

// 属性
Object.keys(arr) // ['0', '1', '2']

// 访问
arr[1] // 2

// 添加其它属性
arr.p = 'a'
```

## 遍历

for循环

```javascript
let a = [1, 2, 3]

for(var i = 0; i < a.length; i++) {
  console.log(a[i])
}
```

while循环

```javascript
let a = [1, 2, 3]

let i = 0
while (i < a.length) {
  console.log(a[i])
  i++
}

let l = a.length
while (l--) {
  console.log(a[l])
}
```

for/in循环可能会遍历索引外的其它添加的属性

```javascript
let a = [1, 2, 3]
a.foo = true

for (let key in a) {
  console.log(key)
}
// 0
// 1
// 2
// foo
```

## 类数组对象

* 有length属性的对象
* length属性不会动态变化

```javascript
let obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}

obj[3] = 'd'
obj.length // 3
```

### 常见类数组对象

* 字符串
* arguments对象
* DOM元素集

转变为数组

```javascript
// 使用slice方法
let arr = Array.prototype.slice.call(arrayLike)
```

调用数组方法

```javascript
// 通过call, apply调用数组方法
Array.prototype.forEach.call('abc', function (chr) {
  console.log(chr);
});
// 'a'
// 'b'
// 'c'
```
