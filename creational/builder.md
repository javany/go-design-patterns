<p align="center">
  <img src="../gopher.png" />
</p>

---

# Builder Pattern
The intent of the Builder design pattern is to separate the **construction** of a complex object from its **representation**.[^1] ([more on wikipedia](https://en.wikipedia.org/wiki/Builder_pattern))

<br />

Builder pattern is useful when:

* **Construction** of a **complex** object requires **multiple steps**: So this pattern let you break up the construction of the complex object
* **Different versions** of **the same product** need to be created: This pattern provides a way to easily add more representations of the product. 

<br />

In builder pattern we have four components.

* Product: the complex object that's constructed
* Director - refers to the Builder interface for building (creating and assembling) the parts of a complex object
* Builder interface: interface for creating and assembling different objects
* Some Concrete Builders: each for a different part or representation


## Example
Suppose that we have a game character and need to have different appearance styles for her.

## Implementation
```go
package main

import "fmt"

// the product : a game character
type character struct {
	hairStyle  string
	hairColor  string
	clothColor string
}

// the builder interface
type builder interface {
	SetHairStyle()
	SetHairColor()
	SetClothColor()
	GetCharacter() character
}

// director
type director struct {
	builder builder
}

func (d *director) setBuilder(b builder) {
	d.builder = b
}

func (d *director) buildGameCharacter() character {
	d.builder.SetHairStyle()
	d.builder.SetHairColor()
	d.builder.SetClothColor()
	return d.builder.GetCharacter()
}

func getDirector(b builder) *director {
	return &director{
		builder: b,
	}
}

// Some Concrete Builders for different styles
// black style builder
type blackStyleBuilder struct {
	hairStyle  string
	hairColor  string
	clothColor string
}

func (b *blackStyleBuilder) SetHairStyle() {
	b.hairStyle = "African"
}

func (b *blackStyleBuilder) SetHairColor() {
	b.hairColor = "black"
}

func (b *blackStyleBuilder) SetClothColor() {
	b.clothColor = "black"
}

func (b *blackStyleBuilder) GetCharacter() character {
	return character{
		hairStyle:  b.hairStyle,
		hairColor:  b.hairColor,
		clothColor: b.clothColor,
	}
}

// blond style builder
type blondeStyleBuilder struct {
	hairStyle  string
	hairColor  string
	clothColor string
}

func (b *blondeStyleBuilder) SetHairStyle() {
	b.hairStyle = "Wavy"
}

func (b *blondeStyleBuilder) SetHairColor() {
	b.hairColor = "blond"
}

func (b *blondeStyleBuilder) SetClothColor() {
	b.clothColor = "white"
}

func (b *blondeStyleBuilder) GetCharacter() character {
	return character{
		hairStyle:  b.hairStyle,
		hairColor:  b.hairColor,
		clothColor: b.clothColor,
	}
}

// -------
func getBuilder(style string) builder {
	if style == "black" {
		return &blackStyleBuilder{}
	}
	if style == "blonde" {
		return &blondeStyleBuilder{}
	}
	return nil
}

func main() {

	blondBuilder := getBuilder("blonde")
	director := getDirector(blondBuilder)
	blondeCharacter := director.buildGameCharacter()

	blackBuilder := getBuilder("black")
	director.setBuilder(blackBuilder)
	blackCharacter := director.buildGameCharacter()

	fmt.Println("blond character hair color: ", blondeCharacter.hairColor)
	fmt.Println("black character hair color: ", blackCharacter.hairColor)

}

// execution result :
// blond character hair color:  blond
// black character hair color:  black

```

[^1]: [wikipedia](https://en.wikipedia.org/wiki/Builder_pattern)