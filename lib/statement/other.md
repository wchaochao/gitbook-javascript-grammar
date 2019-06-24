# 其他语句

标签（空格分隔）： JavaScript语法

---

## with语句

```
WithStatement :
  with ( Expression ) Statement
```

### 算法

```javascript
WithStatement : with ( Expression ) Statement

/**
 * Expression转换为对象
 * 基于当前上下文创建一个新作用域，绑定上述对象
 * 在新作用域中执行Statement，执行完后回归原作用域
 */
```

示例

```javascript
let o = {
  a: 1
}
with (o) {
  a = 2 // o.a = 2
}
```

### 严格模式限制

```javascript
/**
 * 严格模式，直接抛出SyntaxError
 */
 
if (strict) {
  throw SyntaxError
}
```

## try语句

```
TryStatement :
  try Block Catch // try/catch
  try Block Finally // try/finally
  try Block Catch Finally // try/catch/finally

Catch :
  catch ( Identifier ) Block // catch语句

Finally :
  finally Block // finally语句
```

### try/catch

```javascript
TryStatement : try Block Catch

/**
 * 执行Block
 * 不抛出异常，忽略Catch，执行Catch后的语句
 * 抛出异常，将异常传给Catch，执行Catch中的语句
 */
```

示例

```javascript
// 不是throw
try {
  let a = 1
} catch (e) {
  console.log(e) // 不执行
}

// 是throw
try {
  throw 1
} catch (e) {
  console.log(e) // e = 1
}
```

### catch

```javascript
Catch : catch ( Identifier ) Block

/**
 * 基于当前上下文创建一个新作用域，Identifier值为捕获的异常
 * 在新作用域中执行Statement，执行完后回归原作用域
 */
```

严格模式限制

```javascript
/**
 * 严格模式下，Identifier为eval或argument，抛出SyntaxError
 */
if (strict) {
  if (<identifier-name-string> === 'eval' || <identifier-name-string> === 'argument') {
    throw SyntaxError
  }
}
```

### try/finally

```javascript
TryStatement : try Block Finally

/**
 * 执行Block，不管是否有throw、return、continue、break中断，执行Finally
 * Finally有throw、return、continue、break中断，使用Finally的中断
 * Finally无throw、return、continue、break中断，使用Block的
 */
let B = Block
let F = Finally
if (F.type === normal) {
  return B
} else {
  return F
}
```

示例

```javascript
// Finally为normal
function foo () {
  try {
    return 1
  } finally {
    let a = 1
  }
}

foo() // 1

// Finally不为normal
function foo () {
  try {
    return 1
  } finally {
    return 2
  }
}

foo() // 2
```

### try/catch/finally

```javascript
 TryStatement : try Block Catch Finally
 
/**
 * 先执行try/catch，再执行try/finally
 */
```

示例

```javascript
try {
  let a = 1
} catch (e) {
  console.log(e) // 不执行
} finally {
  let b = 2
}

try {
  throw 1
} catch (e) {
  console.log(e) // e = 1
  throw 2 // 不抛出
} finally {
  throw 3 // 抛出
}
```
