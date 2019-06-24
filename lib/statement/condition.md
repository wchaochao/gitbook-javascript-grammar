# 条件语句

标签（空格分隔）： JavaScript语法

---

## if语句

```
IfStatement :
  if ( Expression ) Statement else Statement // if/else
  if ( Expression ) Statement // 单if
```

### if/else

```javascript
IfStatement : if ( Expression ) Statement else Statement

/**
 * Expression转换为true，执行第一个语句
 * Expression转换为false，执行第二个语句
 */
```

示例

```javascript
let a = 10

if (a > 5) {
  a++
} else {
  a--
}

if (a > 5) {
  a++
} else if (a > 0) {
  a--
} else {
  a++
}
```

### 单If

```javascript
IfStatement : if ( Expression ) Statement

/**
 * Expression转换为true，执行Statement
 * Expression转换为false，执行if后的语句
 */
```

示例

```javascript
let a = 10

if (a > 5) {
  a++
}
```

## switch语句

```
SwitchStatement :
  switch ( Expression ) CaseBlock // switch

CaseBlock : // case块
  { CaseClauses(opt) } // case项 不含default case
  { CaseClauses(opt) DefaultClause CaseClauses(opt) } // case项 含default case

CaseClauses : // case项
  CaseClause // 单个case项
  CaseClauses CaseClause // 多个case项

CaseClause : // 单个case项
  case Expression : StatementList(opt)

DefaultClause :
  default : StatementList(opt)
```

### 无default

```javascript
CaseBlock : { CaseClauses(opt) }

/**
 * 遍历case项，匹配Expression（严格相等）
 * 有匹配，执行匹配case项的statementList
 *     有break、cintinue、throw、return中断语句, 跳出switch
 *     无break、cintinue、throw、return中断语句, 执行下一个case项的statementList，同上
 * 无匹配，执行switch后的语句
 */
```

示例

```javascript
// 有匹配且中断
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

// 有匹配且不中断
let a = 1
let count = 0
switch (a) { // count = 3
  case 1: 
    count += 1
  case 2:
    count == 2
}

// 无匹配
let a = '1'
let count = 0
switch (a) { // count = 0
  case 1: 
    count += 1
    break
  case 2:
    count += 2
    break
}
```

### 有default

```javascript
CaseBlock : { CaseClauses(opt) DefaultClause CaseClauses(opt) }

/**
 * 遍历第一个case块，匹配Expression（严格相等）
 * 有匹配，执行匹配case项的statementList
 *     有break、cintinue、throw、return中断语句, 跳出switch
 *     无break、cintinue、throw、return中断语句，执行下一个case项（A、default、B）的statementList，同上
 * 无匹配，遍历第二个case块，匹配Expression（严格相等）
 *    有匹配，执行匹配case项的statementList
 *        有break、cintinue、throw、return中断语句, 跳出switch
 *        无break、cintinue、throw、return中断语句，执行下一个case项（B）的statementList，同上
 *    无匹配，匹配default case
 *        有break、cintinue、throw、return中断语句, 跳出switch
 *        无break、cintinue、throw、return中断语句，执行下一个case项（B）的statementList，同上
 */
```

匹配第一个case块，且中断

```javascript
let a = 2
let count = 0
switch (a) { // count = 2
  case 1: 
    count+=1
    break
  case 2:
    count+=2
    break
  default: 
    count+=3
    break
  case 4: 
    count+=4
    break
  case 5:
    count+=5
    break
}
```

匹配第一个case块，且不中断

```javascript
let a = 2
let count = 0
switch (a) { // count = 5
  case 1: 
    count+=1
    break
  case 2:
    count+=2
  default: 
    count+=3
    break
  case 4: 
    count+=4
    break
  case 5:
    count+=5
    break
}
```

匹配第二个case块，且中断

```javascript
let a = 4
let count = 0
switch (a) { // count = 4
  case 1: 
    count+=1
    break
  case 2:
    count+=2
    break
  default: 
    count+=3
    break
  case 4: 
    count+=4
    break
  case 5:
    count+=5
    break
}
```

匹配第二个case块，且不中断

```javascript
let a = 4
let count = 0
switch (a) { // count = 9
  case 1: 
    count+=1
    break
  case 2:
    count+=2
    break
  default: 
    count+=3
    break
  case 4: 
    count+=4
  case 5:
    count+=5
    break
}
```

匹配default，且中断

```javascript
let a = 3
let count = 0
switch (a) { // count = 3
  case 1: 
    count+=1
    break
  case 2:
    count+=2
    break
  default: 
    count+=3
    break
  case 4: 
    count+=4
    break
  case 5:
    count+=5
    break
}
```

匹配default，且不中断

```javascript
let a = 3
let count = 0
switch (a) { // count = 7
  case 1: 
    count+=1
    break
  case 2:
    count+=2
    break
  default: 
    count+=3
  case 4: 
    count+=4
    break
  case 5:
    count+=5
    break
}
```
