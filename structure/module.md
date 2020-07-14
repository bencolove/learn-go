# Go Project Structure

-/ repository root
    |-/ module
        |-/ package
    |-/ module

For module import like `import "golang.org/x/tools"`, go will look for `https://golang.org/x/tools`

## Package Management by `mod`
1. init `mod`
2. add dependencies and import them
3. verify dependencies
4. bundle dependencies

### Add dependecies
```shell
$ go get github.com/go-chi/chi@v4.0.1

$ go get github.com/go-chi/chi@master

$ go get github.com/go-chi/chi@08c92af
```

### Download Dependencies
`go build`, `go test` will do it implicitly or explicitly by `go mod download`.

Packages by default will be downloaded to `$GOPATH/pkg/mod` folder.

### Update Package Version
`go get -u` or `go get -u=path`

### Verify Dependencies

### Bundle(vendor) Depencies
`go mod vendor` (should follow `go mod tidy`) will:
1. remove unused imports
1. 

## Build a Module
```shell
$ mkdir hello
$ cd hello
// create go.mod in current folder which
// designate a module prefix
$ go mod init example.com/user/hello 
$ cat go.mod
module example.com/user/hello

go 1.14
$
```

## Where go Pakcages Goes
Environment variables:
* `GOBIN` : if set, binaries will be installed there
* `GOPATH`: if set, binaries will be installed `$GOPATH/bin`

List all `GOPATH`s
`go list -f '{{.Target}}' .`

## Build and Import Custom Packages
>-/ hello/  
>>|- go.mod  
>>> |-/ morestrings/  
>>>>    |- reverse.go  

--reverse.go--  
```go
package morestrings

func ReverseRunes(s string) String {
    //...
}
```

Build the Package
```shell
$ cd ./hello/morestrings
$ go build
```
This compiles the package `morestrings` and save in local build cache.??where

Import the Package  
-- ./hello/hello.go --
```go
package main

import "exmaple.com/user/hello/morestrings"
// ...
```

## Fetch Remote Packages
When `import "github/com/google/go-cmp/cmp"`, go will:
1. download the remote module containing the package
2. record an entry in `go.mod` with its version
    `require github.com/google/go-cmp v0.4.0`
3. dependent modules will be downloaded to `$GOPATH/pkg/mod`, which are **globally** shared

[official]: https://github.com/golang/go/wiki/Modules
[source1]: https://www.honeybadger.io/blog/golang-go-package-management/
[source2]: https://medium.com/@adiach3nko/package-management-with-go-modules-the-pragmatic-guide-c831b4eaaf31