# lvalue and rvalue

In simple terms, lvalue represents values that have names and exist in the memory which can be called. rvalue represents value that is temporary and will soon be destroyed.

## Types of values
There are three primary types of value in c++:
- lvalue: designates a function or an object.
- xvalue ("eXpiring" value): refers to object near the end of its lifetime
- glvalue ("Generalized" lvalue): refers to lvalue or xvalue
- rvalue: An xvalue, a temporary object, or sub-object, or a value that is not associated with any object
- prvalue ("pure" rvalue): refers to rvalue that is not an xvalue

Examples:
#### lvalue
- name of function, variable, or data member
- pre-increment/decrement expression `++a`, `--a`
- `*p` indirection expression
#### prvalue
- literals such as `42`, `true` or `nullptr`
- non-reference return type functions, such as `str1 + str2`
- post-increment/decrement expression `a++`, `a--`
- `&a` address of expression
#### xvalue
- functions whose return type is a rvalue reference to object, such as `std::move(x)`
## rvalue reference
### When RVO is not available
Consider three user-defined struct which represents vectors `a, b, c`.
For the simple addition: `c = a + b`, three copying of the struct is needed:

```c++
Vector operator+ (Vector const &target) {
    Vector temp(*this) // first copying
    temp += target     // efficient operator+= requires no copying
    return temp        // returning the temp requires copying
}
```
Then,
```c++
c = a + b // Value returned by operator+ is a rvalue which will then be copied to c
```
We can observe that many unnecessary copying is done, and things worsen for statement like `d = a + b + c`, which need total of 5 copying operation.

```c++
t = (a+b) // 1st and 2nd copying (t is a rvalue thus no return value assignment copy is needed)
d = (t+c) // 3rd to 5th copying
```



### When RVO is available
Modern compiler includes optimization called Return Value Optimization (RVO) for statement like `c = a + b` which will directly assign the rvalue of `(a+b)` to the variable c. It reduced the number of copying needed to 2.

```c++
t = (a+b) // 1 copying needed to copy value of a to temp
d = (t+c) // 1 copying needed to copy value of t to temp
```
We can see that the second copying is not necessary as `t` is already a temporary value. Then it comes the move semantic.

### Move operator

Move generally does the following two things:
1. Steal the data from an object
2. Trick the object we steal into forgetting everything

In this way, we can 

https://stackoverflow.com/questions/9851188/does-it-make-sense-to-use-move-semantics-for-operator-and-or-operator

https://shininglionking.blogspot.com/2019/02/c-how-to-write-reliable-code.html#more