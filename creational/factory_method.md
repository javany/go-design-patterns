<p align="center">
  <img src="../gopher.png" />
</p>

---

# Factory Method Pattern
This pattern provides a way to create objects without having to specify the exact type of the object that will be created.
It hides the creation logic of the instances that are going to be created.
([more on wikipedia](https://en.wikipedia.org/wiki/Factory_method_pattern)) 
<br />
<br />
"Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses." (Gang Of Four)

<br />
In this pattern we have 2 types of interfaces and some concrete objects that implement the interfaces.

* An interface for what is going to be created
* Some concrete objects of the specified interface
* An interface for creator
* Some concrete factory methods that implement the creator interface according to their object type

## Example
Suppose that we need to implement an notification service. The service can send notification messages to email or phone.  

## Implementation

```go
package main

import "fmt"

// An interface for what is going to be created
type Message interface {
	Send()
}

// concrete objects of the specified interface
type Email struct {
	Body    string
	Address string
}

func (e *Email) Send() {
	fmt.Println("To: ", e.Address, " => ", e.Body)
}

type Text struct {
	Content     string
	PhoneNumber string
}

func (t *Text) Send() {
	fmt.Println("To: ", t.PhoneNumber, " => ", t.Content)
}

// An interface for creator
type MessageFactory interface {
	Create() Message
}

//Some concrete factory methods that implement the creator interface according to their object type
type EmailFactory struct{}

func (e *EmailFactory) Create() Message {
	return &Email{
		Body:    "This email is for you",
		Address: "no-reply@example.com",
	}
}

type TextFactory struct{}

func (t *TextFactory) Create() Message {
	return &Text{
		Content:     "A message to your number",
		PhoneNumber: "+112345678",
	}
}

func main() {

	// we initialize a list of messaging services to be used in our app
	factories := []MessageFactory{&EmailFactory{}, &TextFactory{}}

	for _, factory := range factories {
		messageService := factory.Create()
		messageService.Send()
	}

}

// // execution result :
// To:  no-reply@example.com  =>  This email is for you
// To:  +112345678  =>  A message to your number
```