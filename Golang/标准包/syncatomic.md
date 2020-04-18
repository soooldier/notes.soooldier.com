```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var n int32
	var wg sync.WaitGroup
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			n++
		}()
	}
	wg.Wait()
	fmt.Println("The version one result is:", n) // The version one result is: 877

	var m int32
	var wg1 sync.WaitGroup
	for i := 0; i < 1000; i++ {
		wg1.Add(1)
		go func() {
			defer wg1.Done()
			atomic.AddInt32(&m, 1)
		}()
	}
	wg1.Wait()
	fmt.Println("The version two result is:", m) // The version two result is: 1000
    
    // mutex版本相对atomic版本效率低一些，因为atomic是底层硬件实现，而mutex则是软件层面的实现
    var o int32
	var wg3 sync.WaitGroup
	var l sync.Mutex
	for i := 0; i < 1000; i++ {
		wg3.Add(1)
		go func() {
			defer wg3.Done()
			l.Lock()
			o++
			l.Unlock()
		}()
	}
	wg3.Wait()
	fmt.Println("The version three result is:", o) // The version three result is: 1000
}
```

