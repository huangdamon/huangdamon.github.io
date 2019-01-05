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

## 语法

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

