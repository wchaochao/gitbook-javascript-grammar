# 中断语句

标签（空格分隔）： JavaScript语法

---

## label语句

标签语句，用于continue label和break label

* 语句默认有个空的标签组
* 循环语句和switch语句默认有个包含单个元素 empty 的标签组

```
LabelledStatement :
  Identifier : Statement
```

### 算法

```javascript
Identifier : Statement

/**
 * 给Statement添加标签并执行
 */
```

示例

```javascript
outer: for (let j = 0; j < 3; j++) {
  let count = 0
  for (let i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      break outer
    }
  }
}
```

## continue语句

进入下一次循环

* 出现在循环语句中
* Identifier在循环语句的标签组中

```
ContinueStatement :
  continue ;
  continue [no LineTerminator here] Identifier ;
```

### continue

```javascript
ContinueStatement : continue ;

/**
 * 进入当前循环的下一次循环
 */
```

示例

```javascript
let count = 0
for (let i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    continue
  }
}
```

### continue label

```javascript
ContinueStatement : continue [no LineTerminator here] Identifier ;

/**
 * 退出当前循环，进入Identifier循环的下一次循环
 */
```

示例

```javascript
outer: for (let j = 0; j < 3; j++) {
  let count = 0
  for (let i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      continue outer
    }
  }
}
```

## break语句

跳出循环或switch语句

* 出现在循环语句或switch语句中
* Identifier在语句的标签组中

```
BreakStatement :
  break ;
  break [no LineTerminator here] Identifier ;
```

### break

```javascript
BreakStatement : break ;

/**
 * 跳出当前循环或switch
 */
```

示例

```javascript
let count = 0
for (let i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    break
  }
}

let a = 1
let count = 0
switch (a) { // count = 1
  case 1: 
    count += 1
    break
  case 2:
    count += 2
    break
}
```

### break label

```javascript
BreakStatement : break [no LineTerminator here] Identifier ;

/**
 * 跳出Identifier循环
 */
```

示例

```javascript
outer: for (let j = 0; j < 3; j++) {
  let count = 0
  for (let i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      break outer
    }
  }
}
```

## return语句

返回值

* 出现在函数中
* 停止函数执行并把值返回给caller

```
ReturnStatement :
  return ;
  return [no LineTerminator here] Expression ;
```

### return

```javascript
ReturnStatement : return ;

/**
 * 返回undefined
 */
```

示例

```javascript
function foo () {
  return
}

let a = foo() // undefined
```

### return value

```javascript
ReturnStatement : return [no LineTerminator here] Expression ;

/**
 * 返回value
 */
```

示例

```javascript
function foo () {
  return 1
}

let a = foo() // 1
```

## throw语句

抛出错误

```
ThrowStatement :
  throw [no LineTerminator here] Expression ;
```

### 算法

```javascript
ThrowStatement : throw [no LineTerminator here] Expression ;

/**
 * 抛出Expression并停止执行
 */
```

示例

```javascript
throw 1
throw new Error('error')
```

## debugger语句

```javascript
DebuggerStatement :
  debugger ;
```

### 算法

```javascript
DebuggerStatement : debugger ;

/**
 * 断点调试
 */
```

示例

```javascript
function foo () {
  debugger
}
foo()
```
