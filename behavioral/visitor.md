<p align="center">
  <img src="../gopher.png" />
</p>

---

# Visitor Pattern
This pattern provides a way to define a new operation for a struct without modifying the struct. It define a separate (**visitor**) object that implements an operation to be performed on elements of an object structure. Then the struct can run that operation by **accept method**.

([more on wikipedia](https://en.wikipedia.org/wiki/Visitor_pattern))


## Example
We have books and papers and want to extract their references and number of pages. We should be able to add more functionalities later.

## Implementation

```go
package main

import "fmt"

type visitor interface {
	visitBook(*book)
	visitPaper(*paper)
}

type subject interface {
	accept(visitor)
}

type book struct {
	pages      int
	references int
}

func (b *book) accept(v visitor) {
	v.visitBook(b)
}

type paper struct {
	pages      int
	references int
}

func (p *paper) accept(v visitor) {
	v.visitPaper(p)
}

type pageExtractor struct{}

func (v *pageExtractor) visitBook(book *book) {
	fmt.Println("number of the book pages: ", book.pages)
}

func (v *pageExtractor) visitPaper(paper *paper) {
	fmt.Println("number of the paper pages: ", paper.pages)
}

type referenceExtractor struct{}

func (v *referenceExtractor) visitBook(book *book) {
	fmt.Println("number of references in the book: ", book.references)
}

func (v *referenceExtractor) visitPaper(paper *paper) {
	fmt.Println("number of references in the paper: ", paper.references)
}

func main() {

	paper := &paper{
		pages:      8,
		references: 22,
	}

	book := &book{
		pages:      120,
		references: 70,
	}

	pageExtractor := &pageExtractor{}
	paper.accept(pageExtractor)
	book.accept(pageExtractor)

	referenceExtractor := &referenceExtractor{}
	paper.accept(referenceExtractor)
	book.accept(referenceExtractor)

}

// // execution result :
// number of the paper pages:  8
// number of the book pages:  120
// number of references in the paper:  22
// number of references in the book:  70

```