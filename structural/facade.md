<p align="center">
  <img src="../gopher.png" />
</p>

---

# Facade Pattern
This pattern helps to hide the complexities of a system and provides a simple interface to clients. A facade is an object that serves as a simple interface masking a complex code. ([more on wikipedia](https://en.wikipedia.org/wiki/Facade_pattern))
<br />

## Example
When an app starts it usually initializes some services and make instances of some classes. We can hide these steps inside an initializer facade.


## Implementation

```go
package main

import "fmt"

// Logger -----------------
type Logger interface {
	CreateFile()
}
type logger struct{}

func (l *logger) CreateFile() {
	fmt.Println("the logger file is ready")
}

var loggerInstance Logger

func getNewLogger() Logger {
	if loggerInstance == nil {
		loggerInstance = &logger{}
	}
	return loggerInstance
}

// Cache ----------------
type Cache interface {
	Start()
}
type cache struct{}

func (c *cache) Start() {
	fmt.Println("the cache successfully started")
}

var cacheInstance Cache

func getCacheInstance() Cache {
	if cacheInstance == nil {
		cacheInstance = &cache{}
	}
	return cacheInstance
}

// ----------

type InitFacade struct{}

func (f *InitFacade) Start() {

	l := getNewLogger()
	l.CreateFile()

	c := getCacheInstance()
	c.Start()

	fmt.Println("system initialized")
}

func main() {

	starter := &InitFacade{}
	starter.Start()

}

// // execution result :
// the logger file is ready
// the cache successfully started
// system initialized
```