# 正则表达式

标签（空格分隔）： JavaScript语法

---

用于模式匹配，格式`/pattern/flags`

## Pattern语法

```
Pattern :: // 模式
  Disjunction // 析取
  
Disjunction :: // 析取
  Alternative // 单个选择
  Alternative | Disjunction // 多个选择

Alternative :: // 单个选择
  [empty] // 空匹配项
  Alternative Term // 多个匹配项

Term :: // 单个匹配项
  Assertion // 断言
  Atom // 原子匹配
  Atom Quantifier // 重复匹配

Assertion :: // 断言
  ^ // 开头处
  $ // 结尾处
  \ b // 单词边界处
  \ B // 非单词边界处
  ( ? = Disjunction ) // 后面紧跟着
  ( ? ! Disjunction ) // 后面不紧跟着

Quantifier :: // 数量
  QuantifierPrefix // 贪婪匹配
  QuantifierPrefix ? // 非贪婪匹配

QuantifierPrefix :: // 数量符号
  * // 任意次
  + // 至少1次
  ? // 0次或1次
  { DecimalDigits } // n次
  { DecimalDigits , } // 至少n次
  { DecimalDigits , DecimalDigits } // n到m次

Atom :: // 原子匹配
  PatternCharacter // 普通字符
  . // 行结束符外的任意字符
  \ AtomEscape // 转义
  CharacterClass // 字符类
  ( Disjunction ) // 捕获组
  ( ? : Disjunction ) // 非捕获组

PatternCharacter :: // 普通字符
  SourceCharacter but not one of
    ^ $ \ . * + ? ( ) [ ] { } |

AtomEscape :: // 转义
  DecimalEscape // 十进制转义
  CharacterEscape // 字符转义
  CharacterClassEscape // 字符类转义

DecimalEscape :: // 十进制转义
  DecimalIntegerLiteral [lookahead ∉ DecimalDigit]

CharacterEscape :: // 字符转义
  ControlEscape // 控制转义
  c ControlLetter // 控制字符
  HexEscapeSequence // 十六进制转义
  UnicodeEscapeSequence // Unicode转义
  IdentityEscape // 任意字符转义

ControlEscape :: one of // 控制转义
  f n r t v

ControlLetter :: one of // 控制字符
  a b c d e f g h i j k l m n o p q r s t u v w x y z
  A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

IdentityEscape :: // 任意字符转义
  SourceCharacter but not IdentifierPart
  <ZWJ>
  <ZWNJ>

CharacterClassEscape :: one of // 字符类转义
  d D s S w W

CharacterClass :: // 字符类
  [ [lookahead ∉ {^}] ClassRanges ] // 类范围
  [ ^ ClassRanges ] // 类范围之外

ClassRanges :: // 类范围
  [empty] // 空范围
  NonemptyClassRanges // 非空类范围

NonemptyClassRanges :: // 非空类范围
  ClassAtom // 类原子
  ClassAtom NonemptyClassRangesNoDash // 多个类原子
  ClassAtom - ClassAtom ClassRanges // 类原子范围

ClassAtom :: // 类原子
  - // 连字符类原子
  ClassAtomNoDash // 非连字符类原子

ClassAtomNoDash :: // 非连字符类原子
  SourceCharacter but not one of \ or ] or - // 普通字符类原子
  \ ClassEscape // 转义类原子

ClassEscape :: // 转义类原子
  DecimalEscape // 十进制转义
  b // 退格转义
  CharacterEscape // 字符转义
  CharacterClassEscape // 字符类转义

NonemptyClassRangesNoDash :: // 非空非连字符范围
  ClassAtom // 类原子
  ClassAtomNoDash NonemptyClassRangesNoDash // 多个类原子
  ClassAtomNoDash - ClassAtom ClassRanges // 类原子范围
```

## 术语

