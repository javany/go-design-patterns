<p align="center">
  <img src="../gopher.png" />
</p>

---

# Template Method Pattern
This pattern lets us define a template or algorithm for a particular operation. The essence of the template method pattern is that it allows you to inject an implementation of a particular function or functions into the skeleton of an algorithm. <br />

By this pattern, implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary.

([more on wikipedia](https://en.wikipedia.org/wiki/Template_method_pattern))

<br />

## Example
A player commonly do 3 things in a game. So playing can be a template of 3 methods that can be implemented by subclasses (different games)

## Implementation

```go
package main

import (
	"fmt"
)

// game interface
type game interface {
	initialize()
	start()
	end()
}

// concrete games

type sonic struct{}

func (s *sonic) initialize() {
	fmt.Println("sonic game initialized")
}

func (s *sonic) start() {
	fmt.Println("sonic game started")
}

func (s *sonic) end() {
	fmt.Println("sonic game finished")
}

type mario struct{}

func (m *mario) initialize() {
	fmt.Println("mario game initialized")
}

func (m *mario) start() {
	fmt.Println("mario game started")
}

func (m *mario) end() {
	fmt.Println("mario game finished")
}

// gamer struct

type player struct {
	game game
}

func (p *player) play() {
	p.game.initialize()
	p.game.start()
	p.game.end()
}

func main() {

	sonic := &sonic{}

	playerA := &player{
		game: sonic,
	}
	playerA.play()

	mario := &mario{}

	playerB := &player{
		game: mario,
	}
	playerB.play()
}

// // execution result :
// sonic game initialized
// sonic game started
// sonic game finished
// mario game initialized
// mario game started
// mario game finished
```


