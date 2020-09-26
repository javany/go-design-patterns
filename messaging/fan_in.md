<p align="center">
  <img src="../gopher.png" />
</p>

---

# Fan-In Pattern
A function can read from multiple inputs and proceed until all are closed by multiplexing the input channels onto a single channel that's closed when all the inputs are closed. This is called fan-in.([go blog](https://blog.golang.org/pipelines#TOC_4.))


## Implementation

```go
package main

import (
	"fmt"
	"sync"
)

func producer(nums ...int) <-chan int {
	out := make(chan int)
	go func() {
		for _, val := range nums {
			out <- val
		}
		close(out)
	}()
	return out
}

func fanIn(done <-chan struct{}, channels ...<-chan int) <-chan int {
	var wg sync.WaitGroup
	out := make(chan int)

	output := func(c <-chan int) {
		defer wg.Done()
		for n := range c {
			select {
			case out <- n:
			case <-done:
				return
			}
		}
	}

	wg.Add(len(channels))
	for _, c := range channels {
		go output(c)
	}

	go func() {
		wg.Wait()
		close(out)
	}()
	return out
}

func main() {

	done := make(chan struct{})
	defer close(done)

	nums := []int{1, 2, 3}
	c1 := producer(nums...)

	nums = []int{4, 5, 6}
	c2 := producer(nums...)

	nums = []int{7, 8, 9}
	c3 := producer(nums...)

	for n := range fanIn(done, c1, c2, c3) {
		fmt.Println(n)
	}

}

```
