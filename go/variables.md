### Variables
- variable declaration
    ```golang
    var foo int
    var foo int = 42
    foo := 42
    ```
- can't redeclare variables, but can shadow them
- all variables must be used
- visibility
    - lower case first letter for package scope
    - upper case first letter to export
    - no private scope
- naming conventions
    - Pascal or camelCase
        - capitalize acronymss (HTTP, URL)
    - as short as reasonable
        - longer names for longer lives
- type conversions
    - destinationType(variable)
    - use strconv package for strings
- prints value and type:
    ```golang
    fmt.Printf("%v, &T\n", n, n)
    ```