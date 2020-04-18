*   Method Sets：type T consists of all methods with receive type T ；type *T consists of all methods with receive type T and type *T
*   Calls：`x.m()`is valid if the method set of x contains m (x is value or pointer). If x is addressable and &x contains method m, it is also valid.

### Addressable

>   ⚠️ map 和 interface 不可寻址

### Example

1.  可寻址的`slice`

    ```go
    type List []int
    
    func (l List) Len() int        { return len(l) }
    func (l *List) Append(val int) { *l = append(*l, val) }
    
    func main() {
    	plst := new(List) // 指针
    	plst.Append(2)
    	fmt.Printf("%v (len: %d)\n", plst, plst.Len())
        
    	var lst List // 值
        lst.Append(1) // lst可寻址，相当于(&lst).Append(1)
    	fmt.Printf("%v (len: %d)\n", lst, lst.Len())
    }
    ```

2.  不可寻址的`map`

    ```go
    type List []int
    
    func (l List) Len() int        { return len(l) }
    func (l *List) Append(val int) { *l = append(*l, val) }
    
    lists := map[string]List{}
    lists["primes"].Append(7) // error!!!
    
    lists := map[string]*List{}
    lists["primes"] = new(List)
    lists["primes"].Append(7) // valid
    count := lists["primes"].Len() // valid
    ```

    

