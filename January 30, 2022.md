# January 30, 2022: qsort and structs addendum

## qsort

`void qsort (void *base, size_t nmemb, size_t size, int (*compar) (const void * const void *));`

- comparison function takes in two void* pointers and compares them.

- e.g. qsort implemented for char* comparison

```c
int compar_str(const void *s1, const void *s2) {
    return strcmp(*(char**)s1), *(char**) s2);
// I don't understand why we need to use char**
}

qsort(argv, argc, sizeof(argv[0]), compar_str);
```

## struct

```c
struct file_info {
    int num_bytes;
    char name[32];
    int visible;
};
```

- `struct` gets passed by value

- **dot-notation** accesses

```c
char* get_name(struct file_info fi) {
    return fi.name;
}
```

- **arrow notation** + ptr changes. Use this almost all the time!

```c
void change_name(struct file_info *fi) {
    strcpy(fi->name, newname);
}
change_name(&fi);
```

- you can also declare structs by heap allocation. This here is an array of structs

```c
struct coordinate *coords = malloc(COORD_COUNT * sizeof(struct coordinate));
```

- cast void* elements in order to convert them into the desired struct type
