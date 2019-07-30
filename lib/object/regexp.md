# 正则表达式

标签（空格分隔）： JavaScript语法

---

用于字符串匹配, `/pattern/flag`

## 模式

模式匹配

### 字面量字符

只表示字面的含义

```javascript
/dog/ // 表示dog
```

### 转义字符

普通转义

```
\0: 匹配一个NUL字符
\r: 匹配一个回车符
\n: 匹配一个换行符
\t: 匹配一个水平制表符
\v: 匹配一个垂直制表符
[\b]: 匹配一个退格符
\f: 匹配一个换页符
\cX: X为A~Z的一个字母，匹配一个控制字符
\x两位十六进制: 匹配一个对应的ASCII字符
\u四位十六进制: 匹配一个对应的Unicode字符
```

特殊符号转义

```
{} [] () . * + ? ^ $ | \等字符在正则中有特殊用途，需要用\转义
```

### 字符集

```
[xyz]: 匹配任意一个字符集中的字符，连字符表范围
[^xyz]: 匹配任意一个不在字符集中的字符，连字符表范围

[.*+?\b]: 匹配.、*、+、?、退格符中的一个
[^]: 匹配任意字符
```

示例

```javascript
/[abc]/.test('hello world') // false
/[abc]/.test('apple') // true

/[^abc]/.test('hello world') // true
/[^abc]/.test('bbc') // false

var str = "\u0130\u0131\u0132";
/[\u0128-\uFFFF]/.test(str) // true
```

### 字符类

```
.: 匹配行结束符(回车\r、换行\n、行分隔符\u2028、段分隔符\u2029)外的任意单个字符
\d: 匹配任意阿拉伯数字，等价于[0-9]
\D: 匹配任意非阿拉伯数字，等价于[^0-9]
\w: 匹配任意字母、数字、下划线，等价于[A-Za-z0-9_]
\W: 匹配任意非字母、数字、下划线，等价于[^A-Za-z0-9_]
\s: 匹配一个空白符，包括空格符、制表符、换页符、换行符和其他Unicode空格
\S: 匹配一个非空白符

Unicode辅助平面字符在JavaScript中算两个字符，与.不匹配
```

### 数量

```
x{n}: 匹配 x n次
x{n,}: 匹配 x n次或以上，尽可能多的匹配(贪婪模式)，加上?变为尽可能少的匹配(非贪婪模式)
x{n, m}: 匹配 x n次到m次，尽可能多的匹配(贪婪模式)，加上?变为尽可能少的匹配(非贪婪模式)
x*: 匹配 x 0次或以上，等同于{0,}，尽可能多的匹配(贪婪模式)，加上?变为尽可能少的匹配(非贪婪模式)
x+: 匹配 x 1次或以上，等同于{1,}，尽可能多的匹配(贪婪模式)，加上?变为尽可能少的匹配(非贪婪模式)
x?: 匹配 x 0次到1次，等同于{0, 1}，尽可能多的匹配(贪婪模式)，加上?变为尽可能少的匹配(非贪婪模式)
```

示例

```javascript
'abb'.match(/ab*b/) // ["abb"]
'abb'.match(/ab*?b/) // ["ab"]

'abb'.match(/ab?b/) // ["abb"]
'abb'.match(/ab??b/) // ["ab"]
```

### 位置

```
^: 匹配字符串开始处，多行模式时，匹配字符串的行首
$: 匹配字符串结尾处，多行模式时，匹配字符串的行尾
\b: 匹配单词的边界(词首独立，如用空格或连字符隔开单词)
\B: 匹配非单词边界
```

示例

```javascript
/^test/.test('test123') // true
/test$/.test('new test') // true
/world$/.test('hello world\n') // false
/world$/m.test('hello world\n') // true

/\bworld/.test('hello world') // true
/\bworld/.test('hello-world') // true
/\bworld/.test('helloworld') // false
```

### 断言

```
x(?=y): 当x后面紧跟着y时才匹配x
x(?!=y): 当x后面不紧跟着y时才匹配x
```

示例

```javascript
// 先行断言
var m = 'abc'.match(/b(?=c)/);
m // ["b"]

// 先行否定断言
/\d+(?!\.)/.exec('3.14') // ["14"]
```

### 分组

```
(x): 捕获组，匹配x并捕获匹配项
(?:x): 非捕获组，匹配x但不捕获匹配项
```

示例

```javascript
// 匹配标签
var html = '<b class="hello">Hello</b><i>world</i>';
var tag = /<(\w+)([^>]*)>(.*?)<\/\1>/g;

var match = tag.exec(html);

match[1] // "b"
match[2] // " class="hello""
match[3] // "Hello"

match = tag.exec(html);

match[1] // "i"
match[2] // ""
match[3] // "world"

// 非捕获组匹配
var url = /(?:http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/;

url.exec('http://google.com/');
// ["http://google.com/", "google.com", "/"]
```

### 引用

```
\n: n为整数，反向引用，引用第n个捕获括号中的匹配项
```

示例

```javascript
/(.)b(.)\1b\2/.test("abcabc") // true
/y((..)\2)\1/.test('yabababab') // true
```

### 选择

```
x|y: 匹配x或y
```

示例

```javascript
// 匹配fred、barney、betty之中的一个
/fred|barney|betty/

/a( |\t)b/.test('a\tb') // true
```

## 标志

匹配方式

* `g`: 全局匹配
* `i`: 忽略大小写
* `m`: 多行匹配
