# Reliable code in C++

## RAII

Resource Acquisition Is Initialization (RAII) is a C++ programming technique.

### Cause
Sometimes, it is difficult to ensure the disposal or status of an object within the code. E.g.:
```c++
std::mutex m;

void bad(){
  m.lock();                       //lock the mutex
  f();                            //if f() throws exception, the mutex is never released
  if (!everything_ok()) return;   // if early return, mutex also not released
  m.unlock();
}
```