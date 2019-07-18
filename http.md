# Creating web application with Go

## File I/O

As simple as using the function provided in package [`io/ioutil`](https://golang.org/pkg/io/ioutil/), namely `WriteFile` and `ReadFile`.

## Open a listening port

Can utilize the function provided in [`net/http`](https://golang.org/pkg/net/http/)

```go
func ListenAndServe(addr string, handler Handler) error
```
Typically would pass nil to handler, such that DefaultServeMux would be used. Then we can add our own handlers with `Handle` or `HandleFunc` later.

Can also log the panic and fatal returned by the `ListenAndServe` function using `Fatal` in package [`log`](https://golang.org/pkg/log/). E.g.:
```go
log.Fatal(http.ListenAndServe(":8080", nil))
```

## Supply HTTP file

A simple way would be to create handler for a given pattern. Package [`net/http`](https://golang.org/pkg/net/http/) provides functions `Handle` and `HandleFunc` to register patterns in DefaultServeMux.