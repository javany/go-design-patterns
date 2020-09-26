<p align="center">
  <img src="../gopher.png" />
</p>

---

# Adapter Pattern
Adapter pattern is for adapatation of two existing incompatible interfaces without touching the old source code. 
<br />
The existing component have a similar functionality to what a client needs but with different method names or different arguments.([more on wikipedia](https://en.wikipedia.org/wiki/Adapter_pattern)) 
<br />

We have 4 elements in Adapter pattern:

* Target is a specific interface that the clients want to use
* Adaptee is an existing interface that needs to be adapted to be used
* Adapter adapts the Adaptee interface to the Target interface.
* Client that requires a Target interface and cannot reuse the Adaptee struct directly

## Example
Suppose that we have an existing class that sorts a list of numbers by bubble sort algorithm and it has a method named BubbleSort(). In our new system interface, we do not specify the type of the sort algorithm. So our target interface has a method named Sort(). So we need an adapter to be able to work with the current class.

## Implementation

```go
package main

import "fmt"

// Target interface with different method name
type TargetArrayHandler interface {
	Sort([]int) []int
}

// Adaptee
type CurrentArrayHandler struct {
}

// old method
func (a *CurrentArrayHandler) BubbleSort(numbers []int) []int {
	for i := len(numbers); i > 0; i-- {
		for j := 1; j < i; j++ {
			if numbers[j-1] > numbers[j] {
				intermediate := numbers[j]
				numbers[j] = numbers[j-1]
				numbers[j-1] = intermediate
			}
		}
	}
	return numbers
}

// Adapter
type ArrayHandlerAdapter struct {
	*CurrentArrayHandler
}

func (a *ArrayHandlerAdapter) Sort(numbers []int) []int {
	return a.BubbleSort(numbers)
}

func main() {

	numbers := []int{9, 3, 7, 4}
	handler := &ArrayHandlerAdapter{}
	handler.Sort(numbers)
	fmt.Println(numbers)

}
// // execution result :
// [3 4 7 9]
```
