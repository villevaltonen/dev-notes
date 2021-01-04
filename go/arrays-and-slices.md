### Arrays & Slices
- arrays
    - collection of items with same type
    - fixed size
    - declaration styles
        ```golang
        a := [3]int{1, 2, 3}
        a := [...]int{1, 2, 3}
        var a [3]int
        ``` 
    - accessed via zero-based index
    - len function returns size of array
    - copies refer to different underlying data
- slices
    - backed by array
    - creation styles
        - slice existing array or slice
        - literal style
        - via make-function
            ```golang
            a := make([]int, 10) // create slice with capacity and length == 10
            a := make([]int, 10, 100) // slice with lenght == 10 and capacity == 100
            ```
    - len function returns length to slice
    - cap function returns length of underlying array
    - append function to add elements to slice
        - may cause expensive copy operation if underlying array is too small
    - copies refer to same underlying array
