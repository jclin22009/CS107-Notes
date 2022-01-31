# January 28, 2022

> `**(char*)** char = char*****;`
> 
> —Chris Greggs, 2022 

## When to Use Heap and Stack

- **heap** is used when we need to store large amount of data that could blow out stack

- dynamic construction

- need to control lifetime and resize memory

- Always use **stack** if you can, **heap** when you must

## Generic Pointer

- **`void* pointer`**: generic type of pointer
  
  - cannot dereference or do pointer arithmetic, so we have janky memory calculating workarounds
  
  - For example: a generic implementation of swapping ends

```c
/*
 * You NEED to have WIDTH because void* widths are unspecified
 */
void swap_ends (void *arr, size_t nelems, int width) {

    // allocate space for copy. We always use char, because it's
    // one byte.
    char tmp[width];

    // copy first element to tmp
    memmove(tmp, arr, width);

    // you cannot do pointer arithmetic with void types, so we cast
    // char* is the only one-byte width, so that is why it is used
    // when you do ++ on a char*, it goes forward by 1 byte.
    // (nelems - 1) gets the address of the last element.
    memmove(arr, (char*) arr + (nelems - 1) * width, width);
    memmove((char*) arr + (nelems - 1) * width, tmp, width)
}

int main1() {
    int i_array[] = {...};
    size_t nelems = sizeof(i_array) / sizeof(i_array[0]);

    long l_array[] = {...};
    size_t l_nelems = sizeof(l_array) / sizeof(l_array[0]);

    swap_ends(i_array, i_nelems, sizeof(i_array[0]));
    swap_ends(l_array, l_nelems, sizeof(l_array[0]));
}


int main2(int argc, char **argv) {
    swap_ends(argv, argc, sizeof(argv[0]));
    for (int i = 0; i < argc; i++) {
        printf("%s\n", argv[i]);
    }
    return 0;
}
```

- **Very important:** Generic implementation of getting `ith` element of a `void*`:

```c
for (size t i=0; i < nelems; i++) {
    // get ith element
    void *ith = (char*) arr + i * width;
}
```

## Function Pointers

- Especially when you have `void*`, there are cases where you need the function to do something different for each data type or your need to dereference `void*`. So in order to do that, you PASS IN a function as a pointer. The function you pass in knows how to deal with that specific data type

- Read function pointers from *inside out*.

`void (*myfunc) (int)`

- declare `myfunc` as a pointer to function (int) returning void

`void* (*myfunc)(int*)`

- declare `myfunc` as a pointer to function (pointer to int) returning pointer to void

<img src="file:///Users/jasonlin/Library/Application%20Support/marktext/images/2022-01-30-12-01-05-image.png" title="" alt="" width="576">

## qsort function

`void qsort(void *base, size_t nmemb, size_t size, int (*compar)(const void*, const void*))`

- takes two elements. Returns negative if first < second, zero if elements are equal, positive if second > first
- Since you can see the function pointer which is the parameter, sometimes you need to write the comparison function yourself (in which case qsort sounds kinda redundant)