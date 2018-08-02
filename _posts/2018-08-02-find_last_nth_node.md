---
layout: default-blog-md
title: 找到链表里的倒数第N个节点
category: markdown
---

如何找到链表里的倒数第N个节点？  
提供两种办法，办法一：
```go
// 双指针法：构造两个距离为N的指针，同时遍历链表。
// 当后一个指针到达末尾时，前一个指针就是倒数第N个节点。
func findLastNthNode(linklist *Node, nth int) *Node {
	var n1, n2 *Node
	n1 = linklist
	for i := 0 ; n1 != nil && i < nth; i++ {
		n1 = n1.next
	}
	if n1 == nil {
		return n1
	}
	n2 = linklist
	for n1 != nil {
		n1 = n1.next
		n2 = n2.next
	}
	return n2
}
``` 
办法二：
```go
// 间隔逼近法：每次遍历N个节点，并记录上一次遍历的起始节点L。
// 当遍历个数M小于N时，表示已经到达链表末尾，此时从L开始遍历M个节点，就是倒数第N个节点。
func findLastNthNode2(linklist *Node, nth int) *Node {
	var n1, n2, n2Head *Node
	n1 = linklist
	n2 = linklist
	firstNth := true
	for {
		var i int
		n2Head = n2
		for i = 0; n2 != nil && i < nth; i++ {
			n2 = n2.next
		}
		if i < nth {
			for j := 0; j < i; j++ {
				n1 = n1.next
			}
			return n1
		} else {
			if firstNth {
				firstNth = false
			} else {
				n1 = n2Head
			}
		}
	}
}
``` 
测试代码如下：
```go
package main

import "fmt"

type Node struct {
	value int
	next *Node
}

func printLinkList(linklist *Node) {
	for linklist != nil {
		fmt.Println(linklist.value)
		linklist = linklist.next
	}
}

func main() {
	testValue := []int{1,2,3,4,5}
	var linklist, head *Node
	for _, v := range testValue {
		node := &Node {
			value: v,
			next: nil,
		}
		if linklist == nil {
			linklist = node
			head = linklist
		} else {
			linklist.next = node
			linklist = linklist.next
		}
	}
	//printLinkList(head)
	fmt.Println(findLastNthNode(head, 2).value)
	fmt.Println(findLastNthNode2(head, 2).value)
}
``` 
运行地址点[这里](http://tpcg.io/UwsHZz
)。