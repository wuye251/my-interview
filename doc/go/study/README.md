# 基础知识

## 数组与切片

### 切片基本语法

1. Slice[1:3] （截取范围为 索引[1,3)范围）

### [异同](https://golang.design/go-questions/slice/vs-array/)

1. 数组空间大小是定长，切片是动态分配的可扩展的
2. 数组就是分配一片连续的内存空间,  slice是一个结构体, 包含：长度，容量，底层数组

```golang
// src/runtime/slice.go
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

> 标题地址中有个练习题， 没看明白， 打个标记

###  [切片扩容](https://golang.design/go-questions/slice/grow/)

扩容都是发生在append时,   通过策略尽可能少的减少扩容产生的内存开辟回收操作，提高效率

> 当原slice容量(oldcap)小于256的时候，新slice(newcap)容量为原来的2倍；原slice容量超过256，新slice容量newcap = oldcap+(oldcap+3*256)/4

```go
//go1.18 src/runtime/slice.go +193
  ...
  ...
	newcap := old.cap
	doublecap := newcap + newcap //两倍扩容
	if cap > doublecap {
		newcap = cap
	} else {
		const threshold = 256 //默认阈值
		if old.cap < threshold { //没到阈值 则两倍扩容
			newcap = doublecap
		} else {
			// Check 0 < newcap to detect overflow
			// and prevent an infinite loop.
			for 0 < newcap && newcap < cap {  
				// Transition from growing 2x for small slices
				// to growing 1.25x for large slices. This formula 
				// gives a smooth-ish transition between the two.
				newcap += (newcap + 3*threshold) / 4  //如果到了阈值 则是当前容量+3*阈值256 / 4 
			}
			// Set newcap to the requested cap when
			// the newcap calculation overflowed.
			if newcap <= 0 {
				newcap = cap
			}
		}
	}
```

> - [ ] slice的扩容策略 ？  内存对齐是什么？
> - [ ] 

### 切片作为参数传递

1. 函数中， slice仅传递的是其拷贝副本， 所以在函数中修改，对于实参slice是不生效的， 除非传地址指针

```golang
package main

import "fmt"

func myAppend(s []int) []int {
	// 这里 s 虽然改变了，但并不会影响外层函数的 s
	s = append(s, 100)
	return s
}

func myAppendPtr(s *[]int) {
	// 会改变外层 s 本身
	*s = append(*s, 100)
	return
}

func main() {
	s := []int{1, 1, 1}
	newS := myAppend(s)

	fmt.Println(s)
	fmt.Println(newS)

	s = newS

	myAppendPtr(&s)
	fmt.Println(s)
}

/*
【output】
[1 1 1]
[1 1 1 100]
[1 1 1 100 100]
*/
```



# 并发相关

## goroutine和线程，进程的区别

> 线程包含： 用户态资源， 内核态资源， 每个资源中包含了为这个进程任务所必须的资源、开辟的空间地址，上下文等信息
>
> 协程goroutine： 仅有用户态资源， 对于线程少了内核态资源， 故而一个协程的大小是小于线程的， 在切换/开辟/销毁操作，代价比线程更小

## channel

> 使多个协程goroutine之间可以进行通信

- 无缓冲管道
- 有缓冲管道



参考：

- [《Go程序员面试笔试宝典》](https://golang.design/go-questions/)