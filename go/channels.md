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