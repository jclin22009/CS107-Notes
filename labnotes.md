# Misc. Lab Stuff

## January 19, 2021: Segfault Info

- **segfault** is caused by trying to access memory that can't be accessed (i.e. uninitialized)
  - Valgrind `Conditional jump or move depends on uninitialised value(s)` means it tried to read uninitialized value
