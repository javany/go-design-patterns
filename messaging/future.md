<p align="center">
  <img src="../gopher.png" />
</p>

---

# Future Pattern
The **future** is the value that is going to be set asynchronously. The **promise** is the **asynchronous**  function that sets the value. Setting the value of a future is also called **resolving**, fulfilling, or binding it.

<br />

The Future pattern can easily be implemented in channels: a future is a one-element channel, and a promise is a process that sends to the channel, resolving the future.

([more on wikipedia](https://en.wikipedia.org/wiki/Futures_and_promises))


## Implementation

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

// task is a long running task
func task() <-chan int {

	c := make(chan int)

    // promise
	go func() {
		defer close(c)

		time.Sleep(time.Second * 1)
		c <- rand.Intn(100)
	}()

	return c
}

func main() {

    // future is the task one-element channel
    result := <-task()
    
	fmt.Println(result)
}

```

