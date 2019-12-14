# Async

## Awaitable

An object is awaitable if it can be used in an `await` expression. There are mainly three types:

1. coroutines
2. Tasks
3. Futures

### Coroutines

1. `async def` function
2. coroutine object created by calling coroutine function.

### Tasks

Tasks are used to schedule coroutines **concurrently**.

### Futures

A `Future` is generally a special low level awaitable object. Usually not needed in application level code.