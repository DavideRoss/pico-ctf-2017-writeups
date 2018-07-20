# Leaf Of The Tree
## level 1 - misc - 20 points

### Description
> We found this annoyingly named directory tree starting at /problems/5da315e9c7f1c9886ea371abee5ae8d0. It would be pretty lame to type out all of those directory names but maybe there is something in there worth finding? And maybe we dont need to type out all those names...? Follow the trunk, using cat and ls!

### Hints
> * Tab completion is a wonderful, wonderful thing

### Solution

Connect with `ssh` to the server (or use web terminal) and `cd` into `/problems/88518d23aee7ee21e50bdd8414a404c1`. To find where the flag file is we can use the `find` command:

```
$ cd /problems/5da315e9c7f1c9886ea371abee5ae8d0
$ find -name flag
./trunk/trunke655/trunk8845/trunk9942/trunk2d10/trunk55d8/trunke715/trunkb041/flag
```

Now we have the path, just `cat` it:

```
$ cat ./trunk/trunke655/trunk8845/trunk9942/trunk2d10/trunk55d8/trunke715/trunkb041/flag
42eed2e89ae8b05b56555f65e0ab81aa
```

### Flag
```
42eed2e89ae8b05b56555f65e0ab81aa
```