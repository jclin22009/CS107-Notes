# January 24, 2022: Stack and Heap

!!! abstract
    Overview: We will understand where variables go in memory.

## Before the Lecture

### Double Pointers

> Q: Why do we use double pointers?
>
> A: So our functions can modify original parameters passed into the function.

```c
char nextCharA(char *p) {
    char next = p[0];
    p++; // DOES NOT UPDATE ACTUAL VERSION. Only modifies local parameter,
         // which is *p (reference to p)
}

char nextCharB(char **p) {
    char next = (*p)[0];
    (*p)++; // CALLING FUNCTION SEES CHANGE. Essentially, you
            // need a double pointer to modify original. Although
            // the double pointer modifications might only be 
            // limited to the function, by dereferencing the double
            // pointer we transcend the fickle bounds of scope to
            // actually modify the single-pointer, which is what
            // we want.
}

void main() {
    char* p = "AYO";
    nextCharA(p);
    nextCharB(&p); // this one works
}
```

### Pointers to Arrays

- `char* envp[]` is an array of pointers.
  - `envp[1]` gives a pointer. Follow that pointer to get to the string (no asterisk needed unless you only want the first character)

## Memory Layout

- when you declare a variable, it will be in the stack.
- `stack`: stores declared variables
  - only lasts for the functions (if the function returns, it's all gone!)

```c
// This is all in the stack.

int x;
int *xptr = &x;
int bruh[] = {1,2,3};
char str[] = "ayo";
```

- `heap`: stores memory you need to specifically request from the operating system (sounds bossy and needy!). It's like in C when I had to say `new` to declare something.
  - the heap lives low in memory, the stack lives high in memory. The heap grows up, the stack grows down. Seems like a metaphor like Parasite
  - lasts longer than stack memory. Use `malloc` and other functions to create heap in C
  - **hea**p = **hea**vy so it lasts longer

## Stack Allocation

- when declaring variables, the computer allocates the stack according to the _bitwidths/size_ of each variable! So: I might predict the memory addresses of variables declared in the stack if I know all previous variables declared (along with their bitwidths)
  - Chris says: Naw bruh. When there are alignment issues, the computer might skip a few addresses in the stack. It might vary by compiler
- **stack frames**: C technique where it stores function calls and function-related stack clods of memory in a LIFO manner, where the stack proceeds downwards for new function calls
  - Let's say main(int a, int b) calls averageNum(int a, int b) which calls addNum(int a, int b). The stack will look like this. When addNum() is finished, it gets removed from the bottom of the stack.

*[LIFO]: Last in First Out

| main           | int a, int b |
|-              |-              |
| averageNum    | int a, int b|
| addNum | int a, int b|

- stack allocation, with this system of **stack frames**, is:
  - convenient and fast. Think of the stack as scratch space, where the program can jot down things and trash it when it's done (like a pad of LIFO sticky notes).
  - small (only 8MB by default), size-fixed, limited scope

## Dynamic Allocation (malloc/realloc/free)

- where we request and use memory from the heap.
- `malloc`: allocate memory
- `calloc`: call memory
- `realloc`: try to give me memory for same address. Sometimes can give more
- `free`: I'm done with memory, give it back to OS

### malloc

>`void *malloc (size_t size)`

- size is always in bytes
  - i.e. `int *scores = malloc(20 * sizeof(int)); // allocate array of 20 ints`
  - if malloc returns `NULL`, there wasn't enough memory for the request

### calloc

>`void *calloc(size_t nmemb, size_t size)`

- like malloc, except it takes two parameters and also zeros the memory for you!
- looks like it also does the multiplication for you

### realloc

> `void *realloc(void *ptr, size_t size)`

- takes `ptr` you have already passed in and sometimes gives you more space
- returns ptr to new memory (through void or original ptr?)

### free

> `void free(void *ptr)`

- returns all memory. Freed memory cannot be used again