* `Input`: 匹配的字符串
* `InputLength`: 匹配字符串的长度
* `NcapturingParens`: 捕获组数目
* `IgnoreCase`: 是否忽略大小写
* `Multiline`: 是否多行匹配
* `CharSet`: 字符集
* `State`: {endIndex: Number, captures: Array}，匹配状态
 * `endIndex`: 下次匹配开始的位置
 * `captures`: 捕获组的匹配情况
* `MatchResult `: State | failure, 匹配结果
* `Continuation`: (State) => MatchResult, 下次匹配函数
* `Matcher`: (State, Continuation) => MatchResult, 匹配函数
 * 先对初始State进行Matcher匹配，得到新State再进行Continuation匹配
* `AssertionTester`: (State) => Boolean, 判断能否匹配
* `EscapeValue`: 转义值（捕获组引用）

## Pattern

```javascript
Pattern :: Disjunction

/**
 * 执行Disjunction获取Matcher
 * 返回一个函数，参数为str、index
 *    初始状态为{endIndex: index, captures: new Array(NcapturingParens)}，进行Matcher匹配，返回匹配结果
 */
```

## Disjunction

### 单个选择

```javascript
Disjunction :: Alternative

/**
 * 执行Alternative获取Matcher并返回这个Matcher
 */
```

### 多个选择

```javascript
Disjunction :: Alternative | Disjunction

/**
 * 执行Alternative获取Matcher1
 * 执行Disjunction获取Matcher2
 * 返回一个Matcher, 该Matcher先匹配Matcher1，failure后再匹配Matcher2
 */
```

示例

```javascript
/a|ab/.exec('abc') // ['a', index: 0, input: 'abc']
/((a)|(ab))((c)|(bc))/.exec('abc') // ['abc', 'a', 'a', undefined, 'bc', undefined, 'bc', index: 0, input: 'abc']
```

## Alternative

### 空匹配项

```javascript
Alternative :: [empty]

/**
 * 返回一个Matcher，该Matcher执行下一个匹配函数
 */
```

### 多个匹配项

```javascript
Alternative :: Alternative Term

/**
 * 执行Alternative获取Matcher1
 * 执行Term获取Matcher2
 * 返回一个Matcher，该Matcher先匹配Matcher1，再匹配Matcher2
 */
```

## Term

### 断言

```javascript
Term :: Assertion

/**
 * 执行Assertion获取AssertionTester 
 * 返回一个Matcher
 *    AssertionTester能匹配时，执行下一个匹配函数
 *    AssertionTester不能匹配时，返回failure
 */
```

### 原子匹配

```javascript
Term :: Atom

/**
 * 执行Atom获取Matcher并返回这个Matcher
 */
```

### 数量个原子匹配

```javascript
Term :: Atom Quantifier

/**
 * 执行Atom获取Matcher
 * 执行Quantifier获取数量范围[min, max]和是否为贪婪匹配greedy
 * 获取Atom内的捕获组索引和数量，进行重复匹配
 *    每次匹配将Atom内的捕获组匹配置为undefined，数量范围减一
 *    连续匹配直至min减为0
 *    非贪婪匹配，将匹配结果传给下一次匹配
 *       整体有匹配，返回该匹配结果
 *       整体无匹配，进行下一次重复匹配
 *    贪婪匹配，进行下一次重复匹配
 *       整体有匹配，继续下一次重复匹配，直至max为0
 *       整体无匹配，返回上一次匹配结果
 */
```

示例

```javascript
/a[a-z]{2,4}/.exec('abcdefghi') // ['abcde', index: 0, input: 'abcdefghi']
/a[a-z]{2,4}?/.exec('abcdefghi') // ['abc', index: 0, input: 'abcdefghi']
/(aa|aabaac|ba|b|c)*/.exec('aabaac') // ['aaba', 'ba', index: 0, input: 'aabaac']
/(z)((a+)?(b+)?(c))*/.exec('zaacbbbcac') // ['zaacbbbcac', 'z', 'ac', 'a', undefined, 'c', index: 0, input: 'zaacbbbcac']
```

## Assertion

### 开头

```javascript
Assertion :: ^

/**
 * 返回一个AssertionTester函数
 *    非多行模式，判断endIndex是否为开头
 *    多行模式，判断endIndex是否为行首
 */
```

