<p align="center">
  <img src="../gopher.png" />
</p>

---

# Iterator Pattern
Iterators are functions that return the next value in a sequence each time the function is called. <br />
Iterator pattern helps to define a separate (iterator) object that encapsulates accessing and traversing an aggregate object.

([more on wikipedia](https://en.wikipedia.org/wiki/Iterator_pattern))


## Example
Lets make an iterator function like range in golang.

## Implementation

```go
package main

import "fmt"

func iterator(limit int) <-chan int {
	c := make(chan int)
	go func() {
		for i := 0; i < limit; i++ {
			c <- i
		}

		close(c)
	}()
	return c
}

func main() {

	for i := range iterator(10) {
		fmt.Println(i)
	}

}

```