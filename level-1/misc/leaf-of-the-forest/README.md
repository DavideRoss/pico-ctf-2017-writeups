# Leaf Of The Forest
## Level 1 - misc - 30 points

### Description
> We found an even bigger directory tree hiding a flag starting at /problems/7d91c03dff81a9c95bffb6d69358c92d. It would be impossible to find the file named flag manually...

### Hints
> * Is there a search function in Linux? Like if I wanted to 'find' something...

### Solution

The solution is the same of [Leaf Of The Tree](../leaf-of-the-tree), `cd` the folder, use `find` to search the flag file and `cat` it. We can merge `find` and `cat` in a single command with backticks (assuming there is just one file named `flag`):

```
$ cd /problems/7d91c03dff81a9c95bffb6d69358c92d
$ cat `find -name flag`
7ffb59b2f309c09959ba333d0af88565
```

### Flag
```
7ffb59b2f309c09959ba333d0af88565
```