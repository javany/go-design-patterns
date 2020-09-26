<p align="center">
  <img src="../gopher.png" />
</p>

---

# Composite Pattern
The composite pattern describes a group of objects that are treated the same way as a single instance of the same type of object. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.[^1]
<br />
The intent of a composite is to "compose" objects into tree structures. In the tree structure we have **a unified Componet interface** for both **Leaft** objects and **Composite** objects.
<br />
**Leaf** objects implement the Component interface **directly**, and **Composite** objects **forward** requests to their child components
([more on wikipedia](https://en.wikipedia.org/wiki/Composite_pattern)) 

<br />

## Example
Suppose that we have a modular software which is developed by different teams. Each module should offer a function that displays some info about the module. Also we need to have a function that displays the info about whole the system and all modules.

## Implementation

```go
package main

import "fmt"

// unified Componet interface
type module interface {
	Info()
}

// Leaf
type router struct{}

func (r *router) Info() {
	fmt.Println("router handles the http request routing")
}

// Leaf
type handler struct{}

func (h *handler) Info() {
	fmt.Println("handler has functions to process the requests")
}

// Leaf
type repository struct{}

func (r *repository) Info() {
	fmt.Println("repository is responsible for storing and updating data")
}

// Composite
type system struct {
	modules []module
}

func (s *system) Info() {
	fmt.Println("This system is a composite system with the following modules:")
	for _, module := range s.modules {
		module.Info()
	}
}

func main() {
	router := &router{}
	handler := &handler{}
	repository := &repository{}
	system := system{
		modules: []module{
			router,
			handler,
			repository,
		},
	}

	system.Info()

}

// // execution result :
// This system is a composite system with the following modules:
// router handles the http request routing
// handler has functions to process the requests
// repository is responsible for storing and updating data
```

[^1]: Gamma, Erich; Richard Helm; Ralph Johnson; John M. Vlissides (1995). Design Patterns: Elements of Reusable Object-Oriented Software. Addison-Wesley. pp. 395. ISBN 0-201-63361-2.