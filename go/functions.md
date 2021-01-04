## Functions
- basic syntax
```golang
func foo() {
    ...
}
```
### Parameters
```golang
// comma delimited list of variables and types
func foo(bar string, baz int)

// parameters of same type list type once
func foo(bar, baz int)

// when pointers are passed in, the function can change the value in the caller
this is always true for data of slices and maps

// use variadic parameters to send list of same types in (must be last parameter, received as a slice)
func foo(bar string, baz ...int)
```
### Return values
```golang
//single return values just list type
func foo() int

// multiple return value list types surrounded by parentheses
// the (result type, error) paradigm is a very common idiom
func foo() (int, error)

// return using return keyword on its own can return addresses of local variables
// automatically promoted from local memory (stack) to shared memory (heap)
return
```
### Anonymous functions
- functions don't have names if they are:
```golang
// immediately invoked
func() {
    ...
}()
// assigned to a variable or passed as an argument to a function
a := func() {
    ...
}
a()
```

### Functions as types
- can assign functions to variables or use as arguments and return values in functions
- type signature is like function signature, with no parameter names
```golang
var f func(string, string, int) (int, error)
```

### Methods
- function that executes in context of a type
- format
```golang
func (g greeter) greet() {
    ...
}
```

### Receiver can be value or pointer
- value receiver get copy of type
- pointer receiver gets pointer to type