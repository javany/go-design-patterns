<p align="center">
  <img src="../gopher.png" />
</p>

---

# Object Pool Pattern
An object pool is a set of initialized expensive objects that are ready to use. When a client needs an object, instead of creating a new one, it simply requests an object from the pool. When the client finished it's work returns the object to the pool rather than destroying it.([more on wikipedia](https://en.wikipedia.org/wiki/Object_pool_pattern)) 
<br />
Object pooling provides a significant performance boost; it is most helpful when the cost of initializing instances of a class is high and the number of instances in use at at the same time is low.


## Example
Suppose that we need a connection pool for a database to which creating new connections has high cost.

## Implementation
We create an objectPool with an array of initialized objects. The object pool needs acquire and release methods to get and return the object from and to the pool. In practice it's better to implement it as a singleton instance.

```go
package main

import (
	"errors"
	"fmt"
	"sync"
)

type object interface {
	getUniqueId() int
}

type pool struct {
	capacity int
	free     []object
	busy     []object
	locker   *sync.Mutex
}

func createPool(objects []object) *pool {

	return &pool{
		capacity: len(objects),
		free:     objects,
		busy:     make([]object, 0),
		locker:   new(sync.Mutex),
	}
}

func (p *pool) Acquire() (object, error) {
	p.locker.Lock()
	defer p.locker.Unlock()
	if len(p.free) == 0 {
		return nil, errors.New("no free object in the pool")
	}
	// take the first object from the list and append it to busy list
	instance := p.free[0]
	p.free = p.free[1:]
	p.busy = append(p.busy, instance)
	return instance, nil
}

func (p *pool) Release(instance object) error {
	p.locker.Lock()
	defer p.locker.Unlock()

	for i := 0; i < len(p.busy); i++ {
		if p.busy[i].getUniqueId() == instance.getUniqueId() {
			freeItem := p.busy[i]
			p.busy = append(p.busy[:i], p.busy[i+1:]...)
			p.free = append(p.free, freeItem)
			return nil
		}
	}
	return errors.New("unknown object")
}

// suppose that we want to have a connection objects pool
type connection struct {
	uniqueId int
}

func (c *connection) getUniqueId() int {
	return c.uniqueId
}

func main() {

	connections := make([]object, 0)
	for i := 0; i < 2; i++ {
		c := &connection{uniqueId: i}
		connections = append(connections, c)
	}
	pool := createPool(connections)

	connA, _ := pool.Acquire()
	connB, _ := pool.Acquire()

	// there should be an error because all objects are busy
	connC, err := pool.Acquire()
	if err != nil {
		fmt.Println(err.Error())
	}

	pool.Release(connA)
	pool.Release(connB)

	// this time, there should be no error
	connC, err = pool.Acquire()
	if err != nil {
		fmt.Println(err.Error())
	} else {
		fmt.Println("acquired the needed object")
	}
	pool.Release(connC)

	fmt.Println("Goodbye!")

}
// execution result :
// no free object in the pool
// acquired the needed object
// Goodbye!
```