示例

```javascript
/^hello/.test('hello world') // true
/^hello/m.test('\nhello world') // true
```

### 结尾

```javascript
Assertion :: $

/**
 * 返回一个AssertionTester函数
 *    非多行模式，判断endIndex是否为结尾
 *    多行模式，判断endIndex是否为行尾
 */
```

示例

```javascript
/world$/.test('hello world') // true
/world$/m.test('hello world\n') // true
```

### 单词边界处

```javascript
Assertion :: \ b

/**
 * 返回一个AssertionTester函数
 *    判断endIndex是否为单词边界处，即endIndex - 1与endIndex处的字符一个为单词字符、另一个不为单词字符（[a-zA-Z0-9_]）
 */
```

示例

```javascript
/\bworld/.test('hello world') // true
/\bworld/m.test('hello_world\n') // false
```

### 非单词边界处

```javascript
Assertion :: \ B

/**
 * 返回一个AssertionTester函数
 *    判断endIndex是否不为单词边界处，即endIndex - 1与endIndex处的字符都为单词字符或都不为单词字符（[a-zA-Z0-9_]）
 */
```

示例

```javascript
/\Bworld/.test('hello world') // false
/\Bworld/m.test('hello_world\n') // true
```

### 后面紧跟着

```javascript
Assertion :: ( ? = Disjunction )

/**
 * 执行Disjunction获取Matcher函数m
 * 返回一个Matcher函数，该函数进行m匹配
 *    匹配，将上一个匹配结果的endIndex和这个匹配结果的captures传给下一个匹配函数
 *    不匹配，返回failure
 */
```

示例

```javascript
/a(?=(b))/.exec('ab') // ['a', 'b', index: 0, input: 'ab']
```

### 后面不紧跟着

```javascript
Assertion :: ( ? ! Disjunction )

/**
 * 执行Disjunction获取Matcher函数m
 * 返回一个Matcher函数，该函数进行m匹配
 *    匹配，返回failure
 *    不匹配，将上一个的匹配结果传给下一个匹配函数
 */
```

示例

```javascript
/a(?!(c))/.exec('ab') // ['a', undefined, index: 0, input: 'ab']
```

## Quantifier

### 贪婪匹配

```javascript
Quantifier :: QuantifierPrefix

/**
 * 执行QuantifierPrefix获取数量范围[min, max]
 * 返回{min, max, greedy: true}
 */
```

### 非贪婪匹配

```javascript
Quantifier :: QuantifierPrefix

/**
 * 执行QuantifierPrefix获取数量范围[min, max]
 * 返回{min, max, greedy: false}
 */
```

### 任意次

```javascript
QuantifierPrefix :: *

/**
 * 返回数量范围{min: 0, max: Infinity}
 */
```

示例

```javascript
/a(b*)b/.exec('abb') // ['abb', 'b', index: 0, input: 'abb']
/a(b*?)b/.exec('abb') // ['ab', '', index: 0, input: 'abb']
```

### 至少一次

```javascript
QuantifierPrefix :: +

/**
 * 返回数量范围{min: 1, max: Infinity}
 */
```

示例

```javascript
/a(b+)b/.exec('abbb') // ['abbb', 'bb', index: 0, input: 'abbb']
/a(b+?)b/.exec('abbb') // ['abb', 'b', index: 0, input: 'abbb']
```

### 0次或1次

```javascript
QuantifierPrefix :: ?

/**
 * 返回数量范围{min: 0, max: 1}
 */
```

示例

```javascript
/a(b?)b/.exec('abb') // ['abb', 'b', index: 0, input: 'abb']
/a(b??)b/.exec('abb') // ['ab', '', index: 0, input: 'abb']
```

### n次

```javascript
QuantifierPrefix :: { DecimalDigits }

/**
 * 执行DecimalDigits获取次数n
 * 返回数量范围{min: n, max: n}
 */
```

示例

