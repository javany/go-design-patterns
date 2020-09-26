<p align="center">
  <img src="../gopher.png" />
</p>

---

# Proxy Pattern
In short, a proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy, extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.
<br />
This pattern is useful when we want to:

* Control the **access** to an object
* Add some functioality **when accessing** to an object

([more on wikipedia](https://en.wikipedia.org/wiki/Proxy_pattern)) 
<br />

## Example
We have a system that has a cache and a database. Each time we search a record, our proxy first checks the cache to optimize the speed and minimize the load on the database.

## Implementation

```go
package main

import (
	"errors"
	"fmt"
)

type handler interface {
	GetUserById(int) string
}

type user struct {
	Id   int
	Name string
}

// ------------
type cache struct {
	users []user
}

func (c *cache) GetUserById(id int) (user, error) {
	for _, user := range c.users {
		if user.Id == id {
			return user, nil
		}
	}
	return user{}, errors.New("404")
}

var cacheInstance cache

// -------------
type database struct{}

func (d *database) GetUserById(id int) (user, error) {
	fakeUser := user{
		Id:   id,
		Name: fmt.Sprintf("user_%d", id),
	}
	return fakeUser, nil
}

type userProxy struct{}

// the proxy first checks the cache then the database
func (p *userProxy) GetUserById(id int) (user, error) {

	// check if user exists in cache
	user, err := cacheInstance.GetUserById(id)
	if err == nil {
		fmt.Println("user fetched from cache")
		return user, nil
	}

	// query the database to fetch the user
	db := database{}
	fmt.Println("user fetched from database")
	return db.GetUserById(id)

}

func main() {

	// init the cache
	cacheInstance = cache{
		users: []user{
			{
				Id:   1,
				Name: "user_1",
			},
			{
				Id:   2,
				Name: "user_2",
			},
		},
	}

	proxy := &userProxy{}

	user, _ := proxy.GetUserById(1)
	fmt.Println(user)

	user, _ = proxy.GetUserById(5)
	fmt.Println(user)

}
// // execution result :
// user fetched from cache
// {1 user_1}
// user fetched from database
// {5 user_5}
```