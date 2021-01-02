### Variables
- variable declaration
    ```golang
        - var foo int
        - var foo int = 42
        - foo := 42
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

### Primitives
- boolean type
    - values are true or false
    - not an alias for other types (e.g. int)
    - zero value is false
- numeric types
    - integers
        - signed integers
            - int type has varying size, but min 32 bits
            - 8 bit (int8) through 64 bits (int64)
        - unsigned integers
            - 8 bit (byte and uint8) through 32 bit (uint32)
        - arithmetic operations
            - addition, subtraction, multiplication, division, remainder
        - bitwise operations
            - and, or, xor, and not
            - zero value is 0
            - can't mix types in same family (uint16 + uint32 = error)
        - floating types
            - follow IEEE-754 standard
            - zero value is 0
            - 32 and 64 bit versions
            - literal styles
                - decimal (3.14)
                - exponential (13e18 or 2E10)
                - mixed (13.7e12)
            - arithmetic operations
                - addition, subtraction, multiplication, division
        - complex numbers
            - zero value is (0+0i)
            - 64 and 128 bit versions
            - built-in functions
                - complex
                - real
                - imag
            - arithmetic operations
                - addition, subtraction, multiplication, division
- text types
    - strings
        - UTF-8
        - immutable
        - can be concatenated with + operator
        - can be converted to []yte
    - rune
        - UTF-32
        - alias for int32
        - special methods normally required to process

### Constants
- immutable, but can be shadowed
- replaced by the compiler at compile time
    - value must be calculable at compile time
- named like variables
- typed constant work like immutable variables
    - can interoperate only with same type
- untyped constants work like literals
    - can interoperate with similar types
- enumerated constants
    - special symbol "iota" allows related constants to be created easily
    - iota starts at 0 in each const block and increments by one
    - watch out of constant values that match zero values for variables
- enumerated expressions
    - operations that can be determined at compile time are allowed
        - arithmetic
        - bitwise operations
        - bitshifting

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

### If & Switch
- if-statements
    - initializer
    - comparison operators
    - logical operators
    - short circuiting
    - if-else-statements
    - if-else-if-statements
    - equality and floats
- switch-statements
    - switching on a tag
    - initializers
    - switches with no tag
    - fallthrough
    - type switches
    - breaking out early

### Looping
- for-statements
    ```golang
    // simple loops
    for initializer; test; incrementer {}
    for test {}
    for {}

    // exiting early
    break
    continue

    //looping over collections (arrays, slices, maps, strings, channels)
    for k, v := range collection {}
    ```

### Defer, Panic & Recover
- defer
    - used to delay execution of a statement until function exists
    - useful to group "open" and "close" functions together
        - be careful in loops
    - run in LIFO-order
    - arguments evaluated at time defer is executed, not at time of called function execution
- panic
    - occur when program cannot continue at all
        - don't use when file can't be opened, unless it is critical
        - use for unrecoverable events - cannot obtain TCP port for web server
    - function will stop executing
        - deferred functions will still fire
    - if nothing handles panic, program will exit
- recover
    - used to recover from panics
    - only useful in deferred functions
    - current function will not attempt to continue, but higher functions in call stack will

# Pointers
- creating pointers
    - pointer types use an asterisk (*) as a prefix to type pointed to
        - *int - a pointer to an integer
    - use the addressof operator (&) to get address of variable
    - dereferencing pointers
        - dereference a pointer by preceding with an asterisk (*)
        - complex types (e.g. structs) are automatically dereferenced
    - create pointers to objects
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

### Functions
- basic syntax
    ```golang
    func foo() {
        ...
    }
    ```
- parameters
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
- return values
    ```golang
    //single return values just list type
    func foo() int

    // multiple return value list types surrounded by parentheses, the (result type, error) paradigm is a very common idiom
    func foo() (int, error)

    // return using return keyword on its own can return addresses of local variables
    // automatically promoted from local memory (stack) to shared memory (heap)
    return
    ```
- anonymous functions
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
- functions as types
    - can assign functions to variables or use as arguments and return values in functions
    - type signature is like function signature, with no parameter names
        ```golang
        var f func(string, string, int) (int, error)
        ```
- methods
    - function that executes in context of a type
    - format
        ```golang
        func (g greeter) greet() {
            ...
        }
        ```
    - receiver can be value or pointer
        - value receiver get copy of type
        - pointer receiver gets pointer to type

### Interfaces
- basics
    ```golang
    type Writer inteface{
        Write([]byte) (int, error)
        }

        type ConsoleWriter struct {}

        func(cw ConsoleWriter) Wrirte(data []byte) (int, error) {
            n, err = fmt.Println(string(data))
            return n, err
        }
    ```
- composing interfaces
    ```golang
    type Writer interface {
            Write([]byte) (int, error)
        }

        type Closer interface {
            Close() error
        }

        type WriterCloser interface {
            Writer
            Closer
        }
    ```
- type conversion
    ```golang
    var wc WriterCloser = NewBufferedWritereCloser()
    bwc := wc.(*BufferedWriterCloser)
    ```
- the empty interface and type switches
    ```golang
    var i interface{} = 0
        switch i.(type) {
        case int:
            fmt.Println("i is an integer")
        case string:
            fmt.Println("i is a string")
        default:
            fmt.Println("I don't know what i is")
        }
    ```
- implementing with values vs. pointers
    - method set of value is all methods with value receivers
    - method set of pointer is all methods, regardless of receiver type
- best practices
    - use many, small interfaces
        - single method interfaces are some of the most powerful and flexible
            - io.Writer, io.Reader, interface{}
    - don't export interfaces for types that will be consumed
    - do export interfaces for types that will be used by package
    - design functions and methods to receive interfaces whenever possible

### Goroutines
- creating goroutines
    - use go-keyword in front of function call
    - when using anonymous functions, pass data as local variable
- synchronization
    - use sync.WaitGroup to wait for groups of goroutines to complete
    - use sync.Mutex and sync.RWMutex to protect data access
- parallelism
    - by default, Go will use CPU threads equal to available cores
    - change with runtime.GOMAXPROCS
    - more threads can increase performance, but too many can slow it down
- best practices
    - don't create goroutines in libraries
        - let consumer control concurrency
    - when creating a goroutine, know how it will end
        - avoids subtle memory leaks
    - check for race conditions at compile time

### Channels
- basics
    ```golang
    // create a channel with make-command
    make(chan int)

    // send message into channel
    ch <- val

    // receive message from channel
    val := <-ch

    // can have multiple senderes and receivers
    // restricting data flow
    // channel can be cast into send-only
    chan <- int

    // or receive-only versions
    receive-only: <-chan int
    ```
    
- buffered channels
    - channels block sender side till receiver is available
    - block receiver side till message is available
    - can decouple sender and receiver with buffered channels
        - make(chan int, 50)
    - use buffered channels when send and receiver have assymetric loading
- for...range loops with channels
    - use to monitor channel and process messages as they arrive
    - loop exits when channel is closed
- select statements
    - allows goroutine to monitor several channels at once
        - blocks if all channels block
        - if multiple channels receive value simultaneuosly, behavior is undefined

### Go modules
```golang
// initialize a project with go modules
go mod init github.com/villevaltonen/<project-repo> 

// clean modules
go mod tidy

// list all modules
go list -m all
```