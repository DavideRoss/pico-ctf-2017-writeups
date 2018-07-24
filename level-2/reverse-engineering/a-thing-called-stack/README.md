# A Thing Called Stack
## level 2 - reverse engineering - 60 points

### Description
> A friend was stacking dinner plates, and handed you [this](./data/assembly.s), saying something about a "stack". Can you find the difference between the value of esp at the end of the code, and the location of the saved return address? Assume a 32 bit system. Submit the answer as a hexidecimal number, with no extraneous 0s. For example, the decimal number 2015 would be submitted as 0x7df, not 0x000007df

### Hints
> * Where is the return address saved on the stack?
> * Which commands actually affect the stack?

### Solution

Here we have a snippet of assembly code, our objective is retrieve the offset of `esp` register (stack pointer) at the end of the function.

Assuming we are working on a 32 bit system we know every register is 4 byte long, so every `pushl` will add `0x4` to the `esp`, so at the end of line 6 the offset will be `0x10`.

The next instruction is a subtraction: we have to subtract the value `0xf8` to the stack pointer. We know that the stack grow towards lower address, so actually we have to add `0xf8` to the offset. The final offset will be `0x108`.

Next `movl` instructions will not modify the stack pointer in any way, so we can ignore them. The final stack offset will be `0x108`.

### Flag
```
0x108
```