<p align="center">
  <img src="../gopher.png" />
</p>

---

# Command Pattern
This pattern helps to encapsulate all information needed to perform an action at a later time.<br />
This action calls a method of an object with predefined parameter values. 


Terms that always associated with the command pattern are:
* **invoker**: calls the command Execute method. It has no idea of how it works.
* **command**: embeds a receiver object inside to call its method with predefined value(s).
* **receiver**: is the object which the command calls its method with predefined value(s).
<br/>

The command pattern decouples the invoker class from the receiver class that knows how to execute an operation. <br />
([more on wikipedia](https://en.wikipedia.org/wiki/Command_pattern))

## Example
We have a user interface devided into sidebar and panel sections. Each section has a button for changing its color theme. 

## Implementation

```go
package main

import "fmt"

// --------- receiver intercae
type userInterface interface {
	darken()
	lighten()
}

// sidebar
type sideBar struct {
	isDark bool
}

func (s *sideBar) darken() {
	s.isDark = true
	fmt.Println("sidebar changed into dark theme")
}

func (s *sideBar) lighten() {
	s.isDark = false
	fmt.Println("sidebar changed into light theme")
}

// main panel
type panel struct {
	isDark bool
}

func (s *panel) darken() {
	s.isDark = true
	fmt.Println("panel changed into dark theme")
}

func (s *panel) lighten() {
	s.isDark = false
	fmt.Println("panel changed into light theme")
}

// ------ command interface
type command interface {
	Execute()
}

// lighter command
type cmdLightener struct {
	ui userInterface
}

func (c *cmdLightener) Execute() {
	c.ui.lighten()
}

// darkener command
type cmdDarkener struct {
	ui userInterface
}

func (c *cmdDarkener) Execute() {
	c.ui.darken()
}

// --------------

type button struct {
	command command
}

func (b *button) click() {
	b.command.Execute()
}

// --------------

func main() {

	var lightener, darkener command
	var btnLight, btnDark button

	// receivers
	sidebar := &sideBar{}
	panel := &panel{}

	// sidebar commands
	lightener = &cmdLightener{
		ui: sidebar,
	}
	darkener = &cmdDarkener{
		ui: sidebar,
	}

	btnLight = button{
		command: lightener,
	}
	btnDark = button{
		command: darkener,
	}
	btnLight.click()
	btnDark.click()

	// use the same commands for another receiver
	// panel commands
	lightener = &cmdLightener{
		ui: panel,
	}
	darkener = &cmdDarkener{
		ui: panel,
	}
	btnLight = button{
		command: lightener,
	}
	btnDark = button{
		command: darkener,
	}
	btnLight.click()
	btnDark.click()

}

// // execution result :
// sidebar changed into light theme
// sidebar changed into dark theme
// panel changed into light theme
// panel changed into dark theme
```



