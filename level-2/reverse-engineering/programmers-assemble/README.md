# Programmers Assemble
## level 2 - reverse engineering - 75 points

### Description
> You found a text [file](./data/assembly.s) with some really low level code. Some value at the beginning has been X'ed out. Can you figure out what should be there, to make main return the value 0x1? Submit the answer as a hexidecimal number, with no extraneous 0s. For example, the decimal number 2015 would be submitted as 0x7df, not 0x000007df

### Hints
> * All of the commands can be found [here](https://en.wikipedia.org/wiki/X86_assembly_language) along with what they do.
> * It may be useful to be able to run the code, with test values.

### Solution

Like the previous challenge we have to analyze an assembly file. We have to fill the `XXXXXXX` in line 4 to reach the `good` function. If we take a look at the `loop` function we can extract that it increase `%ebx` registry by `%ecx`, that is `0x6` (as assigned in line 6), then decrement `%eax` and check if it's zero. If the condition is true jump to `fin` and check if `%ebx` is exactly `0x553a`. If it's true jump to the `good` ending.

We can summarize that instructions in `loop` function are a simply multiplication, assigning to `%ebx` the result of the multiplication between `%ecx` and `%eax`. We know that `%eax` must be `0x533a` to get the `good` ending, so we can reverse the formula and extract `%eax`. Divide `0x533a` by `0x6` (21.306 by 6 in decimal) and we have `0xddf`, that's the flag.

### Flag
```
0xddf
```