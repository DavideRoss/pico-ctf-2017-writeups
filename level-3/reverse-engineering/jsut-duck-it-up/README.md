# JSut Duck It Up
## level 3 - reverse engineering - 100 points

### Description
> What in the world could this be?!?! [file](./data/file)

### Hints
> * Maybe start searching for uses of these chunks of characers? Is there anything on the Internet with them?

### Solution

First of all, let's check file type:

```
$ file file
file: ASCII text, with very long lines, with no line terminators
```

It's an ASCII file, let's `cat` it:

```
$ cat file
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]...
```

Whoa! What are these symbols? The challenge title provide us another hint: the word **JSut** is not misspelled, but an hint to Javascript language. In JS, thru intensive and repetitive task, you can transform every snippet of code starting from a bad casting of an emtpy array: `![] = false`. Further informations [here](http://www.jsfuck.com/).

To run this (inefficient) code I used `node`:

```
$ node file
undefined:2
alert("aw_yiss_ducking_breadcrumbs_42f0c220")
^

ReferenceError: alert is not defined
    at eval (eval at <anonymous> (/Users/Davide/Documents/Chores/pico-ctf-2017-writeups/temp/file:1:890), <anonymous>:2:1)
    at Object.<anonymous> (/Users/Davide/Documents/Chores/pico-ctf-2017-writeups/temp/file:1:38441)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
    at Function.Module.runMain (module.js:693:10)
    at startup (bootstrap_node.js:188:16)
    at bootstrap_node.js:609:3
```

It's correct, Node.js doesn't have `alert` function, but it prints the flag anyway. You can retrieve the key also `cat`ting the file, pipe it into `pbcopy` (only on MacOS) and paste into the Chrome Inspector.

### Flag
```
aw_yiss_ducking_breadcrumbs_42f0c220
```