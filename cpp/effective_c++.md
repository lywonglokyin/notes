# Effective C++

## Chapter 1

### Prefer `const`, `enum` and `inline` to `#define`
1. Items defined by preprocessor does not appear in compiler hence will result in confusing error messages

### Use `const` whenever possible

1. For objects or literal, use `const`
2. For pointer, use `const T*` (const data), `T* const` (const pointer) or `const T* const` for different uses.
3. For STL objects like `std::vector::iterator`, use the corresponding const class. E.g. `std::vector::const_iterator`.
4. Functions like `operator+` should return `const` object.
5. Overloading member functions to give a `const` version is sometimes useful. (E.g Non-const version of a getter function allows modification while const version do not.)

### Get around `const` member function: `mutable`

Sometimes we would like to change some of the member data even in a `const` member function. We can add the `mutable` keyword to these data members such that they can be modified.

### Get around code duplication with defining the const and non-const function

It is always bad to duplicate code. In case the const and non-const member function are highly similar, we can use const-cast to get around just like this example, which represents the non-const function:
```c++
return const_cast<char&>(static_cast<const T&>(*this)[pos])
```
Here we have to first cast `this` object into a const version in order for it to invoke the `const` version of `operator[]`. We can do a const-cast to remove the "const-ness".

Note that as `const` function made promise not to change any data member while non-const functions did not, it is always undesirable for `const` functions to call non-const function and then cast for `const` return.

### Initialize data member using member initilization list

In this case, object data member are initialized with copy constructor, instead of being initialized with default constructor, then assign with assignment operator.

### Uncertainty on non-local static member initialization

C++ cannot guarantee the order of initialization across different files. Consider the following example:

*filesystem.h*
```c++
class FileSystem{
  public:
    ...
    std::size_t numDisks() const;
    ...
};
extern FileSystem tfs;
}
```
*directory.h*
```c++
extern Directory tempDir;
```
*directory.cpp*
```c++
Directory::Directory(params){
  std::size_t disks = tfs.numDisks();
}
```
In this case, the `static` objects defined in the .h files are both non-local static object, which we cannot guarantee their initialization order! To tackle this, we introduce **local static objects** by storing `static` objects in functions. What C++ can guarantee is that local static object is initializated when the object's definition is first encountered during a call to that function. Thus for the example we can change into:
*filesystem.h*
```c++
class FileSystem{
  public:
    ...
    std::size_t numDisks() const;
    ...
};
FileSystem& tfs(){
  static FileSystem fs;
  return fs;
}
```
*directory.h*
```c++
Directory& tempDir(){
  static Directory dir(params);
  return dir;
}
```
*directory.cpp*
```c++
Directory::Directory(params){
  std::size_t disks = tfs().numDisks();
}
```
The call to the static `tfs` object becomes a call to the function `tfs()`. And it is probably good to `inline` these wrapping functions.

## Chapter 3

### Allocate resources using resource managing objects

Each `new` comes with a `delete`. In this sense, statements in between this pair may interfere with the execution of `delete` statement and cause memory leak. This is best to use wrapper `auto_ptr`,etc. and let the destructor to delete the dynamic object you want to allocate. Related class:
```c++
std::auto_ptr<T>
std::shared_ptr<T>
boost::scoped_array
boost::shared_array
```
**Note**: To prevent multiple deletion on same object, `std::auto_ptr` has special copying behaviour. For example, `std::auto_ptr<A> newA(oldA)` would renders `oldA` as `null`. To facilitate normal copying of pointers, we can use `std::shared_ptr` which would keep track of total amount of pointers pointing to the object and ensure only one deletion.

**Note**: `std::auto_ptr` and `std::shared_ptr` are not suitable for declaring dynamic arrays. Either use `std::vector` instead or refer to classes offered by `boost` shown above.