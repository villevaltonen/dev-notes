## Interfaces

### Basics
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
### Composing interfaces
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

### Type conversion
```golang
var wc WriterCloser = NewBufferedWritereCloser()
bwc := wc.(*BufferedWriterCloser)
```

### The empty interface and type switches
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

### Implementing with values vs. pointers
- method set of value is all methods with value receivers
- method set of pointer is all methods, regardless of receiver type
- best practices
    - use many, small interfaces
        - single method interfaces are some of the most powerful and flexible
            - io.Writer, io.Reader, interface{}
    - don't export interfaces for types that will be consumed
    - do export interfaces for types that will be used by package
    - design functions and methods to receive interfaces whenever possible