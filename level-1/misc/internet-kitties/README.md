# Internet Kitties
## Level 1 - misc - 10 points

### Description
> I was told there was something at IP shell2017.picoctf.com with port 40660. How do I get there? Do I need a ship for the port?

### Hints
> * Look at using the netcat (nc) command!
> * To figure out how to use it, you can run "man nc" or "nc -h" on the shell, or search for it on the interwebz

### Solution

Basic question, we can solve it with the `nc` command:

```
$ nc shell2017.picoctf.com 40660
Yay! You made it!
Take a flag!
fba9c41f9f0326b53919a2ab1ff20a69
```

### Flag
```
fba9c41f9f0326b53919a2ab1ff20a69
```