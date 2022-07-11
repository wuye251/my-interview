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



参考：

- [《Go程序员面试笔试宝典》](https://golang.design/go-questions/)