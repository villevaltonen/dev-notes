## Garbage collection

### General
- A garbage collector is responsible for:
    - Allocating memory
    - Ensuring that any referenced bojects remain in memory
    - Recovering memory used by objects that are no longer reachable from references in executing code
- Referenced objects are considered ```live``` and no longer referenced are considered dead and are termed ```garbage```
- Space allocated from a large pool and is referred as ```heap```
- Garbage is collected when heap is full or reaches a certain threshold

### Design choices
- Serial versus parallel
- Concurrent versus stop-the-world
- Compacting versus non-compacting vs copying

### Performance metrics
- Throughput
- Garbage collection overhead
- Pause time
- Frequency of collection
- Footprint
- Promptness

### Generational collection
- Memory is divided into generations
- Separate pools for objects of different ages

### JVM garbage collectors (Hotspot JVM)
- Serial collector
    - If your applicatoin does not have a requirement for low pause times
- Parallel collector
    - If you have more than one CPU available and no pause time constraints
- Parallel compacting collector
    - If you have more than one CPU available and pause time constraints
    - Not suitable for large shared machines
- Concurrent mark-sweep (CMS) collector
    - If you need shorter garbage collection pauses and can afford to share processor resources with the garbage collector while application is running
    - Applications with a relatively large set of long-lived data and run on machines with two or more processors usually benefit of using CMS collector
    - Should be considered for any application with low pause time requirement

### Tools for evaluating garbage collection performance
- -XX:+PrintGCDetails cli-option
- -XX:+PrintGCTimeStamps cli-option
- jmap
- jstat
- HPROF: Heap profiler
- HAT: Heap analysis tool

### Key options for garbage collection
- Garbage collector selection
- Garbage collector statistics
- Heap and generation sizes
- Options for Parallel and Parallel compacting collector
- Options for CMS collector