# `time`
1. sleep
2. timeout (once)
3. interval (forever)

## Sleep
```go
import "time"
import "math/rand"

time.Sleep(time.Duration(rand.Intn(2e3)) * time.Millisecond)
```

## Timeout - Onetime job
```go
select {
    case <- time.After(1 * time.Second):
        fmt.Println("your trun now")
    ...
}
```
When `timeout := time.After(5 * time.Second)` is a generator, which is a function that returns a channel

```go
time.AfterFunc(3 * time.Second, func() {
    fmt.Println("just like timeout({}, 3000) in JS")
})
```

## Interval
```go
interval := time.Duration(2) * time.Second
timer := time.NewTicker(interval)
for {
    select {
        // timer is not channel, timer.C is!!
        case <- timer.C : break
    }
}

```

