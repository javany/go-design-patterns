<p align="center">
  <img src="../gopher.png" />
</p>

---

# Mediator Pattern
The mediator pattern defines an object that encapsulates how a set of objects interact.
<br />
<br />
With the mediator pattern, communication between objects is encapsulated within a mediator object. **Objects no longer communicate directly with each other**, but instead communicate through the mediator. Objects delegate their interaction to a mediator object instead of interacting with each other directly. This reduces the dependencies between communicating objects, thereby reducing coupling.

## Example
A chat room class can act as a mediator between its members.

## Implementation

```go
package main

import (
	"fmt"
)

type person interface {
	register()
	send(string)
	receive(string)
	getName() string
}

type mediator interface {
	register(person)
	broadcast(person, string)
}

// ----------

type member struct {
	name     string
	mediator mediator
}

func (m *member) register() {
	m.mediator.register(m)
}

func (m *member) send(msg string) {
	m.mediator.broadcast(m, msg)
}

func (m *member) receive(msg string) {
	fmt.Println(msg)
}

func (m *member) getName() string {
	return m.name
}

// -------

type chatRoom struct {
	persons []person
}

func (c *chatRoom) register(p person) {
	c.persons = append(c.persons, p)
}

func (c *chatRoom) broadcast(p person, msg string) {

	for _, person := range c.persons {
		if person.getName() != p.getName() {
			person.receive(msg)
		}
	}

}

func main() {

	room := &chatRoom{}

	daniel := member{
		name:     "daniel",
		mediator: room,
	}
	sara := member{
		name:     "sara",
		mediator: room,
	}

	daniel.register()
	sara.register()

	daniel.send("this is daniel")
	sara.send("this is sara")
	daniel.send("this is another message from daniel")

}

// // execution result :
// this is daniel
// this is sara
// this is another message from daniel
```