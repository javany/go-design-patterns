<p align="center">
  <img src="../gopher.png" />
</p>

---

# Flyweight Pattern
A flyweight is an object that minimizes memory usage by sharing as much data as possible with other similar objects.<br />
([more on wikipedia](https://en.wikipedia.org/wiki/Flyweight_pattern))

## Example
Suppose that we have an in-memory database for our online shop. We have thousands of records for our goods.<br />
Our customers search the needed items by some popular filters like name and categories. <br />
In order to have a fast response time we create some indexes based on those keywords. But in order to minimize the memory usage we will not include objects in all indexes. Instead we create a primary index of objects by a unique key like id and use the address to this objects in all other indexes. This is what we see in almost all relational databases.

## Implementation

```go
package main

import "fmt"

type item struct {
	id         uint
	name       string
	model      string
	price      float32
	categories []string
}

var primaryIndex map[uint]item
var nameIndex map[string][]uint
var categoryIndex map[string][]uint

func findItemById(id uint) item {
	if item, ok := primaryIndex[id]; ok {
		return item
	}
	return item{}
}

func findItemsByName(name string) []item {
	if ids, ok := nameIndex[name]; ok {
		var items []item
		for _, id := range ids {
			item := findItemById(id)
			items = append(items, item)
		}
		return items
	}
	return []item{}
}

func findItemsByCategory(cat string) []item {
	if ids, ok := categoryIndex[cat]; ok {
		var items []item
		for _, id := range ids {
			item := findItemById(id)
			items = append(items, item)
		}
		return items
	}
	return []item{}
}

func main() {

	primaryIndex = map[uint]item{
		1: item{
			id:         1,
			name:       "t-shirt",
			categories: []string{"cheap", "summer", "cloth"},
			price:      100,
		},
		2: item{
			id:         2,
			name:       "shorts",
			categories: []string{"summer", "cloth"},
			price:      300,
		},
		3: item{
			id:         3,
			name:       "pants",
			categories: []string{"cloth"},
			price:      500,
		},
		4: item{
			id:         4,
			name:       "t-shirt",
			categories: []string{"cheap", "summer", "cloth"},
			price:      120,
		},
	}
	nameIndex = map[string][]uint{
		"t-shirt": []uint{1, 4},
		"shorts":  []uint{2},
		"pants":   []uint{3},
	}

	categoryIndex = map[string][]uint{
		"cheap":  []uint{1, 4},
		"summer": []uint{1, 2, 4},
		"cloth":  []uint{1, 2, 3, 4},
	}

	// lets find summer goods
	query := findItemsByCategory("summer")

	for _, item := range query {
		fmt.Println(item.name, ": ", item.price)
	}
}

// // execution result :
// t-shirt :  100
// shorts :  300
// t-shirt :  120

```