<p align="center">
  <img src="../gopher.png" />
</p>

---

# Chain of Responsibility Pattern
This pattern provides a way that each incoming request is passed through a chain of processing objects.
<br />
According to the Gof book[^1] definition, **exactly one** of the classes in the chain handles the request. 
<br> 
But many implementations allow **several classes** in the chain to take responsibility. 
<br />
Also in a variation of the standard model, some handlers may act as **dispatchers** that send the request in a variety of directions. ([more on wikipedia](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern))

<br />

## Example
When a company hires a new employee, some departments need to do some predefined tasks relating to the new employee.

## Implementation

```go
package main

import "fmt"

type employee struct {
	ID          int
	Name        string
	Email       string
	BankAccount string
}

type department interface {
	process(*employee)
}

// HR department for registering the new employee in staff list
type HR struct {
	next department
}

func (h *HR) process(e *employee) {
	fmt.Println("HR department tasks are done.")

	if h.next != nil {
		h.next.process(e)
	}
}

// Accounting department for registering the bank account
type Accounting struct {
	next department
}

func (h *Accounting) process(e *employee) {
	fmt.Println("Accounting department tasks are done.")

	if h.next != nil {
		h.next.process(e)
	}
}

// Technical department for training the new employee
type Technical struct {
	next department
}

func (h *Technical) process(e *employee) {
	fmt.Println("Technical department tasks are done.")

	if h.next != nil {
		h.next.process(e)
	}
}

func main() {

	employee := &employee{
		Name:        "John",
		BankAccount: "a_bank_number",
		Email:       "John@example.com",
	}

	technical := &Technical{}
	accounting := &Accounting{next: technical}
	hr := &HR{next: accounting}

	hr.process(employee)

}

// // execution result :
// HR department tasks are done.
// Accounting department tasks are done.
// Technical department tasks are done.
```

[^1]: Design Patterns: Elements of Reusable Object-Oriented Software (1994) by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.