```javascript
/a(b{1})b/.exec('abb') // ['abb', 'b', index: 0, input: 'abb']
/a(b{1}?)b/.exec('abb') // ['abb', 'b', index: 0, input: 'abb']
```

### 至少n次

```javascript
QuantifierPrefix :: { DecimalDigits , }

/**
 * 执行DecimalDigits获取次数n
 * 返回数量范围{min: n, max: Infinity}
 */
```

示例

```javascript
/a(b{1,})b/.exec('abbb') // ['abbb', 'bb', index: 0, input: 'abbb']
/a(b{1,}?)b/.exec('abbb') // ['abb', 'b', index: 0, input: 'abbb']
```

### n到m次

```javascript
QuantifierPrefix :: { DecimalDigits , DecimalDigits }

/**
 * 执行第一个DecimalDigits获取次数n
 * 执行第二个DecimalDigits获取次数m
 * 返回数量范围{min: n, max: m}
 */
```

示例

```javascript
/a(b{1,3})b/.exec('abbb') // ['abbb', 'bb', index: 0, input: 'abbb']
/a(b{1,3}?)b/.exec('abbb') // ['abb', 'b', index: 0, input: 'abbb']
```

## Atom

### 普通字符

```javascript
Atom :: PatternCharacter

/**
 * 执行PatternCharacter获取字符，组成只含这个字符的字符集
 * 返回一个Matcher函数，该Matcher进行字符集匹配
 *   endIndex处的字符在字符集内，endIndex加1，进行下一次匹配
 *   endIndex处的字符不在字符集内，返回failure
 *   IgnoreCase为true时忽略大小写
 */
```

示例

```javascript
/at/.exec('cat') // ['at', index: 1, input: 'cat']
```

### 行结束符外的任意字符

```javascript
Atom :: .

/**
 * 设置字符集为行结束符外的任意字符
 * 返回一个Matcher函数，该Matcher进行字符集匹配
 */
```

示例

```javascript
/.at/.exec('cat') // ['cat', index: 0, input: 'cat']
```

### 转义字符

```javascript
Atom :: \ AtomEscape

/**
 * 执行AtomEscape获取Matcher函数，返回该Matcher
 * {} [] () . * + ? ^ $ | \等字符在正则中有特殊用途，需要用\转义
 */
```

### 字符类

```javascript
Atom :: CharacterClass

/**
 * 执行CharacterClass获取字符集A和是否在字符集内invert
 * 返回一个Matcher函数，该Matcher进行字符集匹配
 */
```

示例

```javascript
// 类范围
/[-]/.test('-') // true
/[a]/.test('a') // true
/[\0]/.test('\u0000') // true
/(a)[\1]/.test('aa') // false
/[\b]/.test('\b') // true
/[\a]/.test('a') // true
/[\d]/.test('1') // true
/[abc]/.test('a') // true
/[a-c]/.test('a') // true

// 类范围之外
/[^]/.test('a') // true
/[^abc]/.test('a') // false
```

### 捕获组

```javascript
Atom :: ( Disjunction )

/**
 * 执行Disjunction获取Macher1
 * 返回一个Matcher函数，该Matcher进行Macher1匹配
 *    有匹配时，将对应的捕获组置为该匹配，进行下一次匹配
 *    无匹配时，返回failure
 */
let m = evaluate(Disjunction)
let parenIndex = getCaptureParenIndex(Atom)

return (s, c) => {
  let d = (x) => {
    let e = x.endIndex
    let cap = copy(s.captures)
    cap[parenIndex] = Input.slice(s.endIndex, e)
    let r = {
      endIndex: e,
      captures: cap
    }
    return c(r)
  }
  return m(s, d)
}
```

示例

```javascript
/<(\w+)([^>]*)>(.*?)<\/\1>/.exec('<b class="hello">Hello</b>') //  ['<b class="hello">Hello</b>', 'b', ' class="hello"', 'Hello', index: 0, input: '<b class="hello">Hello</b>']
```

### 非捕获组

```javascript
Atom :: ( ? : Disjunction )

/**
 * 执行Disjunction获取Matcher函数，返回该Matcher
 */
```

