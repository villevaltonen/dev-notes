## Pointers

### Creating pointers
- pointer types use an asterisk (*) as a prefix to type pointed to
    - *int - a pointer to an integer
- use the addressof operator (&) to get address of variable
- dereferencing pointers
    - dereference a pointer by preceding with an asterisk (*)
    - complex types (e.g. structs) are automatically dereferenced

### Create pointers to objects
```golang
// can use the addressof operator (&) if value type already exists
ms := myStruct{foo: 42}
p := &ms

// use addressof operator before initializer
&myStruct{foo: 42}
```
- use the new keyword
    - can't initialize fields at the same time
- types with internal pointers
    - all assignment operations in Go are copy operations
    - slices and maps contain internal pointers, so copies point to same underlying data