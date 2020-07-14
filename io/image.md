# `Image`
* [image](https://golang.org/pkg/image/#Image)
* [image/color](https://golang.org/pkg/image/color/)


```go
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}

image.Rect(0, 0, 100, 100) image.Rectangle
image.At(dx, dy)
```