<p align="center">
  <img src="../gopher.png" />
</p>

---

# Observer Pattern
In this pattern an object called **Subject** maintains a list of other objects called **Observers** who want to get notified of state changes. When a subject changes state, all registered observers are notified and updated automatically <br />
<br />

Typically, the observer pattern is implemented so the "subject" being "observed" is part of the object for which state changes are being observed (and communicated to the observers). This type of implementation is considered "tightly coupled".
<br />
The observer pattern may be **used in the absence of publish-subscribe**, as in the case where model status is frequently updated. 

 ([more on wikipedia](https://en.wikipedia.org/wiki/Observer_pattern))

 ## Example
Suppose that we have a config class that holds key-values for different sections of the system. Other classes like logger class need to know when a value changes in config class state.

## Implementation

```go
package main

import "fmt"

// observer interface
type observer interface {
	update(string)
}

// subject interface
type subject interface {
	attach(observer)
	notifyObservers()
}

// concrete subject
type configs struct {
	observers []observer
	logLevel  string
}

func (c *configs) attach(o observer) {
	c.observers = append(c.observers, o)
}

func (c *configs) notifyObservers() {
	for _, observer := range c.observers {
		observer.update(c.logLevel)
	}
}

func (c *configs) SetLogLevel(level string) {
	c.logLevel = level
	c.notifyObservers()
}

// concrete observers
type logger struct {
	logLevel string
}

func (l *logger) update(level string) {
	l.logLevel = level
}

func (l *logger) Info(msg string) {
	fmt.Println(msg)
}

func (l *logger) Error(msg string) {
	if l.logLevel == "debug" {
		fmt.Println(msg)
	}
}

func main() {

	config := &configs{
		logLevel: "info",
	}
	logger := &logger{}

	config.attach(logger)

	logger.Info("this is a test info")

	config.SetLogLevel("debug")
	logger.Error("this error should be printed")

	config.SetLogLevel("info")
	logger.Error("this error is not printed")

}

// // execution result :
// this is a test info
// this error should be printed
```