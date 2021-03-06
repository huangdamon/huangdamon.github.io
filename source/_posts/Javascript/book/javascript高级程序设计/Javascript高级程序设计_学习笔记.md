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

  - 📍`async`（是否立即下载），📍`charset`（字符集，浏览器大部分忽略）， 📍`language`（已废弃），📍`src`（执行代码文件），📍`defer`（延迟执行） 以上五个属性，平常使用过程中，很少会接触，同时也不是特别重要。
  - `async`和`defer`不会阻塞DOM的加载渲染(⚠️延迟脚本放在页面底部仍然是最好的选择)
      - `defer`适用于外部的脚本，按顺序执行，并且会在`DOMContentLoaded`和`load`事件之前⚠️现实中，延迟脚本不一定按顺序执行，也不一定在`DOMContentLoaded`之前
      - `async`适用于外联脚本，无执行顺序，在`load`前，但是不能保证与`DOMContentLoaded`之间的顺序
  - ⭐`type`（MIME类型）：表示我们编写代码使用的脚本语言的内容类型

  >
  > > ⚠️知识扩展
  > > > 什么是 [MIME](https://en.wikipedia.org/wiki/MIME) ?  Multipurpose Internet Mail Extensions ,字面上了解就是 多功能的网络邮件扩展
  > > > 简单理解就是，特定扩展名对应相应类型的MIME

书本中详细的介绍了这6个属性的相关用法，但是由于很少使用到，所以不再做详细解释。请`Google`或者`Baidu`查找相关资料。

## 第三章 基本概念

> 忽略小章节： 3.3 变量    3.4.6 String

### 语法

- 区分大小写
- 标识符： 字符，下划线(`_`)，美元符(`$`)，数字  （❗❗❗ 第一个不能是数字）
  - 驼峰大小写格式（推荐） : helloWorld
  - 注释： `/***/`   或者  `//`

### 数据类型

- `typeof` (重要：返回值是**字符串**)

|                                                              | 结果        |
| ------------------------------------------------------------ | ----------- |
| `typeof undefined `<br />`typeof a /// a声明但未初始化`<br />`typeof a ///a未声明`<br />`typeof NAN` | “undefined“ |
| `typeof  true`<br />`typeof false`                           | “boolean“   |
| `typeof "hello"`<br />`typeof 'world'`<br />`typeof "1234"`  | “string“    |
| `typeof 1234`                                                | “number“    |
| `typeof {}`<br />⭐`typeof []` <br />⭐`typeof null`           | “object“    |
| `typeof function(){}`<br />`typeof Number`<br />`typeof String` | “function“  |

#### Null

`null`值表示一个空对象指针，而这也表明了`typeof`的值是一个”object“的原因

> ⚠️知识扩展
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

只要记住这些false的转换规则，否则就是true

| 数据类型  | False     |
| --------- | --------- |
| `Number`    | `0`和`NAN`    |
| `String`    | `""`       |
| `Object`    | `null`      |
| `Undefined` | `undefined` |

#### Number

> Javascript中所有的数字都是以IEEE-754标准格式表示的，因为浮点数的表示都是不精确
> 因此进行浮点数运算时，还是选择一个靠谱的第三方库。
>
> reference： [bignumber.js](https://github.com/MikeMcl/bignumber.js) and [big.js](https://github.com/MikeMcl/big.js)

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

`Object`实例的属性和方法，一共7个(只列重点关注)：
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

> ⚠️ 知识扩展
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

- **非运算**，无论什么值，⭐<u>**一定**</u>返回一个布尔值：

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
####条件，循环
```JavaScript
if(condition) statementOne else statementTwo
do {statement} while (condition)
while(expression) statement
for(initialization; expression; post-loop-expression) statement
for(proterty in expression) statement
```
重点讲解下 `for in` 语句：
- 如果 expression是`undefined`或者`null`,会抛出错误(❗❗❗ ECMA5更正这个行为)

```JavaScript
break       /// 退出整个循环
continue    /// 退出当前循环，从顶部继续循环

switch(expression){
	case value: statement break;
	default: statement
}
```
### 函数

1. 理解参数
	- 传入参数 <= 定义参数（参数内部是用一个数组来表示）
	- `arguments` 读取到传入的参数
2. 无重载

## 第四章 作用域和内存问题
#### 基本类型和引用类型
>值类型的操作，就是保存在变量里面真实的值<br>引用类型的操作, 操作都是保存在内存中的对象

1. 传递参数 (❗❗❗ ECMAScript 中所有函数的参数都是**按值**传递,对于引用类型，这里的值是指**栈区的值**)
	- ⭐错误理解：在局部作用域中修改对象会在全局作用域反映出来，就说明参数是按引用传递的。
```JavaScript
//// 最终结果是，name并没有被修改为“Greg”.当在函数内重写obj对象时，这个变量引用的就是一个局部变量
function setName(obj) {
    obj.name = "hello wolrd";
    obj = new Object();
    obj.name = "Greg";
}

var person = new Object();
setName(person);
console.log(person.name)
```
2. **typeof** 用于检测基本数据类型是非常有效，但是检测引用类型时就用处不大。

#### 执行环境和作用域

>执行环境：定义了变量或者函数有权访问其他数据
>
>作用域链：保证对执行环境有权访问的所有变量和函数的有序访问

1. 作用域链
	-	允许向上搜索作用域链，不允许向下搜索

2.作用域
  - 没有块级作用域
  - 声明变量需注意：如果没有var声明一个变量，那么其作用域就会升级到全局变量（严格模式下会导致错误）

## 第五章 引用类型
 > 引用类型： 是一种数据结构，用于将数据和功能组织在一起
### Object
 1. 创建对象两种方式
 ```JavaScript
 var obj1 = new Object()   /// new 对象的方式

 var obj2 = {              /// 字面量的方式
	 name: "huangda"
 }
 ```
 2. 调用对象的方式
 ```javascript
 obj.name
 obj['name huangda']   //当字符是常量字符或者存在空格时，这个才能访问
 ```
### Array
> 数组自带的各种方法的具体使用，我就不在这里详细介绍，只列举比较重要的部分。<br>
> `Lodash`能解决大部分问题

1. slice()方法
  	> 基于当前数组中的一个或者多个项创建一个⭐新数组
2. splice()方法

    > 向数组的中部插入项
3. 迭代方法
   ```javascript
    	var nums = [1, 2, 3]
		/// 所有返回true，结果为true
		nums.every((item, index, array) => {})
		/// 存在一个true，结果为true
		nums.some((item, index, array) => {})
		/// 返回结果是一个数组
		nums.map((item, index, array) => {})
	```

4. 归并方法
	```javascript
		var nums = [1, 2, 3]
		/// 左边开始归并
		nums.reduce((pre, cur, index, array) => {})
		/// 右边开始归并
		nums.reduceRight((pre, cur, index, array) => {})
	```
### 正则表达式 RegExp类型
> 不过多介绍正则匹配，会在其他文章中，做更详细的介绍<br>
> flags : `g`: 是全局模式   `i`: 忽略大小写  `m`: 多行模式
```javascript
	var expression = / pattern / flags
```
1. 实例方法
  - exec(text) 执行匹配
  - test(text) 判断是否匹配，返回⭐布尔值

  ### Function 类型

1. `函数名`其实就是指向函数对象的指针(函数是对象，函数名是指针)
2. 函数没有重载

```javascript
function addNumber(num) {
    return num + 100;
}

function addNumber(num) {
    return num + 200;
}

/*
 因为函数名是指针，因为最后函数名addNumber指向了第二个函数
*/
```

## 第六章 面向对象的程序设计

ECMA5中，定义了两种属性，一个是`数据属性`，另一个是`访问其属性`

1. 数据属性
   1. `Configuration `  
      1. 能否detele属性来重新定义
      2. 能否修改属性的特性
      3. 能否把属性修改为访问器属性
   2. `Enumerable`
      1. 是否能 for-in
   3. `Writable`
      1. 是否能修改
   4. `Value`
      1. 包含这个属性的数据值

```javascript
let person = {};
Object.defineProperty(person, "name", {
   writable: false,
   value: "Nicholas"
});

person.name = "Damon";
console.log(person.name)  //// Nicholas
```

2. 访问器属性
   1. `Configuration` 与数据属性一样
   2. `Enumerable` 与数据属性一样
   3. `Get` 读取属性时调用
   4. `Set ` 写入属性时调用

```javascript
let person = {
    name: "Damon",
    age: 12
};

Object.defineProperty(person, "name", {
   get: function() {
       return this.name
   },
   set: function(newValue) {
       this.name = newValue;
       this.age = 13;
   }
});

person.name = "huangda";
console.log(person.age); ////13
```

### 创建对象

1. 工厂模式

   ```javascript
   function createPerson() {
       let obj = new Object();
       obj.name = "Damon";
       return obj;
   }
   ```

2. 构造函数

   ```javascript
   function Person() {
       this.name = "Damon";
   }
   ```

⚠️注意：

1. 只要通过`new`操作符来调用，那他就可以作为构造函数，否则和普通函数没什么区别。

### 继承

❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ ❗ 待续

⚠️注意：

1. 当为对象实例添加属性时，会❗ `屏蔽`原型对象上保存的同名属性,如果想要继续拿到原型对象上的同名属性值，可以通过`delete`对象实例上的属性。
2. HasOwnProperty() 判断属性是否存在对象实例上。
3. 由于在原型中搜索时一次搜索，因为在实例后给原型添加属性也是没问题，实例还是可以访问这个属性。
4. 

## 第七章 函数表达式

定义函数的两种方式：**函数声明**和**函数表达式**

```javascript
// 函数声明
// 1. 函数声明提升
function functionName() {}

// 函数表达式
let functionName = function(){}   // 匿名函数（lamda函数）
```

在处理递归函数时，显然使用 **arguments.callee** 来解决函数指向其他变量空间导致错误

```javascript
function factorial(num){
    if(num <= 1){
        return 1;
    }
    return num * factorial(num - 1);   // return num * arguments.callee(num - 1)
}

//但是严格模式下并不生效，所以要采用命名函数表达式
var factorial = (function f(num) {
    if(num <= 1){
        return 1;
    }
    return num * f(num - 1); 
})

```

闭包在我的理解，就是一个函数包着另外一个函数，并且里面的函数有访问外部函数的权利。

⭐值得注意的是，闭包只能取得包含函数中任何变量的最后一个值

```javascript
function create() {
    let result = new Array();
    for(let i = 0; i < 3; i++){
        result[i] = function() {
            return i;
        }
    }
    return result;
}

// 结果都是2，想达到预期，则需要另外一个闭包
function create() {
    let result = new Array();
    for(let i = 0; i < 3; i++){
        result[i] = (function(i) {
            return i;
        })(i)
    }
    return result;
}
```

⚠️注意：

匿名函数的this并没有绑定到任何一个对象上，因此是指向window，内部函数在搜索this和argument时，只会搜索到其活动对象为止。

匿名函数： https://www.cnblogs.com/pssp/p/5216668.html

### 内存泄漏问题

循环引用，下面例子中，element的onclick事件引用了element

```javascript
function handle() {
    let element = document.getElementById('id');
    element.onclick = function() {
        alert(element.id);
    };
}

// 以下处理方法还是不行，虽然没有直接引用element对象，但是别忘记，闭包包含函数的整个活动对象。
function handle() {
    let element = document.getElementById('id');
    let id = element.id;
    element.onclick = function() {
        alert(id);
    };
}

// best
function handle() {
    let element = document.getElementById('id');
    let id = element.id;
    element.onclick = function() {
        alert(id);
    };
    element = null;
}
```

​	

​																																																																																																																																																																																									

