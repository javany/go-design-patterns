<p align="center">
  <img src="../gopher.png" />
</p>

---

# Strategy Pattern
The strategy pattern (also known as the **policy pattern**) provides a way to change the behavior of an object at run time without any change in the class of that object.<br />
Deferring the decision about which algorithm to use until runtime allows the calling code to be more flexible and reusable.
<br />
([more on wikipedia](https://en.wikipedia.org/wiki/Strategy_pattern))
<br />
<br />

<p align="center">
  <img src="strategy.png" />
</p>

<br />

You should use this pattern when:

* you need an object that can accepts different algorithms for doing a work based on different conditions
* you are not going to use lots of if/else conditionals for chosing the right algorithm


## Example
We have a billing object that should be able to calculate the services total price based on different strategies.

## Implementation

```go
package main

import "fmt"

// strategy
type billingStrategy interface {
	getPrice() int
}

type normalStrategy struct{}

func (s *normalStrategy) getPrice() int {
	return 100
}

type happyHourStrategy struct{}

func (s *happyHourStrategy) getPrice() int {
	return 50
}

// context
type customerBill struct {
	strategy billingStrategy
}

func (c *customerBill) setStrategy(strategy billingStrategy) {
	c.strategy = strategy
}

func (c *customerBill) PrintPrice() {
	fmt.Println("the customer should pay: ", c.strategy.getPrice(), " dollars for service per hour")
}

func main() {

	normal := &normalStrategy{}
	discount := &happyHourStrategy{}

	customerBill := &customerBill{
		strategy: normal,
	}

	customerBill.PrintPrice()

	customerBill.setStrategy(discount)

	customerBill.PrintPrice()

}

// // execution result :
// the customer should pay:  100  dollars for service per hour
// the customer should pay:  50  dollars for service per hour

```