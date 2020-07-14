# `array` and `slice`

## `array`
1. typed `[size]T`
1. on-stack, fixed-size variable, **copied** when passing around
1. contains only `T` ans `size`

```go
var a [4]int
b := [5]int // nil, all zeroed elements
c := [2]string{"1", "2"}
d := [...]string{"1", "2"}
```

## `slice`
1.  typed `[]T`
2.  refer to backing `array`(stack) or `vector`(heap), works like pointers
3.  contains `T`, `size` and `capacity`

```go
a := []string{"a", "b"}
b := make([]string, len, cap)

// when resizing a slice, results in re-allocation and copy over
// expand slice s
t := make([]byte, len(s), (cap(s)+1)*2)
// func copy(dst, src []T) int
// copy(t, s)
// copy based slice's len
for i := range s {
    t[i] = s[i]
}
s = t

func AppendByte(slice []byte, data ...byte) []byte {
    m := len(slice)
    n := m + len(data)

    if n > cap(slice) {
        // expand capacity to double
        newSlice := make([]byte, (n+1)*2)
        copy(newSlice, slice)
        slice = newSlice
    }

    slice = slice[:n]
    copy(slice[m: n], data)
    return slice
}
// func append(s []T, x ...T) []T
```

