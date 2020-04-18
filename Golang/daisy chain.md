```go
package main

import "fmt"

func main() {
	const n = 10
	leftmost := make(chan int)
	left := leftmost
	for i := 0; i < n; i++ {
		right := make(chan int)
		go f(left, right)
		left = right
		if i == n-1 {
			go func(c chan int) {
				c <- 1
			}(right)
		}
	}
    
	fmt.Println(<-leftmost)
}

func f(left, right chan int) {
	left <- 1 + <-right
}
```

