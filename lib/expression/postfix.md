# 后缀表达式

标签（空格分隔）： JavaScript语法

---

## 后缀自增

```javascript
PostfixExpression : LeftHandSideExpression [no LineTerminator here] ++

/*
 * 对LeftHandSideExpression作加一操作并返回原值
 */
```

示例

```javascript
let a = 1
a++ + a++ // 1 + 2, a = 3
```

## 后缀自减

```javascript
PostfixExpression : LeftHandSideExpression [no LineTerminator here] --

/*
 * 对LeftHandSideExpression作减一操作并返回原值
 */
```

示例

```javascript
let a = 1
a-- + a-- // 1 + 0, a = -1
```
