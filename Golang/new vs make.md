### panic

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

###new

通过内置函数`func new(Type) *Type`手动分配内存空间解决panic问题

```go
package main

import (
	"fmt"
)

func main() {
	var i *int
    i = new(int) // 分配内存空间
	*i = 10
	fmt.Println(*i)
}
```

### make

>   Go has two allocation primitives, the built-in functions `new` and `make`

*   `make`只用于`slice`,`map`,`channel`三种数据结构
*   不返回指针类型
*   返回初始化值，而不是零值**returns an *initialized* (not *zeroed*) value of type `T` (not `*T`)**，`new`则是返回零值，对这三种数据结构来说零值都是`nil`

### examples

1.  map赋值

    ```go
    // 错误1, panic: assignment to entry in nil map
    var m map[string]string
    m["a"] = "a"
    
    // 错误2, assignment to entry in nil map
    mp := new(map[string]string)
    (*mp)["a"] = "a"
    
    // 正确
    m := make(map[string]string)
    m["a"] = "a"
    ```

