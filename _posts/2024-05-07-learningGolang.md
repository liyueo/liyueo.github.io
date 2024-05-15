---
layout:     post
title:      学习go
subtitle:   LearningGolang-20240425
date:       2024-05-08
author:     BY
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - go
    - go
---

## 1. GO项目的结构

```
myproject/
├── main.go        // 顶级包的入口文件，包含程序的入口函数 func main()
├── pkg1/
│   ├── util.go    // 包 pkg1 的实现文件
│   └── helper.go  // 包 pkg1 的实现文件
├── pkg2/
│   ├── lib.go     // 包 pkg2 的实现文件
│   └── tools.go   // 包 pkg2 的实现文件
└── README.md      // 项目文档或说明文件
```

**一个项目下可以有很多包，，每个包实现自己的功能，，每个文件都可以由自己的main()函数，，除了顶级包package main中的main函数之外，，其他的main函数只作测试用，
package的main函数作为整个项目的入口**

```go
// main.go
package main

import (
    "fmt"
    "myproject/pkg1"
    "myproject/pkg2"
)

func main() {
    // 调用 pkg1 中的函数
    result1 := pkg1.UtilFunction()
    fmt.Println("Result from pkg1:", result1)

    // 调用 pkg2 中的函数
    result2 := pkg2.LibFunction()
    fmt.Println("Result from pkg2:", result2)
}

```

## 2. 练习循环函数
牛顿法求Sqrt
```go
package main

import(
	"fmt"
	"math"
)

func f (x float64) {
	z := float64(1)
	for i := 0; i < 10; i++ {
		z -= (z * z - x) / (2 * z)
		fmt.Println(z)
	}
	fmt.Println("正确的：", math.Sqrt(x))
}

func main(){
	x := float64(178)
	f(x)
}
```
输出:
```
89.5
45.74441340782123
24.817799474038544
15.995035570419864
13.561744235858818
13.34344979615119
13.34166418361713
13.341664064126334
13.341664064126334
13.341664064126334
正确的： 13.341664064126334
```

**延伸**
加上错误判断：
```go
type error interface {
    Error() string
}
```
go语言的错误接口
```
package main

import (
	"fmt"
)

type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string {
	if e < 0 {
		return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
	}
	return ""
}

func Sqrt(x float64) (float64, error) {
	if x >= 0 {
		z := float64(1)
		for i := 0; i < 10; i++ {
			z -= (z*z - float64(x)) / 2 * z
		}
		return z, nil
	} else {
		return 0.0, ErrNegativeSqrt(x) // 若是错误，，将它转换成错误类型的float64
	}
}

func main() {
	// fmt.Println(Sqrt(2))
	// fmt.Println(Sqrt(-2))
	if result, err := Sqrt(2); err == nil {
		fmt.Println(result)
	} else {
		fmt.Println(err)
	}

	if result, err := Sqrt(-2); err == nil {
		fmt.Println(result)
	} else {
		fmt.Println(err)
	}
}

```


## 3. 数组和切片 ，定义数组和定义切片的四种方式
### 3.1 定义数组
```go
package main
 
import "fmt"
 
func main() {
    // 数组定义 第一种方式
	var arr1 [3]int
	var strArr [2]string
 
	arr1[0] = 1
	arr1[1] = 213
	arr1[2] = 2
 
	strArr[0] = "php"
	strArr[1] = "golang"
	fmt.Println(arr1)   // [1 213 2]
	fmt.Println(strArr) // [php golang]
 
	// 第二种方式
	var arr2 = [3]int{1, 22, 32}
	fmt.Println(arr2) // [1 22 32]
 
	// 第三种方式 不再输入数组数量，让程序自动判断
	var arr3 = [...]int{21, 21, 321, 432, 32}
	fmt.Printf("数组为：%v 长度为：%v\n", arr3, len(arr3)) // 数组为：[21 21 321 432 32] 长度为：5
 
	// 第四种方式 指定索引值
	var arr4 = [...]int{0: 12, 1: 1, 3: 234}
	fmt.Printf("数组为：%v 长度为：%v\n", arr4, len(arr4)) // 数组为：[12 1 0 234] 长度为：4，也就是3-1
}
```

### 定义切片
```go
package main
 
import "fmt"
 
func main() {
    // 定义切片 第一种方式
	var a []int
	fmt.Printf("值：%v 类型：%T\n", a, a) // 值：[] 类型：[]int
 
	// 第二种方式
	var b = []int{1, 2, 3}
	fmt.Printf("值：%v 类型：%T\n", b, b) // 值：[1 2 3] 类型：[]int
 
	// 第三种方式，基于数组声明切片
	var c = [4]string{"php", "java", "nodejs", "golang"}
	d := c[:]                        // [:]获取数组中所有的值
	fmt.Printf("值：%v 类型：%T\n", d, d) // 值：[php java nodejs golang] 类型：[]string
 
	e := c[1:3]    // 获取key从1开始到3结束，但又不包含3的值
	fmt.Println(e) // [java nodejs]
 
	f := c[1:]     // 获取key从1开始到末位的值
	fmt.Println(f) // [java nodejs golang]
 
	g := c[:3]     // 获取key从3到开始的值，但又不包含3的值
	fmt.Println(g) // [php java nodejs]
 
	// 第四种方式，基于切片声明切片.跟第三种方式类似，不再一一展示。
	var h = []int{1, 2, 3, 4}
	i := h[:]
	fmt.Println(i) //[1 2 3 4]
 
	// 空的切片默认值为 nil
	var j []string
	fmt.Println(j == nil) // true

	// 第五种方式：使用make创建
	slice1 = make([]int, 5, 10) // len = 5, cap = 10
}
```

## 4. 函数闭包
<a href="https://blog.csdn.net/weixin_43586120/article/details/89456183"> 闭包看这个，全！！</a>
闭包函数：声明在一个函数中的函数，叫做闭包函数。
闭包：内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后。
可能会造成内存泄漏

```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```
使用函数闭包实现一个 斐波那契数列

```go
package main

import "fmt"

// fibonacci 是返回一个「返回一个 int 的函数」的函数
func fibonacci() func() int {
	a, b := 0, 1
	return func() int {
		result := a
		a, b = b, a+b
		return result
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

## 5. Stringer接口
Stringer接口时fmt包中最常用的接口，，它的定义如下;
```go
type Stringer interface {
    String() string
}
```
自己定义的结构体可以去实现这个接口的方法String（），那么在输出fmt.Println()结构体的时候会输出这个方法的返回值。

```go
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```

```go
return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
```
Springf可以用来格式化语句，其他方方法：：
```go
// 1.
result := "My name is " + p.name + ", and I am " + fmt.Sprintf("%d", p.age) + " years old."
return result

// 2.
ageStr := "" + p.age // 将整数 p.age 转换为字符串
result := "My name is " + p.name + ", and I am " + ageStr + " years old."
return result

// 3.
// 使用 strconv.Itoa 将整数转换为字符串
ageStr := strconv.Itoa(p.age)
result := "My name is " + p.name + ", and I am " + ageStr + " years old."
return result

```
第三种方法 Itoa ，，，c++中有 atoi  itoa

常用的函数有：

> strconv.Atoi()：用于将字符串转换为整数 (int)。
> strconv.ParseInt()：用于将字符串转换为指定位数的整数 (int64)。
> strconv.ParseUint()：用于将字符串转换为无符号整数 (uint64)。
