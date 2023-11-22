> 多个协程之间修改同一个变量  会导致意外结果

1. sync中的waitgroup

```go
package main

import (
	"fmt"
	"sync"
)
func main() {
	var wg sync.WaitGroup
	a := 1

	go func() {
		a = 2
		wg.Done()
	}()

	wg.Add(1)
	a = 3
	wg.Wait()
	fmt.Println("a ===", a)
}
```

2. sync.mutex/rwmutex加锁

参考： https://www.bilibili.com/video/BV1jJ411c7s3?p=120&vd_source=f53bb49fb78a32947a9360dd16a1cf58

https://www.bilibili.com/video/BV1jJ411c7s3?p=121&vd_source=f53bb49fb78a32947a9360dd16a1cf58

2. channel