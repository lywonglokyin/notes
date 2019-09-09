# Go basics

## Interface

In Go, there is no explicit declaration of implementing an interface. They are implemented implicitly, when a struct implements all methods of a specific interface.

## Interface value

Interface value is like the Go way of implementing polymorphism. They have a type of an interface, and can be assigned types or objects of structs implementing that interface. For example:
```go
func main(){
  var i I //I is the interface

  i = &T{"Hello"} //T is a struct implementing I
}
```
We can view interface value as a tuple of `(value, type)`, where the `value` holds the actual object. and `type` holds the struct type implementing the interface. We can access this information through 
```go
fmt.Printf("%v, %T", i, i)
```

In case the assigned value does not have a concrete value, the `value` of the interface value would be `nil`. An unassigned interface value would have both `value` and `type` as `nil`.

A special interface type would be the empty interface `interface{}`, which specifies 0 methods to be implemented. It means that an empty interface can hold values of any type. It is especially useful for handling unknown values.