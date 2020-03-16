# Makefile

## Basic structure

Basic structure of `makefile` consists of "rules" with shape:

```makefile
target ... : prerequisites ...
    recipe
    ...
    ...

...

clean:
    ...
```

## Variables

Suppose there is a `makefile`:

```makefile
edit : main.o kbd.o command.o display.o \
              insert.o search.o files.o utils.o
        cc -o edit main.o kbd.o command.o display.o \
                   insert.o search.o files.o utils.o
```

It can be replaces as:

```makefile
objects = main.o kbd.o command.o display.o \
          insert.o search.o files.o utils.o
edit : $(objects)
        cc -o edit $(objects)
```

## Implicit rules

### Implicit compiling

Suppose

```makefile
utils.o: utils.h utils.cpp
    cc -c utils.c
```

can be written as

```makefile
utils.o: utils.h
```

## Cleaning directory

Example:

```makefile
.PHONY: clean  # Avoid makefile classify "clean" as a file
clean:
    -rm edit $(object)
```

Note that it should not be put as the first command else it would be the default command!

