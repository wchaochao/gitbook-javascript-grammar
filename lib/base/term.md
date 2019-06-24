# 词法

标签（空格分隔）： JavaScript语法

---

## 源代码

* 字符集：Unicode字符集
* 编码方式：USC-2

## 空白字符

| 字符编码值 | 名称 | 符号 |
| --- | --- | --- |
| \u0009 | Tab 制表符 | `<TAB>` |
| \u000B | Vertical Tab 垂直制表符 | `<VT>` |
| \u000C | Form Feed 换页符 | `<FF>` |
| \u0020 | Space 空格符 | `<SP>` |
| \u00A0 | No-break space 不换行空格符 | `<NBSP>` |
| \uFEFF | Byte Order Mark 零宽度非换行空格符 | `<BOM>` |
| Other category “Zs” | Any other Unicode “space separator” 其它空格分隔符 | `<USP>` |

## 行终结符

| 字符编码值 | 名称 | 符号 |
| --- | --- | --- |
| \u000A | Line Feed 换行符 | `<LF>` |
| \u000D | Carriage Return 回车符 | `<CR>` |
| \u2028 | Line separator 行分隔符 | `<LS>` |
| \u2029 | Paragraph separator 段分隔符 | `<PS>` |

## 注释

* 单行注释
* 多行注释

```javascript
// 单行注释

/*
 * 多行注释
 */
```

## 标识符

变量名、函数名、函数参数名

* 字母、数字、下划线、$组成
* 数字不开头
* 大小写不同
* 不为关键字、保留字
* 尽量语义化

## 关键字

有特定意义的词

 列1 | 列2 | 列3 | 列4
---------|----------|---------|----------
 break | do | instanceof | typeof
 case | else | new | var
 catch | finally | return | void
 continue | for | switch | while
 debugger | function | this | with
 default | if | throw | delete
 in | try

## 保留字

可能成为关键字的词

 列1 | 列2 | 列3 | 列4
---------|----------|---------|----------
 abstract | enum | int | short
 boolean | export | interface | static
 byte | extends | long | super
 char | final | native | synchronized
 class | float | package | throws
 implements | protected | volatile | double
 import | public | let | yield

## 自动分号插入

省略分号时自动插入分号用于终止语句

```javascript
// 源文本
{ 1
2 } 3

// 自动插入分号
{ 1
;2 ;} 3;

// 源文本
return
a + b

// 自动插入分号
return;
a + b;

// 源文本
a = b
++c

// 自动插入分号
a = b;
++c;

// 源文本
a = b + c
(d + e).print()

// 自动插入分号
a = b + c(d + e).print()
```
