<p align="center">
  <img src="../gopher.png" />
</p>

---

# Singleton Pattern
Sometimes we need to limit the number of instances of a struct to just one instance. This single instance is called a singleton object.
([more on wikipedia](https://en.wikipedia.org/wiki/Singleton_pattern)) 

<br />

## Example
Let's create a singleton object of a logger.

## Implementation

```go
package main

import (
	"fmt"
	"sync"
)

// a simple logger inside the main package
// we can create another package for our logger
type logger struct {
}

func (l *logger) Info(msg string) {
	fmt.Println(msg)
}

// declare the singleton instance
var loggerInstance *logger

// we need thread safety
// so we use sync.Once from the sync package
var createLoggerOnce sync.Once

// we call this function when we need the logger instance
func GetLogger() *logger {
	// Do calls the function if and only if Do is being called for the first time for this instance of Once
	createLoggerOnce.Do(func() {
		loggerInstance = &logger{}
		fmt.Println("logger instance object is created once")
	})

	return loggerInstance
}

func main() {
	var wg sync.WaitGroup
	// create 3 goroutines and check the result
	for i := 0; i < 3; i++ {
		wg.Add(1)
		go func(number int) {
			mylogger := GetLogger()
			mylogger.Info(fmt.Sprintf("log from goroutine number %d", number))
			wg.Done()
		}(i)
	}
	wg.Wait()
}

// // execution result :
// logger instance object is created once
// log from goroutine number 2
// log from goroutine number 0
// log from goroutine number 1
```


