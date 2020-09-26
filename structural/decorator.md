<p align="center">
  <img src="../gopher.png" />
</p>

---

# Decorator Pattern
This pattern allows to add more functionality to an existing object without modifying it's code. The decorator patterns provides a way to extend the functionality of an object statically or in some cases at run-time. Dceorator class wraps the original class and the decorator and the original class object share a common set of features.
([more on wikipedia](https://en.wikipedia.org/wiki/Decorator_pattern)) 
<br />
Decorators should not alter the interface of an object.
<br />

## Example
We have a log class that can print the log content. We need to extend the class to print the log date too. But we don't want to modify the current class code.

## Implenetation

```go
package main

import (
	"fmt"
	"time"
)

type log struct {
	content   string
	timestamp time.Time
}

func (l *log) Print() {
	fmt.Println(l.content)
}

type logDecorator struct {
	log       log
}

// this decorator adds time print functionality without modifying the current log object
func (l *logDecorator) Print() {
	fmt.Println(l.log.timestamp.Format("02-Jan-2006"))
	l.log.Print()
}

func main() {

	log := log{
		content:   "This is a test log",
		timestamp: time.Now(),
	}
	decorator := logDecorator{
		log: log,
	}

	// compare the outputs
	decorator.Print()
	log.Print()
}

```