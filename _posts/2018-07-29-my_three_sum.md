---
layout: default-blog-md
title: 自己动手写ThreeSum
category: markdown
---

编码解决ThreeSum问题，算法复杂度为O（N<sup>2</sup>），实现如下：  
```go
package main

import "fmt"

func calcThreeSum(array []int) {
	thirdReversed := make(map[int]int)
	for i, item := range array {
		thirdReversed[-item] = i
	}
	calcedTwo := make(map[int]bool)
	calcedThree := make(map[int]bool)
	for i := range array {
		for j := range array {
			if i == j {
				continue
			}
			firstTwo := array[i] + array[j]
			if _, ok := calcedTwo[firstTwo]; ok {
				continue
			}
			if idx, ok := thirdReversed[firstTwo]; ok {
				if _, ok := calcedThree[i+j+idx]; !ok {
					fmt.Println( fmt.Sprintf("%d[%d], %d[%d], %d[%d]", array[i], i, array[j], j, -firstTwo, idx))
					calcedThree[i+j+idx] = true
				}
			}
			calcedTwo[firstTwo] = true
		}
	}
}

func main() {
	testData := [][]int {
		{1,2,3,4,-7,-3},
		{10, -10, 10, -30, 0, 20},
	}
	for i, data := range testData {
		fmt.Println("test", i)
		calcThreeSum(data)
	}
}

``` 
运行地址点[这里](http://tpcg.io/hu1kuX)。