示例

```javascript
/(?:http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/.exec('http://google.com/') // ['http://google.com/', 'google.com', '/', index: 0, input: 'http://google.com/']
```

## AtomEscape

### 十进制转义

捕获组引用

```javascript
AtomEscape :: DecimalEscape

/**
 * 执行DecimalEscape获取结果E
 * E为<NUL>字符，返回一个Matcher函数，该Matcher进行只含该字符的字符集匹配
 * E为整数n, 表示第n个捕获组，返回一个Matcher函数
 *    n = 0 或 n > NCapturingParens, 抛出SyntaxError
 *    第n个捕获组为undefined, 直接进行下一次匹配
 *    第n个捕获组为字符串, 进行字符串匹配，匹配成功后再进行下一次匹配
 */
```

示例

```javascript
// NUL匹配
/a\0/.test('a\u0000') // true

// 捕获组引用
/(a)\2/.exec('ab') // null
/(a)\1/.exec('aa') // ['aa', 'a', index: 0, input: 'aa']
/(a)(a)*\2/.exec('ab') // ['a', 'a', undefined, index: 0, input: 'ab']
```

### 字符转义

```javascript
AtomEscape :: CharacterEscape

/**
 * 执行CharacterEscape获取字符ch
 * 返回一个Matcher函数，该Matcher进行只含该字符的字符集匹配
 */
```

示例

```javascript
// 控制转义
/\t/.test('\t') // true

// 控制字符
let ctrl = String.fromCharCode(1)
/\ca/.test(ctrl) // true

// 十六进制转义
/\x61/.test('a') // true

// Unicode转义
/\u0061/.test('a') // true

// 任意字符转义
/\+/.test('+') // true
```

### 字符类转义

```javascript
AtomEscape :: CharacterClassEscape

/**
 * 执行CharacterClassEscape获取字符集A
 * 返回一个Matcher函数，该Matcher进行字符集匹配
 */
```

示例

```javascript
// 十进制数字
/\d/.test('0') // true
/\D/.test('0') // false

// 空白符
/\s/.test(' ') // true
/\S/.test(' ') // false

// 单词
/\w/.test('_') // true
/\W/.test('_') // false
```

## DecimalEscape

```javascript
DecimalEscape :: DecimalIntegerLiteral [lookahead ∉ DecimalDigit]

/**
 * 执行DecimalIntegerLiteral获取整数v
 *    v为0，返回<NUL>字符
 *    v不为0，返回v
 */
let v = evaluate(DecimalIntegerLiteral)
if (v === 0) {
  return '\u0000'
} else {
  return v
}
```

## CharacterEscape

### 控制转义

```javascript
CharacterEscape :: ControlEscape

/**
 * 按下表进行转义
 */
```

| ControlEscape | Code Unit | Name | Symbol |
| --- | --- | --- | --- |
| t | \u0009 | horizontal tab | <HT> |
| v	| \u000B | vertical tab	| <VT> |
| r | \u000D | carriage return | <CR> |
| n | \u000A | line feed (new line) | <LF> |
| f	| \u000C | form feed | <FF> |

### 控制字符

```javascript
CharacterEscape :: c ControlLetter

/**
 * 执行ControlLetter获取字符ch
 * 返回ch的Unicode码 % 32后对应的字符
 */
let ch = evaluate(ControlLetter)
let j = ch.charCodeAt(0) % 32
return String.fromCharCode(j)
```

### 十六进制转义

```javascript
CharacterEscape :: HexEscapeSequence

/**
 * 返回两位十六进制对应的Unicode字符
 */
```

### Unicode转义

```javascript
CharacterEscape :: UnicodeEscapeSequence

/**
 * 返回Unicode码对应的Unicode字符
 */
```

### 任意字符转义

```javascript
CharacterEscape :: IdentityEscape

/**
 * 返回字符本身
 */
```

## CharacterClassEscape

### 十进制数字

