---
title: JavaScript 高级程序设计（第三版）读书笔记
categories: notebook
tags:
	- javascript
	- notebook
---
# JavaScript 高级程序设计（第三版）读书笔记

> 仅摘录和补充个人所需，并不全面的总结本书，如需了解整本书的内容，请购买相应书籍

## 第二章 在HTML中使用JavaScript

###  script元素

1. 属性

	- **async**（是否立即下载），**charset**（字符集，浏览器大部分忽略）， **language**（已废弃），**src**（执行代码文件），**defer**（延迟执行） 以上五个属性，平常使用过程中，很少会接触，同时也不是特别重要。

	- **type**（MIME类型）：表示我们编写代码使用的脚本语言的内容类型

  > 知识扩展
  >
  > 什么是 [MIME](https://en.wikipedia.org/wiki/MIME) ?  Multipurpose Internet Mail Extensions ,字面上了解就是 多功能的网络邮件扩展
  >
  > 简单理解就是，特定扩展名对应相应类型的MIME

书本中详细的介绍了这6个属性的相关用法，但是由于很少使用到，所以不再做详细解释。请Google或者Baidu查找相关资料。

## 第三章 基本概念

> 忽略小章节： 3.3 变量    3.4.6 String

### 语法

- 区分大小写
- 标识符： 字符，下划线(_)，美元符($)，数字  （**第一个不能是数字**）
  - 驼峰大小写格式（推荐） : helloWorld
  - 注释： /***/   或者  //

### 数据类型

- typeof (重要：返回值是**字符串**)

| typeof {} or                                                                                        | 结果        |
|-----------------------------------------------------------------------------------------------------|-------------|
| typeof undefined <br />typeof 一个声明但未初始化的变量<br />typeof 一个未声明的变量<br />typeof NAN | “undefined“ |
| typeof  true<br />typeof false                                                                      | “boolean“   |
| typeof "hello"<br />typeof 'world'<br />typeof "1234"                                               | “string“    |
| typeof 1234                                                                                         | “number“    |
| typeof {}<br />**typeof []**<br />**typeof null**                                                   | “object“    |
| typeof function(){}<br />typeof Number<br />typeof String                                           | “function“  |

#### Null

null值表示一个空对象指针，而这也表明了typeof的值是一个”object“的原因

> 知识扩展
>
> null 和 undefined的区别 ？
>
> `null` is an **assignment** value. It can be assigned to a variable as a representation of no value
>
> 用来表示当前变量指向一个空的对象
>
> `undefined` means a variable has been declared but has not yet been assigned a value
>
> 用来表示一个定义却未初始化的变量

#### Boolean

只要记住这些false的转换规则

| 数据类型  | False     |
| --------- | --------- |
| Number    | 0和NAN    |
| String    | ""        |
| Object    | null      |
| Undefined | undefined |

#### Number

> Javascript中所有的数字都是以IEEE-754标准格式表示的，因为浮点数的表示都是不精确
> 因此进行浮点数运算时，还是选择一个靠谱的第三方库。
>
> reference [：bignumber.js](https://github.com/MikeMcl/bignumber.js) and     [big.js](https://github.com/MikeMcl/big.js)

1. 数值范围
```JavaScript
Infinity    // 正无穷
Infinity    // 负无穷
```
2. NAN (一个本来用于返回数值的操作数未返回数值的情况)
```JavaScript
0 / 0 === NaN
1 / 0 === Infinity
1 / 0 === -Infinity
```
3. parseInt()函数关注的以下几个案例
```JavaScript
parseInt("124abc")   /// 124
parseInt("")         /// NaN
```

#### Object

Object实例的属性和方法，一共7个(只列重点关注)：
```JavaScript
Object.HasOwnProperty(proName)         /// 属性是否在实例对象中
Object.isPrototypeOf(object)           /// 判断是否对象原型
Object.propertyIsEnumerable(proName)   /// 是否支持for-in语句
```

### 操作符

#### 位操作符
```JavaScript
~   /// 非
|   /// 或
&   /// 与
^   /// 异或
```

> ❗ 知识扩展
> ```JavaScript
> ~(A & B) === ~A | ~B     /// 与非, 含义：有0 出 1，全 1 出 0
> ~(A | B) === ~A & ~ B    /// 或非, 含义：有1 出 0，全 0 出 1
> (~A & B) | (A & ~B)      /// 异或, 含义：相同 0，不同 1
> ```

#### 布尔操作符
```JavaScript
!   /// 非
||  /// 或
&&  /// 与
```

下面列举有关非布尔值的操作结果：

- **非运算**，无论什么值，都会⭐<u>**一定**</u>返回一个布尔值：

  ```javascript
  var obj1 = {}
  - !obj1             			           /// false
  - !""                                       /// true
  - !"hello"                                  /// false
  - !null  !undefined  !NaN                   /// true
  ```



- **与运算**，和非布尔值操作⭐<u>**不一定**</u>返回布尔值：

  ```javascript
  var obj1 = {}
  var obj2 = {}

  - obj1 && "Hello world"                     /// return "Hello world"  第一个是对象，返回第二个
  - false && obj1                             /// false
  - true && obj1                              /// {} 第一个为true，返回第二个值
  - obj1 && obj2                              /// obj2 两个都是对象，返回第二个
  - NaN && "hello world"                      /// NaN
  - "hello world" && NaN                      /// NaN
  - undefined && "hello world"                /// undefined
  - "helllo world" && undefined               /// undefined
  ```

- 或运算，和非布尔值操作<u>**不一定**</u>返回布尔值：

  ```javascript
  var obj1 = {}
  var obj2 = {}

  obj1 || 'hello world'                       /// obj1
  false || 'hello world'                      /// 'hello world' 第一个为false,返回第二个值
  obj1 || obj2                                /// obj1  都为对象，返回第一个
  ```

> 乘性操作符, 加性操作符, 关系操作符, 条件操作符, 赋值操作符, 逗号操作符 不再讨论, 后期有需要再补充

#### 相等操作符

特殊的比较结果：

```javascript
null == undefined                              ///  true
true == 1                                      ///  true

NaN == NaN                                     ///  false
true == 2                                      ///  false
undefined == 0                                 ///  false
null == 0                                      ///  false
```

### 语句

> 由于语句相对于稍微有经验的程序员来说，都是比较熟悉，因此就不做太多详细的解析，只是列举相关的表达式

待续
