<p align="center">
  <img src="../gopher.png" />
</p>

---

# Bridge Pattern
Bridge pattern compose objects in tree structure. It decouples abstraction from implementation so that the two can vary independently.([more on wikipedia](https://en.wikipedia.org/wiki/Bridge_pattern)) 
<br />

## Example
Suppose that we have two logger packages. The first logger writes into a log file and the other one writes the logs into a database. <br />
We have an interface that has a log method. The classes that implement this interface can set each logger they want as their logger because they implement an interface that references to a logger interface not a concrete implementation.

## Implementation

```go
package main

import "fmt"

// a logger interface with two concrete implementations
type logger interface {
	Write(string)
}

// concrete implementation 1
type dbLogger struct{}

func (d *dbLogger) Write(msg string) {
	fmt.Println("saved in the database: ", msg)
}

// concrete implementation 2
type fileLogger struct{}

func (d *fileLogger) Write(msg string) {
	fmt.Println("appended to the file: ", msg)
}

// an interface which needs logging
type handler interface {
	Log(string)
	setLogger()
}

// a concrete implementation that references the logger interface
// it can set any logger that implements the logger interface
type httpHandler struct {
	logger logger
}

func (h *httpHandler) Log(msg string) {
	h.logger.Write(msg)
}

// this method provides a way to separate the abstraction from implementation
// our concrete class refers to a logger interface not a concrete one
// so we can choose between existing concrete implementations of the logger interface
func (h *httpHandler) setLogger(l logger) {
	h.logger = l
}

// a concrete implementation that references the logger interface
// it can set any logger that implements the logger interface
type rpcHandler struct {
	logger logger
}

func (r *rpcHandler) Log(msg string) {
	r.logger.Write(msg)
}

func (r *rpcHandler) setLogger(l logger) {
	r.logger = l
}

func main() {

	dbLogger := &dbLogger{}
	fileLogger := &fileLogger{}

	httpHandler := &httpHandler{}
	httpHandler.setLogger(dbLogger)
	httpHandler.Log("db log from http handler")
	httpHandler.setLogger(fileLogger)
	httpHandler.Log("file log from http handler")

	rpcHandler := &rpcHandler{}
	rpcHandler.setLogger(dbLogger)
	rpcHandler.Log("db log from rpc handler")
	rpcHandler.setLogger(fileLogger)
	rpcHandler.Log("file log from rpc handler")

}

// // execution result :
// saved in the database:  db log from http handler
// appended to the file:  file log from http handler
// saved in the database:  db log from rpc handler
// appended to the file:  file log from rpc handler
```