```javascript
CharacterClassEscape :: d

/**
 * 返回一个字符集，该字符集为0-9
 */
```

### 非十进制数字

```javascript
CharacterClassEscape :: D

/**
 * 返回一个字符集，该字符集为0-9之外的所有字符
 */
```

### 空白符

```javascript
CharacterClassEscape :: s

/**
 * 返回一个字符集，该字符集为空白字符或行结束符
 */
```

### 非空白符

```javascript
CharacterClassEscape :: S

/**
 * 返回一个字符集，该字符集为空白字符、行结束符之外的所有字符
 */
```

### 单词

```javascript
CharacterClassEscape :: w

/**
 * 返回一个字符集，该字符集为a-zA-Z0-9_
 */
```

### 非单词

```javascript
CharacterClassEscape :: W

/**
 * 返回一个字符集，该字符集为a-zA-z0-9_之外的所有字符
 */
```

## CharacterClass

### 类范围

```javascript
CharacterClass :: [ [lookahead ∉ {^}] ClassRanges ]

/**
 * 执行ClassRanges获取字符集A
 * 返回{A, invert: false}
 */
```

### 类范围之外

```javascript
CharacterClass :: [ ^ ClassRanges ]

/**
 * 执行ClassRanges获取字符集A
 * 返回{A, invert: true}
 */
```

## ClassRanges

### 空范围

```javascript
ClassRanges :: [empty]

/**
 * 返回空的字符集
 */
```

### 非空类范围

```javascript
ClassRanges :: NonemptyClassRanges

/**
 * 执行NonemptyClassRanges获取字符集，返回该字符集
 */
```

## NonemptyClassRanges

### 类原子

```javascript
NonemptyClassRanges :: ClassAtom

/**
 * 执行ClassAtom获取字符集，返回该字符集
 */
```

### 多个类原子

```javascript
NonemptyClassRanges :: ClassAtom NonemptyClassRangesNoDash

/**
 * 执行ClassAtom获取字符集A
 * 执行NonemptyClassRangesNoDash获取字符集B
 * 返回A与B联合成的字符集
 */
```

### 类原子范围

```javascript
NonemptyClassRanges :: ClassAtom - ClassAtom ClassRanges

/**
 * 执行第一个ClassAtom获取字符集A
 * 执行第二个ClassAtom获取字符集B
 * 执行ClassRanges获取字符集C
 * 只含单个字符的字符集A、B组成字符集范围D（Unicode码范围）
 * 返回D与C联合成的字符集
 */
```

## ClassAtom

### 连字符类原子

```javascript
ClassAtom :: -

/**
 * 返回只含连字符的字符集
 */
```

### 非连字符类原子

```javascript
ClassAtom :: ClassAtomNoDash

/**
 * 执行ClassAtomNoDash获取字符集，返回该字符集
 */
```

## ClassAtomNoDash

### 普通字符类原子

```javascript
ClassAtomNoDash :: SourceCharacter but not one of \ or ] or -

/**
 * 执行SourceCharacter获取普通字符，返回只含该字符的字符集
 */
```

### 转义类原子

```javascript
ClassAtomNoDash :: \ ClassEscape

/**
 * 执行ClassEscape获取字符集，返回该字符集
 */
```

## ClassEscape

### 十进制转义

```javascript
ClassEscape :: DecimalEscape

/**
 * 执行DecimalEscape获取结果E
 * E为<NUL>字符，返回只含该字符的字符集匹配
 * E不为<NUL>字符，抛出SyntaxError
 */
```

### 退格转义

```javascript
ClassEscape :: b

/**
 * 返回只含退格符的字符集
 */
```

### 字符转义

```javascript
ClassEscape :: CharacterEscape

/**
 * 执行CharacterEscape获取字符集，返回该字符集
 */
```

### 字符类转义

```javascript
ClassEscape :: CharacterClassEscape

/**
 * 执行CharacterClassEscape获取字符集，返回该字符集
 */
```

## Flags

匹配标志

* `g`: 全局匹配
* `i`: 忽略大小写
* `m`: 多行匹配
