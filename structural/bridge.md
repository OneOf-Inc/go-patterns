# Bridge Pattern
The Bridge design pattern allows you to separate the abstraction from the implementation. It is a structural design pattern. 

There are 2 parts in Bridge design pattern : 
- Abstraction
- Implementation

This is a design mechanism that encapsulates an implementation class inside of an interface class.  

The bridge pattern allows the Abstraction and the Implementation to be developed independently and the client code can access only the Abstraction part without being concerned about the Implementation part.
The abstraction is an interface or abstract class and the implementer is also an interface or abstract class.
The abstraction contains a reference to the implementer. Children of the abstraction are referred to as refined abstractions, and children of the implementer are concrete implementers. Since we can change the reference to the implementer in the abstraction, we are able to change the abstraction’s implementer at run-time. Changes to the implementer do not affect client code.
It increases the loose coupling between class abstraction and it’s implementation.

## Implementation
```
package structural

import "fmt"

// TimeImp is the interface for different implementations of telling the time.
type TimeImp interface {
	Tell()
}

// BasicTimeImp defines a TimeImp which tells the time in 24 hour format.
type BasicTimeImp struct {
	hour   int
	minute int
}

// NewBasicTimeImp creates a new TimeImp.
func NewBasicTimeImp(hour, minute int) TimeImp {
	return &BasicTimeImp{hour, minute}
}

// Tell tells the time in 24 hour format.
func (t *BasicTimeImp) Tell() {
	fmt.Fprintf(outputWriter, "The time is %2.2d:%2.2d\n", t.hour, t.minute)
}

// CivilianTimeImp defines a TimeImp which tells the time in 12 hour format with meridem.
type CivilianTimeImp struct {
	BasicTimeImp
	meridiem string
}

// NewCivilianTimeImp creates a new TimeImp.
func NewCivilianTimeImp(hour, minute int, pm bool) TimeImp {
	meridiem := "AM"
	if pm {
		meridiem = "PM"
	}
	time := &CivilianTimeImp{meridiem: meridiem}
	time.hour = hour
	time.minute = minute
	return time
}

// Tell tells the time in 12 hour format.
func (t *CivilianTimeImp) Tell() {
	fmt.Fprintf(outputWriter, "The time is %2.2d:%2.2d %s\n", t.hour, t.minute, t.meridiem)
}

// ZuluTimeImp defines a TimeImp which tells the time in Zulu zone format.
type ZuluTimeImp struct {
	BasicTimeImp
	zone string
}

// NewZuluTimeImp creates a new TimeImp.
func NewZuluTimeImp(hour, minute, zoneID int) TimeImp {
	zone := ""
	if zoneID == 5 {
		zone = "Eastern Standard Time"
	} else if zoneID == 6 {
		zone = "Central Standard Time"
	}
	time := &ZuluTimeImp{zone: zone}
	time.hour = hour
	time.minute = minute
	return time
}

// Tell tells the time in 24 hour format in Zulu time zone.
func (t *ZuluTimeImp) Tell() {
	fmt.Fprintf(outputWriter, "The time is %2.2d:%2.2d %s\n", t.hour, t.minute, t.zone)
}

// Time is the base struct for Time containing the implementation.
type Time struct {
	imp TimeImp
}

// NewTime creates a new time which uses a basic time for its implementation.
func NewTime(hour, minute int) *Time {
	return &Time{imp: NewBasicTimeImp(hour, minute)}
}

// Tell tells the time with the given implementation.
func (t *Time) Tell() {
	t.imp.Tell()
}

// CivilianTime is a time with a civilian time implementation.
type CivilianTime struct {
	Time
}

// NewCivilianTime creates a new civilian time.
func NewCivilianTime(hour, minute int, pm bool) *Time {
	time := &CivilianTime{}
	time.imp = NewCivilianTimeImp(hour, minute, pm)
	return &time.Time
}

// ZuluTime is a time with a Zulu time implementation.
type ZuluTime struct {
	Time
}

// NewZuluTime creates a new Zulu time.
func NewZuluTime(hour, minute, zoneID int) *Time {
	time := &ZuluTime{}
	time.imp = NewZuluTimeImp(hour, minute, zoneID)
	return &time.Time
}
```
