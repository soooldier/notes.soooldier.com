`unsafe.Pointer` 可以绕过GO的语言类型系统检查，任意底层类型的指针类型都可以以`unsafe.Pointer`作为桥梁进行转换。也就是说如果两种类型具有相同的内存结构，那么我们就可以使用`unsafe.Pointer`来进行转换从而避免内存开销。也就是说让同一份内存有两种不同的解读方式。以`[]byte`和`string`举例来说：

```go
package main

import "fmt"

import "unsafe"

func main() {
	b := []byte{104, 101, 108, 108, 111}
	p := unsafe.Pointer(&b)
	fmt.Println(*(*string)(p)) // 输出hello
}

```

