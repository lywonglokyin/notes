# gcc vs g++ compiler

They are both compiler-drivers of the GNU compiler Collection

## Main differences

1. Main difference is the default library that they link, in short:
`g++` is equivalent to `gcc -xc++ -lstdc++ -shared-libgcc`, which the 1st is a compiler option, 2nd two are the linker options.

2. `gcc` will compile \*.c/\*.cpp files as C and C++ respectively, while `g++` treated both as C++ files. 

3. Extra macros when compiling *.cpp files which is not available for \*.c files for gcc compiler:
```c++
#define __GXX_WEAK__ 1
#define __cplusplus 1
#define __DEPRECATED 1
#define __GNUG__ 4
#define __EXCEPTIONS 1
#define __private_extern__ extern
```