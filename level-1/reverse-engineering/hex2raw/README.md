# Hex2Raw
## Level 1 - reverse engineering - 20 points

### Description
> This program requires some unprintable characters as input... But how do you print unprintable characters? CLI yourself to /problems/88518d23aee7ee21e50bdd8414a404c1 and turn that Hex2Raw!

### Hints
> * Google for easy techniques of getting raw output to command line. In this case, you should be looking for an easy solution.

### Solution

Connect with `ssh` to the server (or use web terminal) and `cd` into `/problems/88518d23aee7ee21e50bdd8414a404c1`. The executable `hex2bin` will translate every character given in correnspondant ASCII hex representation:

```
$ ./hex2raw
Give me this in raw form (0x41 -> 'A'):
7ca67167db329a5d1508cc4ad5380678

You gave me:
A
410a
```

The executable wants `7ca67167db329a5d1508cc4ad5380678` in raw form to print the key. You can convert easily the string in data using `xxd`, but part of bytes are not printable. Instead you have to pipe the data directly into `hex2raw`:

```
$ echo 7ca67167db329a5d1508cc4ad5380678 | xxd -r -p | ./hex2raw
Give me this in raw form (0x41 -> 'A'):
7ca67167db329a5d1508cc4ad5380678

You gave me:
7ca67167db329a5d1508cc4ad5380678
Yay! That's what I wanted! Here be the flag:
75d3080d00407fa709c18a6cc69d1edc
```

### Flag
```
75d3080d00407fa709c18a6cc69d1edc
```