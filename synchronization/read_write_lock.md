<p align="center">
  <img src="../gopher.png" />
</p>

---

# Read/Write Mutual Exclusion
A **RWMutex** is a reader/writer mutual exclusion lock. The lock can be held by an arbitrary number of readers or a single writer.<br />
RWMutex safely allows multiple readers. 
<br />


## Implementation

```go
package main

import (
	"fmt"
	"sync"
)

type bank struct {
	balance int
	mux     sync.RWMutex
}

func (s *bank) add(wg *sync.WaitGroup) {
	defer wg.Done()

	for i := 0; i < 10; i++ {
		s.mux.Lock()
		s.balance += 5
		fmt.Println("set balance: ", s.balance)
		s.mux.Unlock()
	}
}

func (s *bank) get(wg *sync.WaitGroup) {
	defer wg.Done()

	for i := 0; i < 15; i++ {
		s.mux.RLock()

		fmt.Println("balance: ", s.balance)

		s.mux.RUnlock()
	}
}

func main() {

	s := bank{
		balance: 0,
	}

	var wg sync.WaitGroup

	wg.Add(1)
	go s.add(&wg)

	wg.Add(1)
	go s.get(&wg)

	wg.Wait()
}

```
