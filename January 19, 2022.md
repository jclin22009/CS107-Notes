# Jan 19, 2022: More String Stuff and Arrays

<https://stanford-pilot.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=e308aa9c-c5c8-48c1-a1b4-ae20006e8fb2>

> "It turns out that..."
> 
> —Chris Greggs, again

## String Library

- for `strcat` and `strncat`: create array with lengths of characters + 1
  - `char concatWord(total_len + 1)`
- `strspn(char *str1, char *str2)`: how many characters in initial (contiguous) part of str1 are contained within all of str2
  - **tends to appear on midterms and finals**
- `strcspn(char *str1, char *str2)`: how many characters in initial (contiguous) part of str1 are NOT contained in str2
- `strdup(char *s)`: returns pointer to _heap-allocated_ version of string
  - for some reason, frees pointer when it is not needed
  - `strndup(char *s, size n)`: like strdup, but only copies first n bytes
- c-strings don't keep their own length (except for the 0 at the end)

## Arrays and Pointers

```c
char *s_ptr; // pointer with uninitialized value 
char *s_read_only1 = "hello"; // pointer to read-only "hello" string
char *s_read_only2 "hi there"; // pointer to read-only "hi there" string
char s_array1[10]; // array (on the stack) with fixed space for 10 chars 
char s_array2[] = "hi there"; // array (on the stack) with "hi there" string
```

- printing `s_ptr` will segfault (uninitialized)
- printing `s_array1` might error (uninitialized)
- pointers act like arrays when you use bracket notation
  - i.e. `s_read_only[0]`
- `sizeof()` gives pointer size for char* but actually gives strlen + 1 (the 1 is the trailing 0) for arrays!
- you can't pass an array as a parameter. It always gets converted to a pointer! (40:25)
  - `char *s` = `char s[]`
