### panic ???

```go
package main

import (
	"fmt"
)

func main() {
	var i *int
	*i = 10
	fmt.Println(*i)
}

```

```shell
panic: runtime error: invalid memory address or nil pointer dereference
```

声明了引用类型的变量`i`，但默认没有分配内存空间，因此报错；而对于值类型的声明`var i int`，已经默认分配内存空间并使用zero value进行了初始化。