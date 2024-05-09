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

## 练习循环函数
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

## 数组和切片 ，定义数组和定义切片的四种方式
### 定义数组
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
}
```

