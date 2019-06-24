# 类型转换

标签（空格分隔）： JavaScript语法

---

系统运行时会在需要的时候对语言类型进行自动类型转换

## ToPrimitive(input, PreferredType)

转换为原始值类型

| 输入类型 | 结果 |
| --- | --- |
| Undefined | input |
| Null | input |
| Boolean | input |
| Number | input |
| String | input |
| Object | input.[[DefaultValue]] (preferredType) |

对象转换为Number类型

* 调用`valueOf()`方法
  * 若返回值为原始值类型，则自动转换为Number类型
  * 若返回值不为原始值类型，则继续调用`toString()`方法
    * 若返回值为原始值类型，则自动转换为Number类型
    * 若返回值不为原始值类型，则报错

对象转换为String类型
