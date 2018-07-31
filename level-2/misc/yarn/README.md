# Yarn
## level 2 - misc - 55 points

### Description
> I was told to use the linux strings command on [yarn](./data/yarn), but it doesn't work. Can you help? I lost the flag in the binary somewhere, and would like it back

### Hints
> * What does the strings command use to determine if something is a string?
> * Is there an option to change the length of what strings considers as valid?

### Solution

Running `strings` on the file will give us a lot of gibberish, but not a reference to a flag:

```
$ strings yarn
/lib/ld-linux.so.2
U^TR
libc.so.6
_IO_stdin_used
putchar
__libc_start_main
__gmon_start__
GLIBC_2.0
PTRh
[^_]
;*2$"(
GCC: (Debian 4.9.2-10) 4.9.2
GCC: (Debian 4.8.4-1) 4.8.4
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
...
```

As suggested we can change the minimum string length, which default is 4. In the middle of the listing we can notice something peculiar:

```
$ strings -3 yarn
...
D$4
[^_]
Sub
mit
_me
_fo
r_I
_am
_th
e_f
lag
;*2$"(
GCC: (Debian 4.9.2-10) 4.9.2
GCC: (Debian 4.8.4-1) 4.8.4
.symtab
...
```

The flag is stored chunked in three characters in different positions of memory. Join them to get the flag.

### Flag
```
Submit_me_for_I_am_the_flag
```