# Volatile keyword
Mainly used to counteract the optimization carried out by compilers. Useful for developing embedded systems, or device drivers, where read or write to a **memory-mapped hardware device** is needed. Do NOT use `volatile` for synchronization. Use `atomic` instead.

## Optimization done by compilers

Depends on different compilers, several optimization will be used, such as
- Rearrange expressions that has seemingly no effect
- Skip expressions that would not be executed
- Combine multiple memory access of the same address into one

These optimiztion make cause problem for programs, especially multi-threaded ones. Then the `volatile` keyword comes in.

## `Volatile` keyword

Having the `volatile` keyword ensures the compiler do not optimize expression related to the object.
https://liam.page/2018/01/18/volatile-in-C-and-Cpp/

https://stackoverflow.com/questions/4437527/why-do-we-use-volatile-keyword