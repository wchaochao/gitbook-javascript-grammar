# 循环语句

标签（空格分隔）： JavaScript语法

---

```
IterationStatement :
  do Statement while ( Expression ); // do/while语句
  while ( Expression ) Statement // while语句
  for ( ExpressionNoIn(opt) ; Expression(opt) ; Expression(opt) ) Statement // for语句 无声明
  for ( var VariableDeclarationListNoIn ; Expression(opt) ; Expression(opt) ) Statement // for语句 有声明
  for ( LeftHandSideExpression in Expression ) Statement  // for/in语句 无声明
  for ( var VariableDeclarationNoIn in Expression ) Statement // for/in语句 有声明
```

## do/while语句

```javascript
IterationStatement : do Statement while ( Expression );

/**
 * 执行Statement
 * Expression转换为false，退出循环
 * Expression转换为true，继续下次循环
 */
```

示例

```javascript
// normal
let i = 0
do {
  i++
} while (i < 3)

// continue
let i = 0
do {
  i++
  if (i === 1) {
    continue
  }
} while (i < 3)

// continue label
outer: for (let j = 0; j < 3; j++) {
  let i = 0
  do {
    i++
    if (i === 1) {
      continue outer
    }
  } while (i < 3)
}

// break
let i = 0
do {
  i++
  if (i === 1) {
    break
  }
} while (i < 3)

// break label
outer: for (let j = 0; j < 3; j++) {
  let i = 0
  do {
    i++
    if (i === 1) {
      break outer
    }
  } while (i < 3)
}

// throw
let i = 0
do {
  i++
  if (i === 1) {
    throw i
  }
} while (i < 3)
```

## while语句

```javascript
IterationStatement : while ( Expression ) Statement

/**
 * Expression转换为false，退出循环
 * Expression转换为true，执行Statement，继续下次循环
 */
```

示例

```javascript
// normal
let i = 0
while (i < 3) {
  i++
} 

// continue
let i = 0
while (i < 3) {
  i++
  if (i === 1) {
    continue
  }
} 

// continue label
outer: for (let j = 0; j < 3; j++) {
  let i = 0
  while (i < 3) {
    i++
    if (i === 1) {
      continue outer
    }
  } 
}

// break
let i = 0
while (i < 3) {
  i++
  if (i === 1) {
    break
  }
} 

// break label
outer: for (let j = 0; j < 3; j++) {
  let i = 0
  while (i < 3) {
    i++
    if (i === 1) {
      break outer
    }
  } 
}

// throw
let i = 0
while (i < 3) {
  i++
  if (i === 1) {
    throw i
  }
} 
```

## for语句

### 无声明

```javascript
IterationStatement : for ( ExpressionNoIn(opt) ; Expression(opt) ; Expression(opt) ) Statement

/**
 * 执行ExpressionNoIn
 * 第一个Expression转换为false，跳出循环
 * 第一个Expression转换为true，执行Statement，再执行第二个Expression并继续下次循环
 */
```

示例

```javascript
// normal
let i, count = 0
for (i = 0; i < 3; i++) {
  count++
}

// continue
let i, count = 0
for (i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    continue
  }
}

// continue label
outer: for (let j = 0; j < 3; j++) {
  let i, count = 0
  for (i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      continue outer
    }
  }
}

// break
let i, count = 0
for (i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    break
  }
}

// break label
outer: for (let j = 0; j < 3; j++) {
  let i, count = 0
  for (i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      break outer
    }
  }
}

// throw
let i, count = 0
for (i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    throw 1
  }
}
```

### 有声明

```javascript
IterationStatement : for ( var VariableDeclarationListNoIn ; Expression(opt) ; Expression(opt) ) Statement

/**
 * 执行VariableDeclarationListNoIn
 * 第一个Expression转换为false，跳出循环
 * 第一个Expression转换为true，执行Statement，再执行第二个Expression并继续下次循环
 */
```

示例

```javascript
// normal
let count = 0
for (let i = 0; i < 3; i++) {
  count++
}

// continue
let count = 0
for (let i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    continue
  }
}

// continue label
outer: for (let j = 0; j < 3; j++) {
  let count = 0
  for (let i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      continue outer
    }
  }
}

// break
let count = 0
for (let i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    break
  }
}

// break label
outer: for (let j = 0; j < 3; j++) {
  let count = 0
  for (let i = 0; i < 3; i++) {
    count++
    if (count === 1) {
      break outer
    }
  }
}

// throw
let count = 0
for (let i = 0; i < 3; i++) {
  count++
  if (count === 1) {
    throw 1
  }
}
```

## for/in语句

### 无声明

```javascript
IterationStatement : for ( LeftHandSideExpression in Expression ) Statement

/**
 * Expression不能转换为对象，直接跳出循环
 * Expression能转换为对象，转换为对象
 *     遍历对象及其原型链上的可枚举属性，将属性名赋值给LeftHandSideExpression，执行Statement
 */
```

示例

```javascript
// normal
let o = {a: 1}
let i, count = 0
for (i in o) {
  count++
}

// continue
let o = {a: 1}
let i, count = 0
for (i in o) {
  count++
  if (count === 1) {
    continue
  }
}

// continue label
outer: for (let i = 0; i < 3; i++) {
  let o = {a: 1}
  let i, count = 0
  for (i in o) {
    count++
    if (count === 1) {
      continue outer
    }
  } 
}

// break
let o = {a: 1}
let i, count = 0
for (i in o) {
  count++
  if (count === 1) {
    break
  }
}

// break label
outer: for (let i = 0; i < 3; i++) {
  let o = {a: 1}
  let i, count = 0
  for (i in o) {
    count++
    if (count === 1) {
      break outer
    }
  } 
}

// throw
let o = {a: 1}
let i, count = 0
for (i in o) {
  count++
  if (count === 1) {
    throw 1
  }
}
```

### 有声明

```javascript
IterationStatement : for ( var VariableDeclarationNoIn in Expression ) Statement

/**
 * 执行VariableDeclarationNoIn
 * Expression不能转换为对象，直接跳出循环
 * Expression能转换为对象，转换为对象
 *     遍历对象及其原型链上的可枚举属性，将属性名赋值给变量，执行Statement
 */
```

示例

```javascript
// normal
let o = {a: 1}
let count = 0
for (let i in o) {
  count++
}

// continue
let o = {a: 1}
let count = 0
for (let i in o) {
  count++
  if (count === 1) {
    continue
  }
}

// continue label
outer: for (let i = 0; i < 3; i++) {
  let o = {a: 1}
  let count = 0
  for (let i in o) {
    count++
    if (count === 1) {
      continue outer
    }
  } 
}

// break
let o = {a: 1}
let count = 0
for (let i in o) {
  count++
  if (count === 1) {
    break
  }
}

// break label
outer: for (let i = 0; i < 3; i++) {
  let o = {a: 1}
  let count = 0
  for (let i in o) {
    count++
    if (count === 1) {
      break outer
    }
  } 
}

// throw
let o = {a: 1}
let count = 0
for (let i in o) {
  count++
  if (count === 1) {
    throw 1
  }
}
```
