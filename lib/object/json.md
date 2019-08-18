# JSON

标签（空格分隔）： JavaScript语法

---

JavaScript对象表示法(`JavaScript Object Notation`), 一种基于文本、独立于语言的轻量级数据交换格式

## 词法

### JSONWhiteSpace

```
JSONWhiteSpace :: // JSON空白字符
  <SP> // 空格
  <TAB> // Tab
  <CR> // 回车
  <LF> // 换行
```

### JSONNullLiteral

```
JSONNullLiteral :: // JSON Null字面量
  NullLiteral
```

示例

```javascript
null
```

### JSONBooleanLiteral

```javascript
JSONBooleanLiteral :: // JSON Boolean字面量
  BooleanLiteral
```

示例

```javascript
true
false
```

### JSONNumber

```
JSONNumber :: // JSON数字
  -(opt) DecimalIntegerLiteral JSONFraction(opt) ExponentPart(opt)

JSONFraction :: // JSON小数部分
  . DecimalDigits
```

示例

```javascript
1
1.2
1e+2
1.2e+2
-1.2e+2
```

### JSONString 

```
JSONString :: // JSON字符串
  "JSONStringCharacters(opt)"

JSONStringCharacters :: // 多个JSON字符
  JSONStringCharacter JSONStringCharacters(opt)

JSONStringCharacter :: // 单个JSON字符
  SourceCharacter but not one of " or \ or U+0000 through U+001F // 普通JSON字符
  \ JSONEscapeSequence // JSON转义系列

JSONEscapeSequence :: // JSON转义系列
  JSONEscapeCharacter // JSON转义字符
  UnicodeEscapeSequence // Unicode转义系列

JSONEscapeCharacter :: one of // JSON转义字符
" / \ b f n r t
```

示例

```javascript
""
"'"
"\""
"ab"
```

## 句法

```
JSONText : // JSON文本
  JSONValue // JSON值

JSONValue : // JSON值
  JSONNullLiteral // JSON Null字面量
  JSONBooleanLiteral // JSON Boolean字面量
  JSONNumber // JSON数字
  JSONString // JSON字符串
  JSONObject // JSON对象
  JSONArray // JSON数组

JSONObject : // JSON对象
  { } // 空对象
  { JSONMemberList } // 非空对象

JSONMemberList : // JSON属性列表
  JSONMember // JSON单个属性
  JSONMemberList , JSONMember // JSON多个属性

JSONMember : // JSON单个属性
  JSONString : JSONValue

JSONArray : // JSON数组
  [ ] // 空数组
  [ JSONElementList ] // 非空数组

JSONElementList : // JSON元素列表
  JSONValue // JSON单个元素
  JSONElementList , JSONValue // 
```

* null：`null`
* 布尔值：`true, false`
* 数值: 十进制数值
  * 小数点后要有数字
  * 禁止前导`0`
  * 不能使用八进制和十六进制
  * 不能使用`NaN, Infinity, -Infinity`
* 字符串：必须使用双引号
* 数组
  * 元素必须是JSON格式值
  * 最后一个元素后不能有逗号
* 对象
  * 属性名必须放在双引号中
  * 属性值必须是JSON格式值
  * 最后一个属性后不能有逗号

示例

```javascript
// JSONNullLiteral
null

// JSONBooleanLiteral
true
false

// JSONNumber
1
1.2
1e+2
1.2e+2
-1.2e+2

// JSONString
""
"'"
"\""
"ab"

// JSONObject
{}
{
  "a": 1
}
{
  "a": 1,
  "b": {}
}

// JSONArray
[]
[1]
[1, []]
```
