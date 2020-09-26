<p align="center">
  <img src="../gopher.png" />
</p>

---

# Fan-Out Pattern
Multiple functions can read from the same channel until that channel is closed; this is called fan-out. This provides a way to distribute work amongst a group of workers to parallelize CPU use and I/O.([go blog](https://blog.golang.org/pipelines#TOC_4.))


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

func fanOut(ch <-chan int, size int) []chan int {
	chans := make([]chan int, size)
	for i, _ := range chans {
		chans[i] = make(chan int)
	}
	go func() {
		for i := range ch {
			for _, c := range chans {
				c <- i
			}
		}
		for _, c := range chans {
			close(c)
		}
	}()
	return chans
}


func consumer(in <-chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range in {
		fmt.Println(n)
	}
}


func main() {

	// generate numbers in an input channel
	nums := []int{11, 12, 13}
	c := producer(nums...)

	// clone the input channel into 3 channels
	chans := fanOut(c, 3)

	var wg sync.WaitGroup
	wg.Add(1)
	go consumer(chans[0], &wg)
	wg.Add(1)
	go consumer(chans[1], &wg)
	wg.Add(1)
	go consumer(chans[2], &wg)

	wg.Wait()

}

```
