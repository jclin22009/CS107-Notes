# January 21, 2022: Arrays and Pointers in C

## C Pointers

- `*str` = `str[0]`
- you can do `str[-2]` to go two characters back or `*(str - 2)`
- **pointers**: just a memory address, a number
- `&`: gives the pointer address. i.e.:

```c
int x = 7;
int *xptr = &x;
```

- use *xptr to dereference xptr (gives x value, which is 7)
- **pointer to pointer**: when you follow the address, you get another address
  - useful when you need a pointer of things, like an array of pointers (an array of strings)
  - **strptr gives first character of original str. Or *strptr gives the entire string
  - **need to dereference a double ptr to access entire string for char cspn (relevant to assignment 2)**

```c
int x = 7;
int* xptr = &x;
int** xptrptr = &xptr;
// *xptrptr gives x's address
// **xptrptr gives x's value
```

- dereferencing null or bogus values leads to a _segfault_!

```c
*NULL; // segfault
char* ptr;
ptr[0]; // segfault
```

## Arrays in C

- **arrays**: continuous blocks in memory with fixed length.
  - If it's an array of ints, arr[2] returns 8 bytes down (because ints are 4 bytes each)
  - so to find number of elements in an array:
`int nelems = sizeof(values) / sizeof(values[0]));`
- arrays _ARE NOT_ pointers! You cannot reassign arrays, but you can reassign pointers. However, you can reassign blocks within an array to certain pointer addresses.
- `arr` is the same as `arr[0]`, and it gives the first value of the array
- `int *arrptr = arr;`
- a pointer to an array, like `arrptr`, points to the first element in an array. We need to use pointer arithmetic, like `arrptr++`, to access the other elements inside the array (also like `*(arrptr+1)`)
- don't write stuff like `*arrptr++` for style
- arrays are passed by reference into functions, but you also gotta pass in the array size as a parameter because of how C works
- when you have a const array, you can use the values but can't change them
- check array bounds. C does not give `arrayIndexOutOfBounds` exception, but instead segfaults! You must know how long your array is.

```c
// SAMPLE SWAP FUNCTION
void swapA (int *arr, int index_x, int index_y) {
  int temp = *(arr + index_x); // OR arr[index_x]
  *(arr + index_x) = *(arr + index_y);
  *(arr + index_y) = tmp;
}
```

- alternatively, we can do this with memmove!

```c
void swapB(int *arr, int index_x, int index_y) {
  int tmp;
  memmove(&tmp, arr + index_x, sizeof(int));
  memmove (arr + index_x, arr + index_y, sizeof(int));
  memmove (arr + index_y, &tmp ,sizeof(int));
}
```

## pointers to arrays

- _exercise_: let's swap two pointers in the argv array! argv is an array of char* pointers, so we have `**arr` as a parameter.

```c
void swapA(char **arr, int index_x, int index_y) {
  char *tmp = *(arr + index_x);
  *(arr + index_x) = *(arr + index_y);
  *(arr + index_y) = tmp;
}
```

- let's do the same with `memmove`

```c
void swapB(char **arr, int index_x, int index_y) {
  char *tmp;
  memmove (tmp, *(arr + index_x), sizeof(char*));
  memmove (*(arr + index_x), *(arr + index_y), sizeof(char*));
  memmove (*(arr + index_y), tmp, sizeof(char*));
}
```

## memcpy and memory management

- `void *memcpy (void *dst, const void *src, size_t n)`
  - `void` means it can take any pointer type
  - copies n bytes from src to dst
- `void *memmove (void *dst, const void *src, size_t n)`
  - **USE 99% of the time over memcpy**, unless you know your memory can't overlap. Does essentially the same thing
  - it still copies, doesn't exactly move

## string modification

- two ways to declare strings with multiple chars in C:

```c
char s1[] = "this is a modifiable string cuz it's an array";
char* s1 = "but this cannot be modified"; // ← not exactly sure why
```
