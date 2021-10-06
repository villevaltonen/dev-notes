## Defer, Panic & Recover

### Defer
- used to delay execution of a statement until function exists
- useful to group "open" and "close" functions together
    - be careful in loops
- run in LIFO-order
- arguments evaluated at time defer is executed, not at time of called function execution

### Panic
- occur when program cannot continue at all
    - don't use when file can't be opened, unless it is critical
    - use for unrecoverable events - cannot obtain TCP port for web server
- function will stop executing
    - deferred functions will still fire
- if nothing handles panic, program will exit

### Recover
- used to recover from panics
- only useful in deferred functions
- current function will not attempt to continue, but higher functions in call stack will