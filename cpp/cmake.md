# CMake

CMake is a utility tool that helps creating a corresponding `Makefile` with ease.

## Basic structure

A very basic `CMakeLists.txt` file would look like this:

```cmake
cmake_minimum_required(VERSION 3.10)

# set the project name
project(Tutorial)

# add the executable
add_executable(Tutorial tutorial.cpp)
```

## Other functionalities

### Versioning

With CMake, we can easily specify the version of the executable of our project, and also pass the corresponding number to a config header file for internal use.

Example:

In `CMakeLists.txt`, add the following:
```cmake
project(Tutorial VERSION 1.0)
```

