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

## February 9, 2022: Assembly

GDB Register Commands

- `disas`: disassemble in GDB 

- `stepi` and `nexti` are the assembly versions of `step` and `next`

- `info reg` prints out the values in all integer registers.

- `b *[ADDRESS]` lets you set a breakpoint at a specific assembly instruction (e.g. `b *0x400570`) or function offset (e.g. `b *myfn+8`). Offsets are in *bytes*. Make sure you use the `*` here - otherwise GDB will misinterpret the address as a function name!

- `print $[REGISTER]` prints a register's contents. Eg. `print $rax` (note `$` instead of `%`).

- `set $[REGISTER] = VALUE` sets a register's contents. E.g. `set $rax = 9` (note `$` instead of `%`). Useful for testing hypotheses about program behavior!

- `layout split` enters "Text user interface" (TUI) mode with a split code/assembly view for easier viewing. The gdb command `layout <argument>` starts tui mode. The argument specifies which pane you want (`src`, `asm`, `regs`, `split` or `next`). While it can be useful, it can be buggy and sometimes the display gets messed up. Try `refresh` to fix it. `ctrl-x a` will exit tui mode and return you to ordinary non-graphical gdb.

- `x` ("examine") prints out bytes in memory starting at the specified address. (full documentation [here](http://ftp.gnu.org/old-gnu/Manuals/gdb/html_chapter/gdb_9.html#SEC56)). E.g. to print the 8 hex bytes starting at the address in some pointer `ptr`, you could do `x/8bx ptr`. You can specify optional print settings after the slash. `8` is where you specify the number of bytes to print. `b` is where you specify the unit size (`b` is bytes, `w` is words, for instance). `x` is where you specify the print format (e.g. `x` for hex, `d` for decimal).

![](/Volumes/GoogleDrive/My%20Drive/AD_CS%20107/Notes/assets/2022-02-09-12-18-08-image.png)

## February 16, 2022: More Assembly

- `%rip` stores next command, `%rsp` stores address at current top of the stack (which is the lowest address)

- `%rax` stores return values of functions

- `display varname` prints out changes to that variable

- conditional breakpoints: `break 2 if count == 1`

- watchpoints: `watch myvar` reports when `myvar` changes and breaks

## March 2, 2022: Callgrind

- `callgrind` determines memory usage of functions

```c
// Total instruction counts
valgrind --tool=callgrind --toggle-collect='*power*' ./trials

// Annotated instructions by function
callgrind_annotate --auto=yes callgrind.out.[pid] 
// PID is the process number previously outputted by valgrind 
// with calgrind enabled

```
