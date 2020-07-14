# `sync`
1. **sleep**
2. **lock**

## Sleep
```go
import "time"
import "rand"

time.Sleep(time.Duration(rand.Intn(2e3)) * time.Millisecond)
```

The _mutex_
```go
import (
    "fmt"
    "sync"
    "time"
) 

type ThreadSafeMap struct {
    v map[string]int
    // no need to instantiate?
    mux sync.Mutext
}

// read
func (c *ThreadSafeMap) Value(key string) int {
    // block if not available
    c.mux.Lock()
    // unlock after return
    defer c.mux.Unlock()
    return c.v[key]
}

// write
func (c *ThreadSafeMap) Inc(key string) {
    c.mux.Lock()
    // only one goroutine at a time is allowed
    c.v[key] ++
    c.mux.Unlock()
}

func main() {
    c := ThreadSafeMap{v: make(map[string]int)}

    for i:=0;i<1000;i++ {
        go c.Inc("theKey")
    }
    // block as well
    time.Sleep(time.Second)
    fmt.Println(c.Value("theKey"))
}
```