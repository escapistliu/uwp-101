# Concurrency in Go Language 🚀

This repository provides an in-depth guide to understanding concurrency in the Go programming language. It covers core concepts, detailed features, and practical examples, making it a valuable resource for developers—especially computer science students—looking to master Go's concurrency model for efficient, scalable applications.

## See Also
[Software licence](https://opensource.org/license/mit)

## Setup 🛠️
1. [Install Go](https://go.dev/dl/)
2. In your own Go package, import sexpr package
   ```
   import (
  	"your_path_to_package/sexpr"
    )
   ```

## Concepts 🔗
Concurrency in Go is built around the following fundamental ideas:

- Goroutines: Lightweight threads managed by the Go runtime for concurrent execution.
- Channels: Typed communication pipes that synchronize and pass data between goroutines.
- Select Statement: A mechanism to handle multiple channel operations at once.
- Synchronization Primitives: Tools like mutexes and wait groups to manage shared resources.
These concepts are orchestrated by the Go runtime, which handles scheduling and execution behind the scenes.

## Features 📚
### Goroutines
- Lightweight: Created and managed by the Go runtime, not the OS, with low overhead.
- Dynamic Stack: Starts with a small stack (e.g., 2 KB) that grows as needed.
- Syntax: Use the go keyword to launch a function concurrently.

### Channels
- Typed: Channels specify the data type they carry (e.g., chan int).
- Blocking: Send and receive operations synchronize goroutines naturally.
- Safe: Prevents race conditions by avoiding shared memory access.

### Select Statement
- Multiplexing: Waits on multiple channel operations, executing the first ready one.
- Default Case: Optional non-blocking behavior with a default clause.
- Timeouts: Pair with time.After for time-limited waits.

### Synchronization Primitives
- Mutexes: sync.Mutex locks shared resources to prevent concurrent modification.
- WaitGroups: sync.WaitGroup ensures all goroutines complete before proceeding.

### Go Runtime
- Scheduler: Maps goroutines to OS threads efficiently.
- GOMAXPROCS: Configures the number of OS threads for parallelism (set via runtime.GOMAXPROCS).

## Examples 📝
Here are practical, runnable code snippets showcasing Go’s concurrency features.

### Basic Goroutine 📬
Run a function concurrently:

```
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    fmt.Println("Hello from goroutine!")
}

func main() {
    go sayHello()
    time.Sleep(1 * time.Second) // Wait for goroutine
}
```

Output: 
```
Hello from goroutine!
```

### Channel Communication 📬
Send data between goroutines:

```
package main

import "fmt"

func producer(ch chan<- int) {
    for i := 0; i < 5; i++ {
        ch <- i
    }
    close(ch)
}

func main() {
    ch := make(chan int)
    go producer(ch)
    for val := range ch {
        fmt.Println("Received:", val)
    }
}
```

Output:

```
Received: 0
Received: 1
Received: 2
Received: 3
Received: 4
```

### Select Multiplexing 📬
Handle multiple channels with a timeout:

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(2 * time.Second)
        ch1 <- "Message from ch1"
    }()

    go func() {
        time.Sleep(1 * time.Second)
        ch2 <- "Message from ch2"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        case <-time.After(3 * time.Second):
            fmt.Println("Timeout")
        }
    }
}
```

Output:

```
Message from ch2
Message from ch1
```

### Mutex Synchronization 📬
Protect a shared counter:

```
package main

import (
    "fmt"
    "sync"
)

var counter int
var mutex sync.Mutex

func increment(wg *sync.WaitGroup) {
    defer wg.Done()
    mutex.Lock()
    counter++
    mutex.Unlock()
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go increment(&wg)
    }
    wg.Wait()
    fmt.Println("Final counter:", counter)
}
```

Output: 
```
Final counter: 1000
```

## Best Practices 💡
- Use Channels: Prefer channels over shared memory for communication.
- Keep It Simple: Leverage goroutines and channels for clean, modular code.
- Limit Mutexes: Use sync.Mutex only when necessary to avoid complexity.
- Avoid Leaks: Ensure goroutines terminate (e.g., close channels or use context).

## Real-World Use Cases 🌍
- Web Servers: Handle thousands of requests concurrently (e.g., net/http).
- Pipelines: Process data streams with goroutines and channels.
- Parallel Tasks: Speed up computations like sorting or file processing.

## Conclusion 🎉
Go’s concurrency model—driven by goroutines, channels, and the select statement—offers a powerful, approachable framework for concurrent programming. Supported by an efficient runtime, it simplifies building responsive, scalable applications. This guide provides a foundation to explore and apply these concepts effectively.

For further learning, check out:

- [The Go Programming Language Documentation](https://go.dev/doc/)
- [Go by Example: Concurrency](https://gobyexample.com/goroutines)
- [Effective Go](https://go.dev/doc/effective_go)

























