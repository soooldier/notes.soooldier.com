>   *go version 1.14*
>
>   `runtime.LockOSThread` 绑定goroutine到当前的系统Thread，不被其他goroutine占用来达到优先调度的效果

```go
package main

import (
	"fmt"
	"os"
	"runtime"
	"time"
)

func main() {

	var ch = make(chan bool, 20000)
	var begin = make(chan bool)

	go func() {
		runtime.LockOSThread()
		<-begin
		fmt.Println("begin")
		tm := time.Now()
        // 假设只消费10000000条数据
		for i := 0; i < 10000000; i++ {
			<-ch
		}
		fmt.Println(time.Now().Sub(tm))
		os.Exit(0)
	}()

    // {{{ 创建其他goroutine来产生大量的调度
	for i := 0; i < 500; i++ {
		go func() {
			var count int
			for {
				count++
				if count >= 100000 {
					count = 0
					runtime.Gosched()
				}
			}
		}()
	}
    // }}}

	for i := 0; i < 20; i++ {
		go func() {
			for {
				ch <- true
			}
		}()
	}

	fmt.Println("all start")
	begin <- true

	fmt.Println("it is running...")
	select {}
}

```

以上程序模拟了一个生产-消费模型，开启20个goroutine产生任务，一个goroutine去消费【当然也可以多个goroutine消费】，在消费的代码中关闭`runtime.LockOSThread()`程序的耗时比打开要多数倍。





*   https://www.cnblogs.com/dearplain/p/8276138.html