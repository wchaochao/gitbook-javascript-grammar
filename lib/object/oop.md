# 面向对象编程

标签（空格分隔）： JavaScript语法

---

## 构造函数

用于创建实例对象

* 任一函数都可以作为构造函数使用
* 约定使用大驼峰命名法命名

示例

```javascript
function Fubar(foo, bar) {
  if (!(this instanceof Fubar)) { // 处理不使用new的情况
    return new Fubar(foo, bar);
  }

  this._foo = foo;
  this._bar = bar;
}

Fubar(1, 2)._foo // 1
(new Fubar(1, 2))._foo // 1

// new.target判断是否使用了new
function f() {
  console.log(new.target === f);
}

f() // false
new f() // true
```

## 原型对象

用于包含实例对象共享的属性和方法

* 函数创建时会自动添加prototype属性，为实例对象的原型对象
* `构造函数.prototype = 原型对象`，`原型对象.constructor = 构造函数`

## 实例对象

* 通过构造函数创建的对象
* `实例对象.__proto__ = 原型对象`

创建过程

```javascript
function _new(/* 构造函数 */ constructor, /* 构造函数参数 */ params) {
  // 将 arguments 对象转为数组
  var args = [].slice.call(arguments);
  // 取出构造函数
  var constructor = args.shift();
  // 创建一个空对象，继承构造函数的 prototype 属性
  var context = Object.create(constructor.prototype);
  // 执行构造函数
  var result = constructor.apply(context, args);
  // 如果返回结果是对象，就直接返回，否则返回 context 对象
  return (typeof result === 'object' && result != null) ? result : context;
}

// 实例
var actor = _new(Person, '张三', 28);
```

## 原型链

实现继承

* 原型对象也是一个对象，也有自己的原型`__proto__`
  * 原型对象的原型默认为`Object.prototype`
  * `Object.prototype`的原型为`null`
* 原型对象、原型对象的原型...构成原型链
* 实例对象可以继承原型链上的属性
  * 查找对象属性时，先查找对象本身的，再沿着原型链查找

## 自定义类型

### 构造函数-原型模式

* 构造函数模式用于定义实例属性
* 原型模式用于定义方法和共享的属性

混合模式

```javascript
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
}

Person.prototype.sayName = function () {
  console.log(this.name);
}
```

动态模式

```javascript
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;

  if (typeof this.sayName !== "function") {
    Person.prototype.sayName = function () {
      console.log(this.name);
    }
  }
}
```

### 工厂模式

* 抛弃构造函数模式，直接在函数内创建一个新对象
* 设置该对象的属性和方法并返回该对象

寄生模式

```javascript
function Person(name, age, job) {
  var o = new Object();

  o.name = name;
  o.age = age;
  o.job = job;

  o.sayName = function () {
    console.log(this.name);
  }

  return o;
}
```

稳妥模式

```javascript
function Person(name, age, job) {
  var o = new Object();

  //可以在这里定义私有变量和函数

  o.sayName = function () {
    console.log(name);
  }

  return o;
}
```

## 继承

使用原型链实现对原型属性和方法的继承

### 组合继承

常用的继承方式

```javascript
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

Super.prototype.sayName = function () {
  console.log(this.name);
}

function SubType(name, age) {
  SuperType.call(this, name);

  this.age = age;
}

SubType.prototype = new SuperType();

SubType.prototype.sayAge = function () {
  console.log(this.age);
}
```

### 原型式继承

```javascript
function proto(o) {
  function F() {};
  F.prototype = o;
  return new F();

  // 或直接使用Object.Create()方法
  // return Object.create(o);
}
```

### 寄生式继承

```javascript
function proto(o) {
  function F() {};
  F.prototype = o;
  return new F();

  // 或直接使用Object.Create()方法
  // return Object.create(o);
}

function createAnother(o) {
  var clone = proto(o);
  clone.sayHi = function () {
    console.log("hi");
  }

  return clone;
}
```

### 寄生组合式继承

最理想的继承方式

```javascript
function proto(o) {
  function F() {};
  F.prototype = o;
  return new F();

  // 或直接使用Object.Create()方法
  // return Object.create(o);
}

function inheritPrototype(subType, superType) {
  var prototype = proto(super.prototype);
  subType.prototype = prototype;
  prototype.constructor = subType;
}

function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

Super.prototype.sayName = function () {
  console.log(this.name);
}

function SubType(name, age) {
  SuperType.call(this, name);

  this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function () {
  console.log(this.age);
}
```

### 多重继承

```javascript
function M1() {
  this.hello = 'hello';
}

function M2() {
  this.world = 'world';
}

function S() {
  M1.call(this);
  M2.call(this);
}

// 继承 M1
S.prototype = Object.create(M1.prototype);
// 继承链上加入 M2
Object.assign(S.prototype, M2.prototype);

// 指定构造函数
S.prototype.constructor = S;

var s = new S();
s.hello // 'hello：'
s.world // 'world'
```
