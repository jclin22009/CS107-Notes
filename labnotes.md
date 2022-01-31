# Misc. Lab Stuff

## January 19, 2021: Segfault Info

- **segfault** is caused by trying to access memory that can't be accessed (i.e. uninitialized)
  - Valgrind `Conditional jump or move depends on uninitialised value(s)` means it tried to read uninitialized value

## January 26, 2022: Memory Allocation

- when you have a pointer to an array, you need to dereference the pointer first in order for `*ptr = arr`
  - `*(ptr + 1) = arr[i]`
  - even if you have a single ptr function `changeFirstLetter char* bruh)`, if you do `*char = 'a'`, that's legal because you did a dereference!

> TLDR: When you **dereference something**, you escape the shackles of function scope and you modify the original!
