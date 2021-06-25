# Development

This file includes tips and tricks that are important during the development of a project.

## ETC: Easy to change (TPP P.28)

### What

Prefer code that are easy to change

### Why

Because the project likely keeps changing (new features, bug fixing, etc).

### How

- No specific methods. For every commit/save, ask if the changes make the future changes to the system easier.

- There are no final decisions. Expect architechtures, deployments, etc to change over time.

## DRY: Don't repeat yourself (TPP P.30)

### What

Every piece of **knowledge** must have a single, unambiguous, authoritative representation within a system (**knowledge** means more than code). A counter-example would be when one part of the project get updated, another part also need to follow some changes.

### Why

Make project ETC. Avoid errors when two pieces of knowledge is not syncronized.

### How

- Usually code duplication violates DRY (sometimes, code duplication due to **coincidence**, when the two piece of code represents different knowledge).

- Avoid useless documentation (repeating trivial implmentation in docs)

- Avoid duplication of data (e.g. Point: {start, end, length} vs {start, end, **length()**}). For performance issue, use specific cache notations instead of treating the repeated value as separated knowledge. (Meyer's Uniform Access principle)

- For duplication among developers in a team:

    - More meetings

    - Assign a "project librarian", that is, to facilitate the exchange of knowledge, and to manage a central place in the source tree where utility routines and scripts resides.

## Representational duplication

### What

Similar to DRY, but these duplications is due to API or remote calls to other libraries. Both your code and the library knows the interface. (Your code) -> (interface) <- (library)

### How

- For duplications across internal APIs, store all your APIs in a central repository. (?)

- For duplication across external APIs, use OpenAPI service that allow you to import the API spec into the local API tools and integrate more reliably. (?)

- For duplication with data sources, generate containers for data with schema provided directly from the source. Or, represent data in key/value pairs. (?)


## Orthogonality (TPP P.39)

### What

When changes in a part of the project will affect another part.

### Why

Easier to read or write tests, promote reusability, less prone to error due to changes, easier to locate bugs induced by changes.

### How

- Do not rely on changes you cannot control (e.g. social security number as identifier)

- For toolkits and library, alert when the external library create or access local objects in a special way (?), which implies orthogonality.

- Keep code decoupled (see more at decoupling)

- Avoid using global data

- Avoid similar functions, with the help of patterns like **Strategy**.

- Use bug fix reports (Source file related to bug, "second-wave" bugs after first fix) to access the orthogonality of the project

## Design by contract (TPP P.105)

### What

Design methods in a way such that if the routine's preconditions are met, then the routine shall guarantee all postconditions and invariants will be true when it completes.

- Preconditions: Guaranteed by the callor of the method.
- Postcondition: Guaranteed by the routine
- Class invariants: Conditions that are shared across all objects of the same class, and is guaranteed by the routine (or state invariants for functional programming)

## Why

- More reliable (conditions and purpose more clear, easier to reuse)

- Crashing early, if conditions are violated (e.g. as compared to sqrt() function in some language that return NaN instead of crashing if a negative number is supplied.)

## How

- Write contracts (If language has DBC syntax then use it, else write it as documentation)

- Use `asserts` if language has no DBC syntax so that compiler can check the contract for you. (note the difference between `assert` and `exceptions`)

## Avoid catch and release (TPP P.113)

### What

Catch and release refers to the practice that when calling functions from libraries, some developers catch all possible error raisable by the function, then re-raise it with either another custom error, or append a more comprehensive error message.

### Why

- It eclipsed the actual workflow by introducing a lot of clauses
- The resulting code is highly coupled with the library (e.g. if the library changed some of its raised error)

## Avoid statement in asserts (TPP P.116)

### Why

The statement may cause side effects, making program harder to debug.

## The one who allocates is responsible for deallocating (TPP P.118)

### What

Functions or objects that allocate memory, threads, etc, should be the responsible for the deallocation.

### Why

If the responsibility is delegated to other functions, it will become very messy.

### How

- Use scoped allocation (e.g `with ... do`) provided by most modern language.

## Tell, Don't ask (TPP P.132)

### What

The idea is to tell the object to perform action, rather than ask the object for information and compute it ourselves.

### Why

- Components are less coupled, because when changes is needed on the action performed, developers do not need to look through both components, but instead, just the class that the action is told.

### How

- Avoid fetching object attributes then perform actions.

- Avoid volatile method call chains. E.g. `amount = customer.orders.last().totals().amount`. A method is considered volatile if it is a third party library, and has possiblity to change in the future.

### Notes

- An extreme of "Tell, Don't Ask" is probably God classes (?), so sometimes it is necessary to expose additional components (when?).

## Avoid global data (TPP P.135)

### Why

- It is difficult to separate components that use global data from the application.

- Setup code for global environment is needed for testing

### Notes

- Singleton, in some sense, is just a global data with another name.

- Some component **has** to be global, e.g. database, file system, etc. For these components, wrap them inside APIs.


## Inheritance: Programming to an interface, not an implementation (DP P.30)

### What

Two main purpose of inheritance:

1. Reuse existing component's functionality.

2. Polymorphism, defining a family of objects sharing the same interface.

### Why

1. Implementation is detached from the interface, thus it will be easily changable in the future. (Interface is like a contract stating the scopes of the implementation)

2. Clients who uses the packages have the freedom to choose among implementation (as long as they adhere to the interface), and is easy to switch implementations in the future.

3. Implementations can be changed in runtime. (any use cases??)

## Favor object composition over class inheritance (DP P.31)

### Why

1. Inheritance breaks the encapsulation of the parent class.

### How

1. A simple test: Does `TypeB` want to expose the complete interface (all public methods) of `TypeA` such that `TypeB` can be used where `TypeA` is expected? -> **Inheritance**. Does `TypeB` only want some/part of the behavior exposed by `TypeA`? -> **Composition**.
