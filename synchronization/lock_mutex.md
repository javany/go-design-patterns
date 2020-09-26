<p align="center">
  <img src="../gopher.png" />
</p>

---

# Mutual Exclusion
It's a concurrency mechanism to prevent the race conditions.<br />
It is the requirement that one thread of execution never enters its critical section at the same time that another concurrent thread of execution enters its own critical section, which refers to an interval of time during which a thread of execution accesses a shared resource, such as shared memory. 

([more on wikipedia](https://en.wikipedia.org/wiki/Mutual_exclusion))

## Example
In this example we increment a count value inside a struct by multiple goroutines concurrently. If you remove the mutex lock and execute the code multiple times, you will see different values for the count.

## Implementation

```go
package main

import (
	"fmt"
	"sync"
)

type item struct {
	count int
	mux   sync.Mutex
}

func (d *item) Inc(wg *sync.WaitGroup) {
	defer wg.Done()

	d.mux.Lock()
	// only one goroutine at a time can access the d.count
	d.count++
	d.mux.Unlock()
}

func main() {

	d := item{
		count: 0,
	}
	var wg sync.WaitGroup

	for i := 0; i < 1000; i++ {
		wg.Add(1)

		go d.Inc(&wg)
	}

	wg.Wait()
	fmt.Println(d.count)
}

```
