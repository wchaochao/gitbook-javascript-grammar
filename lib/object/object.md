# 对象

标签（空格分隔）： JavaScript语法

---

## 属性

key/value对

### 组成

* 属性名：标识符、关键字、保留字、String字面量、Number字面量都可为属性名
* 属性值：可为任意类型，为函数时称为方法

### 分类

* 自身属性
* 继承属性
* 可枚举属性

## 属性描述符

属性的描述信息

### 数据属性

属性值由`[[value]]`决定

| 属性描述 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| [[Value]] | any | undefined | 属性值 |
| [[Writable]] | Boolean | false | 属性值是否可写 |
| [[Enumerable]] | Boolean | false | 属性是否可枚举，`for/in, Object.keys, JSON.stringify`中遍历可枚举属性 |
| [[Configurable]] | Boolean | false | 属性是否可配置，为false时属性不可删除、其他属性描述一般不能修改 |

```javascript
let obj = {}

obj.defineProperty('a', {
  value: 1,
  writable: true,
  enumberable: true,
  configurable: true
})
```

### 访问器属性

属性值由`get/set`函数决定

| 属性描述 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| [[Get]] | Function、Undefined | undefined | 获取属性时执行的函数，参数为空，返回值为属性值 |
| [[Set]] | Function、Undefined | undefined | 设置属性时执行的函数，参数为设置的值 |
| [[Enumerable]] | Boolean | false | 属性是否可枚举，`for/in, Object.keys, JSON.stringify`中遍历可枚举属性 |
| [[Configurable]] | Boolean | false | 属性是否可配置，为false时属性不可删除、其他属性描述一般不能修改 |

```javascript
let obj = {
  _a: 1
}

obj.defineProperty('a', {
  get: function () {return this._a},
  set: function (val) {this._a = val},
  enumberable: true,
  configurable: true
})
```

### 默认描述

* 内置对象的内置属性的`writable、enumerable、configurable`都为`false`
* var声明的变量的`writable、enumerable`为`true`，`configurable`为`false`
* 直接添加的属性的`writable、enumerable、configurable`都为`true`

## 通用内部属性

所有对象都有的内部属性

| 内部属性 | 类型 | 描述 |
| --- | --- | --- |
| [[Class]] | String | 对象种类 |
| [[Prototype]] | Object \| Null | 对象的原型<br/>原型的数据属性会被继承，用于[[Get]]<br/>原型的访问器属性会被继承，用于[[Get]]和[[Put]] |
| [[Extensible]] | Boolean | 对象是否可扩展，为false时不可添加属性、不可修改[[Class]]和[[Prototype]] |
| [[GetOwnProperty]] | (propertyName: String) => Property Descriptor \| Undefined | 获取自身属性的属性描述符 |
| [[DefineOwnProperty]] | (propertyName: String, value: Property Descriptor, flag: Boolean) | 设置自身属性的属性描述符, flag控制错误处理 |
| [[GetProperty]] | (propertyName: String) => Property Descriptor \| Undefined | 获取自身属性或继承属性的属性描述符 |
| [[HasProperty]] | (propertyName: String) => Boolean | 是否是自身属性或继承属性 |
| [[Get]] | (propertyName: String) => any | 获取自身属性或继承属性的属性值 |
| [[CanPut]] | (propertyName: String) => Boolean | 能否设置属性 |
| [[Put]] | (propertyName: String, value: any, flag: Boolean) | 设置属性, flag控制错误处理 |
| [[Delete]] | (propertyName: String, flag: Boolean) => Boolean | 删除自身属性, flag控制错误处理 |
| [[DefaultValue]] | (Hint: String) => primitive | 返回对象的默认值 |

### [[GetOwnProperty]] (P)

获取自身属性的属性描述符

```javascript
/*
 * 非自身属性，返回undefined
 * 自身属性，获取自身属性的属性描述符
 */
```

示例

```javascript
let o = {
  a: 1,
  get p () { return 2 }
}

// 非自身属性
o.[[GetOwnProperty]]('b') // undefined

// 数据属性
o.[[GetOwnProperty]]('a')
Property Descriptor {
  [[Value]]: 1,
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
}

// 访问器属性
o.[[GetOwnProperty]]('p')
Property Descriptor {
  [[Get]]: getter,
  [[Set]]: undefined,
  [[Enumerable]]: true,
  [[Configurable]]: true
}
```

