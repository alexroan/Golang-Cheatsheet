# Golang Cheatsheet

## Project Management

### Directory Structure

* src
	* source files, organised into folders of each project.
* bin
	* runnables
* pkg
	* built package files

#### To create a package: 

create the source files, then call:

* go build /path/to/source/folder (or go build if in the directory)

This will create the package in the pkg folder and can be imported into other projects.

#### To create a runnable project: 

create the source files, then call:

* go install /path/to/course/folder (or go install is in the directory)

This will create the runnables in bin. To run the runnable project files, just call /path/to/runnable/folder.

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

## Loops

### For

```go
for i := 0; i < 10; i++{
	//do something
}

//or, initilization and increment are optional
sum := 0
for ; sum < 10; {
	//do something
}
```

For actually takes the place of While:

```go
sum := 0
for sum < 10{
	//do something
}
```


Infinite loop:
```go
for {
	//do something
}
```

## Arrays

Expression:

```go
// var name [n]T
var variableName [10]int
```

Arrays cannot be resized

Array literal:

```go
[3]bool{true, true, false}
```

### Slices

Slices are portions of arrays that can be resized. To get a slice from an array:

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4] //start at index 1, show everything before index 4
//var s []int = primes[0:5] //gets the first 5
fmt.Println(s)
```

They are references to arrays, they do not store data. Changing the elements of a slice changes the underlying array, and any other slices referencing the same portion of the array will obviously display those differences.

Zero value of a slice is nil, and has no underlying array.

Slice literals:

```go
[]bool{true, true, false}
```

This builds an underlying array, then a slice that references it. Example:

```go
s := []struct {
	i int
	b bool
}{
	{2, true},
	{3, false},
	{5, true},
	{7, true},
	{11, false},
	{13, true},
}
fmt.Println(s)
```

Omitting the lowest or highest bounds when slicing, and it will use the default lowest (0) and desfault highest (length):

```go
var a[10]int

//These all do the same thing
a[0:10]
a[:10]
a[0:]
a[:]
```

* Length
	* The length of a slice is the number of elements in the slice
* Capacity
	* The capacity of a slice is the number of elements in the underlying array

Example with resizing:

```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

### 'make' Keyword

Used to create dynamically-sized arrays.

Example:

```go
a := make([]int, 5) // the len(a) = 5 here
```

A third parameter can be passed to denote the capacity:

```go
b := make([]int, 0, 5) //len(b) = 0, cap(b) = 5
```

Working example:

```go

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}

func main(){
	//make underlying zeroed array (5 long), and create a slice referencing that array (a) with the same length and obsiously capacity 5.
	a := make([]int, 5)
	printSlice("a", a)

	//make underlying array 5 long, create a slice referencing that arrray but withonly a length of 0 (capacity is still 5 because the underlying array has a length of 5)
	b := make([]int, 0, 5)
	printSlice("b", b)

	//take a slice of b, getting only the first 2 elements
	c := b[:2]
	printSlice("c", c)

	//take a slice of c, taking the last 3 elements
	d := c[2:5]
	printSlice("d", d)
}
```

### Slices of Slices

2D Tick Tac Toe board:

```go
board := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}
```

### Appending to Slices

```go
var s []int
s = append(s, 0, 1, 2, 3)
```

If the underlying array doesn't have the capactiy for all the new values, a bigger array will be allocated

## Conditionals

### If

```go
if x < 0 {
	//do something
}
```

Like For loops, if statement can start with a short declaration. The declared variable is only available within the scope of the if:

```go
if v := math.Pow(2, 2); v < 10{
	return v
}
```

### Else

```go
if v := something; v < somethingElse{
	return v
}
else{
	return somethingElse
}
```

### Switch

```go
fmt.Print("Go runs on ")
switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux.")
default:
	// freebsd, openbsd,
	// plan9, windows...
	fmt.Printf("%s.", os)
}
```

Switch without condition is the same as switch(true).

## Other Keywords

### Defer

Defers execution of statement until rest of scope is executed

```go
defer fmt.Println("world")
fmt.Println("Well...")
fmt.Println("hello")	
```

looped defers are stacked LIFO

## Pointers

* Pointer holds memory location
* zero value = nil

```go
var p *int
```
pointer is generated by the & operator:

```go
i := 42
pointer = &i
```

to acces the value of the data being pointed to, use *:

```go
i, j := 42, 2701
p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i
p = &j         // point to j
*p = *p / 37   // divide j through the pointer
fmt.Println(j) // see the new value of j
```

## Structs

```go
type Vertex struct{
	X int
	Y int
}

func main(){
	v := Vertex{1, 2}
	p := &v
	//(*p).X makes sense but is cumbersome, so go allows the following
	p.X = 1e9
	fmt.Println(v)
}
```

### Initializing Structs

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```
