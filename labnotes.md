# Misc. Lab Stuff

## January 19, 2021: Segfault Info

- **segfault** is caused by trying to access memory that can't be accessed (i.e. uninitialized)
  - Valgrind `Conditional jump or move depends on uninitialised value(s)` means it tried to read uninitialized value

## January 26, 2022: Memory Allocation

- when you have a pointer to an array, you need to dereference the pointer first in order for `*ptr = arr`
  - `*(ptr + 1) = arr[i]`
  - even if you have a single ptr function `changeFirstLetter char* bruh)`, if you do `*char = 'a'`, that's legal because you did a dereference!

> TLDR: When you **dereference something**, you escape the shackles of function scope and you modify the original!

## February 2, 2022: generic pointers

```c
int abs_func(const void *a, const void *b) {
    int num1 = *(int*) a; // cast to pointer to an integer, then deref!
    int num2 = *(int*) b; // asterisk goes before! (can't deref void 
                          // until you cast it!)
    return abs(a - b);
}
```

- since things passed into `void*` are always POINTERS, then you always have to cast to pointer and dereference in the function itself!!! 
  
  - i.e. the raw value of any variable is never passed into a `void*` -- it's always a pointer -- so dereference and reference accordingly!

- `p ELEM@COUNT` syntax works in GDB!
