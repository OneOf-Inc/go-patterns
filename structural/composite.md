## Composite Pattern
Composite pattern is a partitioning design pattern and describes a group of objects that is treated the same way as a single instance of the same type of object. The intent of a composite is to “compose” objects into tree structures to represent part-whole hierarchies. It allows you to have a tree structure and ask each node in the tree structure to perform a task.

## Implementation
```
package structural

import "fmt"

// Component describes the behavior that needs to be exercised uniformly
// across all primitive and composite objects.
type Component interface {
	Traverse()
}

// Leaf describes a primitive leaf object in the hierarchy.
type Leaf struct {
	value int
}

// NewLeaf creates a new leaf.
func NewLeaf(value int) *Leaf {
	return &Leaf{value}
}

// Traverse prints the value of the leaf.
func (l *Leaf) Traverse() {
	fmt.Printf("%v  ", l.value)
}

// Composite describes a composite of components.
type Composite struct {
	children []Component
}

// NewComposite creates a new composite.
func NewComposite() *Composite {
	return &Composite{make([]Component, 0)}
}

// Add adds a new component to the composite.
func (c *Composite) Add(component Component) {
	c.children = append(c.children, component)
}

// Traverse traverses the composites children.
func (c *Composite) Traverse() {
	for i := 0; i < len(c.children); i++ {
		c.children[i].Traverse()
	}
}
```
