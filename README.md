# Golang Cheatsheet

## Packages

```go
package main
```

Programs are run from the 'main' package

## Importing Packages

```go
import(
	"fmt"
	"math/rand"
)

//or

import "fmt"
import "math/rand"
```

## 'Name' rules

* Names are exported (public) if they begin with a capital letter. 
* Unexported are private

## Types

* bool
* string
* int  
* int8
* int16
* int32
* int64
* uint 
* uint8
* uint16
* uint32
* uint64
* uintptr
* byte // alias for uint
* rune // alias for int32. represents a Unicode code point
* float32 
* float64
* complex64
* complex128

### Conversions

```go
var x, y int = 3, 4
var f float64 = math.Sqrt(float64(x*x + y*y))
var z uint = uint(f)
//[Type]([Value]) converts value to given type

//simpler:
i := 42
f := float64(i)
u := uint(f)
```

### Inferrence from right hand side

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

Type inferred from the precision

## Variables

```go
var MyName string = "Alex Roan"
var aNumber, anotherNumber int
var aBoolean, anotherBoolean = true, false
```

* Multiple declarations and initializers in one line
* Also different type initializers on one line:

```go
//bool, bool, string
var c, python, java = true, false, "no!"
```

Variables not initialized with a value are given their zero value (not null):
* 0 for numeric types
* false for boolean
* "" for string

### := instead of 'var'

```go
MyName := "Alex Roan"
```

Can only be used inside a function because outside everything begins with a keyword

### Constants

like variables (var), but with 'const' keyword

```go
const name = "Alex Roan"
```

## Functions

```go
func main(){
	//do something
}

//with params and returns

//func [name]([parameterName] [parameterType]) [returnType]
func add(x int, y int) int{
	//do something
}
```

### Consecutive same-type params

```go
func add(x, y int) int{
	//do something
}
```

### Multiple returns

```go
func swap(x, y string) (string, string) {
	return y, x
}
a, b := swap("hello", "world")
```

### Named multiple returns

```go
//used as if variables being declared at beginning of function
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return //<- naked return
}
```

