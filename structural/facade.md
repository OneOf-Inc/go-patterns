## Facade Pattern
Facade Pattern is classified as a structural design pattern. This design pattern is meant to hide the complexities of the underlying system and provide a simple interface to the client. It provides a unified interface to underlying many interfaces in the system so that from the client perspective it is easier to use. Basically it provides a higher level abstraction over a complicated system.

## Implementation
```
package structural

import (
	"fmt"
)

// CarModel is the first car subsystem which describes the car model.
type CarModel struct {
}

// NewCarModel creates a new car model.
func NewCarModel() *CarModel {
	return &CarModel{}
}

// SetModel sets the car model and logs.
func (c *CarModel) SetModel() {
	fmt.Fprintf(outputWriter, " CarModel - SetModel\n")
}

// CarEngine is the second car subsystem which describes the car engine.
type CarEngine struct {
}

// NewCarEngine creates a new car engine.
func NewCarEngine() *CarEngine {
	return &CarEngine{}
}

// SetEngine sets the car engine and logs.
func (c *CarEngine) SetEngine() {
	fmt.Fprintf(outputWriter, " CarEngine - SetEngine\n")
}

// CarBody is the third car subsystem which describes the car body.
type CarBody struct {
}

// NewCarBody creates a new car body.
func NewCarBody() *CarBody {
	return &CarBody{}
}

// SetBody sets the car body and logs.
func (c *CarBody) SetBody() {
	fmt.Fprintf(outputWriter, " CarBody - SetBody\n")
}

// CarAccessories is the fourth car subsystem which describes the car accessories.
type CarAccessories struct {
}

// NewCarAccessories creates new car accessories.
func NewCarAccessories() *CarAccessories {
	return &CarAccessories{}
}

// SetAccessories sets the car accessories and logs.
func (c *CarAccessories) SetAccessories() {
	fmt.Fprintf(outputWriter, " CarAccessories - SetAccessories\n")
}

// CarFacade describes the car facade which provides a simplified interface to create a car.
type CarFacade struct {
	accessories *CarAccessories
	body        *CarBody
	engine      *CarEngine
	model       *CarModel
}

// NewCarFacade creates a new CarFacade.
func NewCarFacade() *CarFacade {
	return &CarFacade{
		accessories: NewCarAccessories(),
		body:        NewCarBody(),
		engine:      NewCarEngine(),
		model:       NewCarModel(),
	}
}

// CreateCompleteCar creates a new complete car.
func (c *CarFacade) CreateCompleteCar() {
	fmt.Fprintf(outputWriter, "******** Creating a Car **********\n")
	c.model.SetModel()
	c.engine.SetEngine()
	c.body.SetBody()
	c.accessories.SetAccessories()
	fmt.Fprintf(outputWriter, "******** Car creation is completed. **********\n")
}
```
