### Maps & Structs
- maps
    - collections of value types that are accessed via keys
    - created via literals or via make-function
    - members accessed via [key] syntax
        ```golang
        myMap["key"] = "value"
        ```
    - check for presence with "value, ok" form of result
    - multiple assignments refer to same underlying data
- structs
    - collections of disparate data types that describe a single concept
    - keyed by named fields
    - normally created as types, but anonymous structs are allowed
    - structs are value types
    - no inheritance, but can use composition via embedding
    - tags can be added to struct fields to describe field