### [[DefineOwnProperty]] (P, Desc, Throw)

设置自身属性的属性描述符

```javascript
/*
 * 非自身属性，添加属性
 *     对象不可扩展时，不能添加属性
 * 自身属性，修改属性的属性描述符
 *    自身属性的Configurable为false时
 *        Configurable、Enumberable不能修改
 *        不能由一种属性描述符改为另一种属性描述符
 *        数据属性描述符Writable为false时, Writable和Value都不能修改
 *        访问器属性描述符的Get和Set不能修改
 */
```

示例

```javascript
let o = {
  a: 1
}

Object.defineProperty('p', {
  get () { return 1 },
  enumerable: true,
  configurable: false
})

// 添加属性
// 可扩展
o.[[DefinedOwnProperty]]('b', {
  [[Value]]: 2
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
}, true)

// 不可扩展
Object.preventExtensions(o)
o.[[DefinedOwnProperty]]('c', { // 抛出TypeError
  [[Value]]: 2
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
}, true)

// 修改属性
// 可修改
o.[[DefineOwnProperty]]('a', {
  [[Value]]: 3
}, true)

// 不可修改
o.[[DefineOwnProperty]]('p', {
  [[Value]]: 3
}, true)
```

### [[GetProperty]] (P)

获取自身属性或继承属性的属性描述符

```javascript
/*
 * 获取自身属性的属性描述符
 *    存在，直接返回
 *    不存在，沿原型链递归，都不存在时返回undefined
 */
```

示例

```javascript
let o = {
  a: 1
}

// 属性不存在
o.[[GetProperty]]('b') // undefined

// 自身属性
o.[[GetProperty]]('a')
Property Descriptor {
  [[Value]]: 1
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
}

// 继承属性
o.[[GetProperty]]('toString')
Property Descriptor {
  [[Value]]: function toString () { [native code] }
  [[Writable]]: true,
  [[Enumerable]]: false,
  [[Configurable]]: true
}
```

### [[HasProperty]] (P)

是否是对象自身或继承的属性

```javascript
/*
 * 获取自身属性或继承属性的属性描述符
 *     存在，返回true
 *     不存在，返回false
 */
```

示例

```javascript
let o = {
  a: 1
}

// 属性不存在
o.[[HasProperty]]('b') // false

// 自身属性
o.[[HasProperty]]('a') // true

// 继承属性
o.[[HasProperty]]('toString') // true
```

### [[Get]] (P)

获取自身属性或继承属性的属性值

```javascript
/*
 * 获取自身属性或继承属性的属性描述符
 *     不存在，返回undefined
 *     存在，根据描述符类型返回属性值
 *         数据属性，返回[[Value]]
 *         访问器属性，调用[[Get]]方法
 */
```

示例

```javascript
let o = {
  a: 1,
  get p () { return this.a }
}

// 属性不存在
o.[[Get]]('b') // undefined

// 自身属性
o.[[Get]]('a') // 1
o.[[Get]]('p') // 1

// 继承属性
o.[[Get]]('toString') // function toString () { [native code] }
```

### [[CanPut]] (P)

是否能设置属性

```javascript
/*
 * 自身属性，返回是否可写
 * 继承属性，根据属性描述符及继承规则判断能否修改
 *     数据属性，返回是否可写且是否可扩展
 *     访问器属性，返回是否可写
 * 属性不存在，返回是否可扩展
 */
```

示例

```javascript
let o = {
  a: 1,
  get p () { return this.a }
}

// 属性不存在
o.[[CanPut]]('b') // o可扩展返回true，不可扩展返回false

// 自身属性
o.[[CanPut]]('a') // true
o.[[CanPut]]('p') // false

// 继承的数据属性属性
o.[[CanPut]]('toString') // o可扩展返回true，不可扩展返回false
```

### [[Put]] (P, V, Throw)

设置属性

```javascript
/*
 * 能否设置属性
 *   不能，根据Throw决定是否抛出错误
 *   能设置属性
 *      自身的数据属性，修改[[Value]]
 *      自身或继承的访问器属性，调用[[set]]方法
 *      继承的数据属性或新属性，添加属性
 */
```

示例

