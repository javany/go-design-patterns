<p align="center">
  <img src="../gopher.png" />
</p>

---

# Monitor Pattern
This pattern provides a way to make a goroutine wait till some event occur.<br />
Another definition of monitor is a thread-safe class that wraps around a mutex in order to safely allow access to a method or variable by more than one thread. <br />
<br />

A **condition variable** essentially is a container of threads that are waiting for a certain condition. <br />

([more on wikipedia](https://en.wikipedia.org/wiki/Monitor_(synchronization)))

## Example
In Go, we can create Condition Variable using **sync.Cond** type.<br />
It has one contructor function(**sync.NewCond()**) which takes a **sync.Locker**. <br />
It has three methods:

* **Wait**()
* **Signal**()
* **Broadcast**()

Let's make a goroutine wait until a value is assigned to a variable.

## Implementation

### *Solution 1: **using sync.Cond***
<br />

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

type price struct {
	sync.Mutex
	value string
	cond  *sync.Cond
}

func NewPrice() *price {
	r := price{}
	r.cond = sync.NewCond(&r)
	return &r
}

func main() {

	var wg sync.WaitGroup

	pr := NewPrice()
	wg.Add(1)
	go func(pr *price) {
		defer wg.Done()
		pr.Lock()
		pr.cond.Wait()
		pr.Unlock()
		fmt.Println("price is : ", pr.value)
		return
	}(pr)

	time.Sleep(1 * time.Second)

	pr.Lock()
	pr.value = "$100"
	pr.Unlock()

	pr.cond.Signal()

	wg.Wait()
}

```
<br />

### *Solution 2: **using channel***
<br />

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

type price struct {
	sync.Mutex
	value string
}

func main() {

	var wg sync.WaitGroup

	ch := make(chan struct{})
	pr := &price{}

	wg.Add(1)
	go func(pr *price) {
		defer wg.Done()
		<-ch
		fmt.Println("price value is: ", pr.value)
		return
	}(pr)

	time.Sleep(1 * time.Second)

	pr.Lock()
	pr.value = "$100"
	pr.Unlock()
	ch <- struct{}{}

	wg.Wait()
}

```
