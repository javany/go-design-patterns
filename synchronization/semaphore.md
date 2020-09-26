<p align="center">
  <img src="../gopher.png" />
</p>

---

# Semaphore
a **semaphore** is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system.

([more on wikipedia](https://en.wikipedia.org/wiki/Semaphore_(programming)))


Semaphore is similar to **mutex** but:

* **Mutex** gives access to just a single thread at a time
* **Semaphore** gives access to at most N threads, to limit concurrent access to a shared resource


## Example
This example demonstrates how to use a semaphore to limit the number of goroutines working on parallel tasks.

([go doc](https://godoc.org/golang.org/x/sync/semaphore#example-package--WorkerPool))

## Implementation

```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"

	"golang.org/x/sync/semaphore"
)

func main() {
    // a non-nil empty context
	ctx := context.TODO()

	var (
		maxWorkers = 3
		sem        = semaphore.NewWeighted(int64(maxWorkers))
		out        = make([]int, 20)
	)

	var wg sync.WaitGroup
	// do the job using up to maxWorkers goroutines at a time.
	for i := range out {
		wg.Add(1)
		// When maxWorkers goroutines are in flight,
		// Acquire blocks until one of the workers finishes.
		if err := sem.Acquire(ctx, 1); err != nil {
			fmt.Printf("Failed to acquire semaphore: %v", err)
			break
		}

		go func(i int) {
			defer wg.Done()
			defer sem.Release(1)
			out[i] = i + 1
			fmt.Print(" ", i+1)
			time.Sleep(1 * time.Second)
		}(i)
	}

	wg.Wait()
	fmt.Println("\n", out)

}

```