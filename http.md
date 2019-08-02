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

## Supply HTTP response

A simple way would be to create handler for a given pattern. Package [`net/http`](https://golang.org/pkg/net/http/) provides functions `Handle` and `HandleFunc` to register patterns in **DefaultServeMux**.

### `Handle` function

```go
func Handle(pattern string, handler Handler)
```
It register a `Handler` with a specific pattern.

---

#### [`Handler`](https://golang.org/pkg/net/http/?m=all#Handler)
It is an interface implementing the `ServerHTTP` function.
```go
type Handler interface {
  ServeHTTP(ResponseWriter, *Request)
}
```
`Request` refers to the HTTP request. `ResponseWriter` accepts reply headers and data.

---

On the other hand `HandleFunc` accepts a `func(ResponseWriter, *Request)` directly.

Generally we can write to the `ResponseWriter` using `fmt.Fprintf`.

## HTTP template

With package `html/template`, we can create html file templates for responses. P.S. Use `html/template` instead of `text/template` to prevent script injection.

1. Prepare a template file
2. Parse the template file using `template.ParseFiles()`
   ```go
   t, err := template.ParseFiles("template.html")
   ```
   A helpful wrapper `template.Must()` is available, which throws a panic when `err` is not `nil`.
   ```go
   template.Must(template.ParseFiles(...)) 
   ```
   It is also good practise to parse files only once by storing the template struct for repeated use, in order to reduce parsing times.
3. Write to the `ResponseWriter` using the `Template` struct method `Execute()`, or `ExecuteTemplate` if multiple files are parsed.
   ```go
   t.Execute(w, p)
   ```
   ```go
   t.ExecuteTemplate(w, "template.html", p)
   ```

#### Template file format

The template file is very similar to a typical html file, with the following special cases:

1. Mixing with Go syntax

   To mix in Go statements in the template file, we use the syntax: `{{.STATEMENT}}`. For example

   ```html
   <h1> Heading: {{.Title}}</h1>
   ```
   or
   ```html
   <div> {{printf "%s" .Body}}</div>
   ```

2. 


## Routing

### Using regular expression

Use package `regexp` for a ton of regular expression related function. Common usage:
```go
validPath = regexp.MustCompile("regex")
```
`validPath` would return a Regexp pointer, which could invoke many useful functions.
```go
m = validPath.FindStringSubmatch(r.URL.Path)
```
`m` is a slice of string containing the matches in the parathesizes of the regex.

### Page redirection

Page redirection is as simple as a call to the function `http.Redirect()`. E.g:
```go
http.Redirect(w, r, "new_page.html", http.StatusFound)
```

### Error handling

To handle error from runtime, we can use the `http.Error()` function. E.g:
```go
http.Error(w, err.Error(), http.StatusInternalSeverError)
```

### Directory name different from the name in URL

When using directories to serve HTTP request, sometimes the directories we would provide does not match the URL. (For example, `localhost:8080/tmp/style.css` may seek the file from directory `./asset/`). In this case, we can use the function:
```go
func StripPrefix(prefix string, h Handler) Handler
```
The returned `Handler` will strip the given prefix, then call wrapped handler. Common usage:
```go
http.Handle("/tmp/",http.StripPrefix("/tmp/", http.FileServer(http.Dir("asset"))))
```

## Using directories to serve HTTP request

Sometimes it is beneficial to use directories to serve HTTP request directly, rather than writing to the ResponseWriter directly(?). For example, the use of linking javascript and css. In this case, we can use the `FileServer` function provided.
```go
func FileServer(root FileSystem) Handler
```
Normally we would use `http.Dir` as a concrete implementation of `http.FileSystem`.
![alt text](./assets/Dir_FileSystem.svg "class diagram")

Common usage:
```go
http.FileServer(http.Dir("tmp"))
```