```go
package main

import (
	"bytes"
	"flag"
	"fmt"
	"io/ioutil"
	"log"
	"os"
	"path/filepath"
	"strconv"
	"strings"
	"sync/atomic"
	"time"

	"golang.org/x/net/context"
	"golang.org/x/sync/errgroup"
)

// https://github.com/go-kratos/kratos/blob/master/pkg/sync/errgroup/errgroup.go
// go run main.go -timeout=20ms /Users/tangyi3/Projects/php/weibo/v2.api.task.weibo.com test
func main() {
	duration := flag.Duration("timeout", 500*time.Millisecond, "timeout in milliseconds")
	flag.Usage = func() {
		fmt.Printf("%s by Brian Ketelsen\n", os.Args[0])
		fmt.Println("Usage:")
		fmt.Printf("    gogrep [flags] path pattern \n")
		fmt.Println("Flags:")
		flag.PrintDefaults()
	}
	flag.Parse()
	if flag.NArg() != 2 {
		flag.Usage()
		os.Exit(-1)
	}
	path := flag.Arg(0)
	pattern := flag.Arg(1)
	ctx, _ := context.WithTimeout(context.Background(), *duration)
	m, err := search(ctx, path, pattern)
	if err != nil {
		log.Println(err)
	}
	for _, name := range m {
		fmt.Println(name)
	}
	fmt.Println(len(m), "hits")
}
func search(ctx context.Context, root string, pattern string) ([]string, error) {
	g, ctx := errgroup.WithContext(ctx)
	paths := make(chan string, 100)
	// get all the paths

	g.Go(func() error {
		defer close(paths)

		return filepath.Walk(root, func(path string, info os.FileInfo, err error) error {
			if err != nil {
				return err
			}
			if !info.Mode().IsRegular() {
				return nil
			}
			if !info.IsDir() && !strings.HasSuffix(info.Name(), ".go") {
				return nil
			}

			select {
			case paths <- path:
			case <-ctx.Done():
				return ctx.Err()
			}
			return nil
		})

	})

	c := make(chan string, 100)
	count := 0
	var count1 int64
	for path := range paths {
		p := path
		count++
		g.Go(func() error {
			select {
			case <-ctx.Done():
				return ctx.Err()
			default:
			}
			atomic.AddInt64(&count1, 1)
			data, err := ioutil.ReadFile(p)
			if err != nil {
				return err
			}
			if !bytes.Contains(data, []byte(pattern)) {
				return nil
			}
			c <- p
			return nil
		})
	}
	go func() {
		g.Wait()
		close(c)
	}()
	var m []string
	for r := range c {
		m = append(m, r)
	}
	fmt.Println("Count: " + strconv.Itoa(count))
	fmt.Println("Count1: " + strconv.Itoa(int(count1)))
	return m, g.Wait()
}

```
