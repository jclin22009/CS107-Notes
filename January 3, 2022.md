# January 3, 2022: Intro to C

## C Language Intro

- C was created to write Unix. Can write to systems-level language
- Small, minimalistic.
  - Versus C++: more efficient
- Comparison to C++
  - similar: syntax, logical operators
  - different: no overloading, pass by reference, ADTs, classes/objects
- make files compile code

## SSH commands

- ssh (address)
- ls: list
- cd: open directory
- 2 main editors
  - `vim filename.c` <-- *Gregg uses this one*
  - `emacs filename.c`
  - nano: easy to use but "terrible"

## Let's write a C program

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char* argv[]) {
    printf("Hello, world!");
    return 0;
}
```

- type `make` to compile and `./filename` to run
- `:wq` saves and exits

## Bits and Bytes

- This class is about **0s** and **1s** (binary). It's about the way a computer, at the lowest level, works.
- The more 0s and 1s, the more combinations of digits and therefore the more states
- bits are two states, so they can support $2^n$ number of integers
  - ints are $2^{32}$
- 8 bits is the equivalent of one ASCII character
- **Three types of numbers**
  - unsigned integers: positive integers and zero only
    - gives more space, so we can get higher numbers without erroring / giving false values
  - signed: negative, positive, and zero only
  - floating point: base-2 representation of scientific notation
  - lots of wacky bugs with each type of number!
