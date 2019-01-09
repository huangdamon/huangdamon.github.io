---
title: Go语言编程-学习笔记
categories: notebook
tags:

 - golang
 - notebook
---

# Go语言编程 -学习笔记

## 入门

### 正常的工程目录组织

project

------​	src

-------------calc

-------------------calc,go

-------------simplemath

-------------------add.go

-------------------sqrt.go

------	bin

------	pkg

```go
// calc.go
import (
	"fmt"
)
func main() {
    fmt.Println("hello world")
}
```

```shell
cd /bin
go build calc
```

#### 语法

### 变量

#### 变量声明

```go
var a int         /// 单纯的声明
var b int = 2     /// 声明并初始化
var c = 2         /// 声明并初始化，不用指定类型，编译器可以根据右边自动判断
d := 2            /// 使用 := 声明并初始化，连var都不需要
```

#### 匿名变量

```go
func getName() {
    return "Hello", "world"
}
_, world = getName()
```

#### 常量

可以是`常量表达式`，并且时`编译期`的运算，不能有任何`运行期`的行为

`iota` 在每一个`const`关键字出现时被重置为0

```go
const Pi = 3.14
const mask = 1 << 3

const (
	c0 = iota   // 0
    c1 = iota   // 1
)
const （
	a1 = iota   // 0
）
```

### 类型

#### 布尔类型

关键字`bool`,不支持自动或者强制的类型转换

```go
var b = bool
b = 1       // 错误
b = bool(1) // 错误
```

#### 整型

1. `int` 和`int32`是两种不一样的类型，因为不会做自动转换，可以强制转换。
2. 不同类型的整形数不能直接比较，但是可以和字面量比较。

#### 浮点型

`float32` 和 `float64` 

浮点数比较可以使用`math`包

```javascript
import “math”
```

#### 字符串

可以用 <u>*数组下标*</u> 访问，但是一旦初始化就*<u>不能</u>* 被修改

如果想保存非ANSI字符，必须保存文件编码格式为UTF-8

字节数组的方式遍历

```go
// 长度是13，因为中嗯在UTF-8占了3个字节
str := "Hello 世界"
for i := 0; i < len(str); i++ {
    ch := str[i]
    fmt.Println(i, ch)
}

for i, ch := range str {
    fmt.Println(i, ch)
}
```

#### 数组

数组是值类型，因为值类型变量在赋值和作为参数都是产生一次复制动作

```go
for i, v := range array {
}

for i := 0; i < len(array); i++ {
} 
```

#### 数组切片

数组切片就是一个指向数组的指针，可以动态扩容，并且传递不会被重复复制。

```go
var myArray [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
var mySlice []int = myArray[:5]
mySlice1 := make([]int, 5)

mySlice = append(mySlice, 1, 2, 3) 
```

#### map

```go
var myMap map[string] string
myMap = make(map[string] string)
```

