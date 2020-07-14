# `io.Reader`

```go
type interface Stringer {
    String() string
}

type interface Error {
    Error() string
}

func (T) Read(b []byte) (n int, err error)
```

## Stream Reader
```go
r := strings.NewReader("hello, blar blar")

buf := make([]byte, 8)

for {
    n, err := r.Read(buf)
    fmt.Printf("n=%v err=%v buf=%v\n", n, err, buf)
    fmt.Printf("b[:n]=%q\n", buf[:n])
    if err == io.EOF {
        break
    }
}
```