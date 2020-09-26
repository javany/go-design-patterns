<p align="center">
  <img src="../gopher.png" />
</p>

---

# Prototype Pattern
This pattern provides a way to create copies of objects. <br /> 
This pattern is commonly used when the creation process of an object is complex or costly.<br />
([more on wikipedia](https://en.wikipedia.org/wiki/Prototype_pattern))


## Implementation

```go
package main

import "fmt"

type prototype interface {
	print()
	clone() prototype
}

type data struct {
	records []int
}

func (d *data) print() {
	for _, i := range d.records {
		fmt.Println(i)
	}
}

func (d *data) clone() prototype {

	copySlice := make([]int, len(d.records))
	copy(copySlice, d.records)

	return &data{
		records: copySlice,
	}
}

func main() {

	original := &data{
		records: []int{12, 14, 18, 20},
	}

	cloned := original.clone()

	fmt.Println("original object records:")
	original.print()
	fmt.Println("cloned object records:")
	cloned.print()

}

```