```javascript
let o = {
  a: 1,
  set p (val) { this.a = val}
}

// 属性不存在，添加属性
o.[[Put]]('b', 2, true) // {a: 1, b: 2, set p}

// 自身数据属性，修改属性
o.[[Put]]('a', 3, true) // {a: 3, set p}

// 访问器属性，调用set方法
o.[[Put]]('p', 3, true) // {a: 3, set p}

// 继承的数据属性，添加属性
o.[[Put]]('toString', 5, true) // {a: 1, toString: 5}
```

### [[Delete]] (P, Throw)

删除属性

```javascript
/*
 * 非自身属性，直接返回true
 * 自身属性，判断是否可删除
 *     可删除，删除属性并返回true
 *     不可删除
 *         Throw为true抛出TypeError
 *         Throw为false返回false
 */
```

示例

```javascript
let o = {
  a: 1
}
Object.defineProperty(o, 'b', {
  value: 2,
  configurable: false
})

// 非自身属性
o.[[Delete]]('c', true) // true

// 可删除的自身属性
o.[[Delete]]('a', true) // true

// 不可删除的自身属性
o.[[Delete]]('b', true) // TypeError
```

### [[DefaultValue]] (hint)

获取对象的默认值

```javascript
/*
 * hint为'String'时，先获取toString方法返回的原始值，没有就再获取valueOf方法返回的原始值，还没有就抛出TypeError
 * hint为'Number'时，先获取valueOf方法返回的原始值，没有就再获取toString方法返回的原始值，还没有就抛出TypeError
 * hint为空时，O为Date Object时，相当于hint为'String'
 *             O不为Date Object时，相当于hint为'Number'
 */
```

示例

```javascript
let o = {
  toString () {
    return 'o'
  },
  valueOf () {
    return 1
  }
}

// 传入参数
o.[[DefaultValue]]('String') // 'o'
o.[[DefaultValue]]('Number') // 1

// 不传参数
(new Date()).[[DefaultValue]]() // 'Mon Apr 29 2019 14:26:25 GMT+0800 (中国标准时间)'
o.[[DefaultValue]]() // 1
```

## 特定内部属性

某些特定对象的内部属性

| 内部属性 | 类型 | 描述 |
| --- | --- | --- |
| [[PrimitiveValue]] | primitive | 对象的原始值（Boolean, Number, String, Date对象） |
| [[FormalParameters]] | List of Strings | 函数的形参列表（Function对象） |
| [[Code]] | ECMAScript code | 函数的代码（Function对象） |
| [[Scope]] | Lexical Environment | 函数所在的词法环境（Function对象） |
| [[TargetFunction]] | Function | 通过Function.prototype.bind方法创建的函数的目标函数（Function对象） |
| [[BoundThis]] | any | 通过Function.prototype.bind方法创建的函数绑定的this值（Function对象） |
| [[BoundArguments]] | List of any | 通过Function.prototype.bind方法创建的函数绑定的参数值（Function对象） |
| [[Construct]] | (argList: List of any) => Object | 构建实例对象，通过new调用（Function对象） |
| [[Call]] | (thisArg: any, argList: List of any) => any \| Reference | 函数调用，通过()调用（Function对象） |
| [[HasInstance]] | (instance: any) => Boolean | 是否是构造器的实例（Function对象） |
| [[Match]] | (str: String, index: Number) => MatchResult | 正则匹配（RegExp对象） |
| [[ParameterMap]] | Object | 共享的形参（Arguments对象） |

## 属性访问

```javascript
MemberExpression : MemberExpression [ Expression ]
MemberExpression . IdentifierName 等价于 MemberExpression [ <identifier-name-string> ]
```

* []访问时可以使用表达式
* 点访问时要符合标识符命名规则

## 检测属性

```javascript
obj.hasOwnProperty(key) // 判断是否是对象本身的属性
obj.propertyIsEnumerable(key) // 判断是否是对象本身的可枚举属性
key in obj // 判断是否是对象本身及其原型链上的属性
```

## 查看属性

```javascript
Object.getOwnPropertyNames(obj) // 返回对象本身所有属性名组成的数组
Object.keys(obj) // 返回对象本身的所有可枚举属性名组成的数组
```

## 遍历属性

```javascript
for/in // 遍历对象及其原型链上所有可枚举的属性
```
