# Raw2Hex
## Level 1 - reverse engineering - 20 points

### Description
> This program just prints a flag in raw form. All we need to do is convert the output to hex and we have it! CLI yourself to /problems/87c7dd790daa359b529f1a24e9f8763f and turn that Raw2Hex!

### Hints
> * Google is always very helpful in these circumstances. In this case, you should be looking for an easy solution.

### Solution

Connect with `ssh` then `cd` into `/problems/87c7dd790daa359b529f1a24e9f8763f`. Executing `raw2hex` prints this:

```
$ ./raw2hex
The flag is:7JM^B
```

In this case we have to perform the reverse operation: take the output of `raw2hex` and convert in hex format. We have to use `cut` to take only the part after `:`, then convert it to hex:

```
$ ./raw2hex | cut -d : -f 2 | xxd -p
f4f47fac37994a4dbe5e15b9c342891b0a
```

### Flag
```
f4f47fac37994a4dbe5e15b9c342891b0a
```