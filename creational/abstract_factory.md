<p align="center">
  <img src="../gopher.png" />
</p>

---

# Abstract Factory Pattern
Abstract factory is a factory of factories. It's an abstraction over the factory pattern.
<br/>
"Intent: Provide an interface for creating families of **related or dependent objects** without specifying their concrete classes.".[^1] ([more on wikipedia](https://en.wikipedia.org/wiki/Abstract_factory_pattern)) 


## Example
Suppose that we are going to buy a laptop with a mouse and want them to be of the **same brand**.

## Implementation

```go
package main

import (
	"errors"
	"fmt"
)

// define interfaces for the products
type laptop interface {
	GetPrice() int
	GetInfo() string
}

type mouse interface {
	GetPrice() int
	GetColor() string
}

// define concrete products
// apple
type macBookPro struct{}

func (m *macBookPro) GetPrice() int {
	return 3000
}

func (m *macBookPro) GetInfo() string {
	return "MacBook Pro elevates the notebook to a whole new level of performance and portability."
}

type magicMouse struct{}

func (m *magicMouse) GetPrice() int {
	return 80
}

func (m *magicMouse) GetColor() string {
	return "Silver"
}

// microsoft
type surfacePro struct{}

func (s *surfacePro) GetPrice() int {
	return 2000
}

func (s *surfacePro) GetInfo() string {
	return "Surface Pro 7 is your endlessly adaptable partner now with faster processing and more connections."
}

type arcMouse struct{}

func (a *arcMouse) GetPrice() int {
	return 800
}

func (a *arcMouse) GetColor() string {
	return "Black"
}

// define the Abstract Factory interface
type DeviceFactory interface {
	GetLaptop() laptop
	GetMouse() mouse
}

// define concrete factories for some popular brands
// aple
type apple struct{}

func (a *apple) GetLaptop() laptop {
	return &macBookPro{}
}
func (a *apple) GetMouse() mouse {
	return &magicMouse{}
}

// microsoft
type microsoft struct{}

func (m *microsoft) GetLaptop() laptop {
	return &surfacePro{}
}

func (m *microsoft) GetMouse() mouse {
	return &arcMouse{}
}

// ------
func getDeviceFactory(brand string) (DeviceFactory, error) {
	switch brand {
	case "apple":
		return &apple{}, nil
	case "microsoft":
		return &microsoft{}, nil
	default:
		return nil, errors.New("unknown brand")
	}
}

func main() {

	// I want to buy from apple
	// change the brand name and see the result
	appleCo, _ := getDeviceFactory("apple")
	laptop := appleCo.GetLaptop()
	mouse := appleCo.GetMouse()
	appleBasketPrice := laptop.GetPrice() + mouse.GetPrice()
	fmt.Println("You spent ", appleBasketPrice, " dollars.")
	fmt.Println(laptop.GetInfo())

}
// // execution result for apple
// You spent  3080  dollars.
// MacBook Pro elevates the notebook to a whole new level of performance and portability.
// // execution result for microsoft
// You spent  2800  dollars.
// Surface Pro 7 is your endlessly adaptable partner now with faster processing and more connections.
```

[^1]: Gamma, Erich; Richard Helm; Ralph Johnson; John M. Vlissides.

