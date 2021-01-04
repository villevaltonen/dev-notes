## Variables

### Variable declaration
```golang
var foo int
var foo int = 42
foo := 42
```
- can't redeclare variables, but can shadow them
- all variables must be used

### Visibility
- lower case first letter for package scope
- upper case first letter to export
- no private scope

### Naming conventions
- Pascal or camelCase
    - capitalize acronymss (HTTP, URL)
- as short as reasonable
    - longer names for longer lives

### Type conversions
- destinationType(variable)
- use strconv package for strings

### Prints value and type:
```golang
fmt.Printf("%v, &T\n", n, n)
```