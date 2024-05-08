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

