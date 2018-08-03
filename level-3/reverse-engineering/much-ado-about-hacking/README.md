# Much Ado About Hacking
## level 3 - reverse engineering - 165 points

### Description
> In a lonely file, you find [prose](./data/MuchAdoAboutHacking.spl) written in an interesting style. What is this Shakespearean play? What does it have to say? How could one get it to produce this [ending](./data/ending.txt)?

### Hints
> * Some would say that Shakespearean english is an... esoteric language
> * I swear that this play compiles. However, there are different versions of the shakespeare language. If you get errors when you run spl2c on MuchAdoAboutHacking, then you need to use a different version of the language. There is a fixed version of the language here: https://stackoverflow.com/questions/1948372/compiling-and-executing-the-shakespeare-programming-language-translator-spl2c-on

### Solution

Ok, this is the most insane challenge I encountered in my (limited) CTF career. I knew about Shakespeare programming language, it's an esoteric language born cleary as a joke, but its specifications are at the same time hilarious and terrible.

Let's look at `MuchAdoAboutHacking.spl`. The first part of the program is the title at the top and the character list: they are the variables used in the program. Descriptions are just decorative. Variable names must be characters playing in a Shakespeare operas, or the compiler will output an error.

Then we have the acts and the scenes, they *act* as labels for `goto`s instruction. Titles are purely decorative too.

In every scene we have the stage, in which the characters enter (line 15, command `[Enter Beatrice and Don John]`). Then the characters can modify themselves or each other speaking on the stage: you can assign a variable to zero (`You are nothing!`) or set to a number.

The number calculation is the craziest thing of all: if the first adjective is positive or neutral the base is 1, instead if it's negative is -1. In line 30 (`You are as proud as...`) the base is 1. Then count the other adjectives and for each of those multiply by 2. Take the line 30: `You are as proud as a bold brave gentle noble amazing hero.` in pseudocode translates as `Achilles = 2 * 2 * 2 * 2 * 2` so 32.

Conditionals are made simply asking: `Am I` (line 51) or `Are you` (line 96) must be answered by another character on stage, starting with `If so,...`.

Another thing: the character are at the same time variables and stacks. You can push into this stack using `Remember...` (line 47) and pop with `Recall...` (line 60).

To read from `stdin` you have to use `Listen to your heart` (not used in the program) to read a number and `Open your mind!` (line 47) to read an ASCII character. Similarly you can use `Open your heart` to print a number and `Speak your mind` (line 93) to print the ASCII character.

I tried to run it locally, but after a couple of tentatives I chose to rewrite the program in Python:

```python
import string

INPUT = 'tu1|\h+&g\OP7@% :BH7M6m3g='
INPUT_INDEX = 0
OUT = ''

class Character:
    def __init__(self):
        self.value = 0
        self.stack = []

    def push(self, val):
        self.stack.append(val)

    def pop(self):
        return self.stack.pop()
    
    def __str__(self):
        return str(self.value) + ' ' + str(self.stack)

    def print_string(self):
        print string.join(list(chr(x) for x in self.stack), '')

benedick = Character()
beatrice = Character()
don_pedro = Character()
don_john = Character()
achilles = Character()
cleopatra = Character()

def scene1():
    don_john.value = 0
    don_pedro.value = 0
    achilles.value = 32
    cleopatra.value = 128 - achilles.value
    benedick.value = 0

def scene2():
    global INPUT, INPUT_INDEX
    c = ord(INPUT[INPUT_INDEX])
    INPUT_INDEX += 1
    
    benedick.value = c
    benedick.push(c)
    beatrice.value += 1

    if benedick.value != 32:
        scene2()
    else:
        beatrice.value -= 1
        benedick.pop()

def scene3():
    global OUT

    benedick.value = benedick.pop()
    benedick.value -= achilles.value
    beatrice.value -= 1
    don_john.value = benedick.value
    benedick.value += don_pedro.value
    benedick.value = benedick.value % cleopatra.value
    don_pedro.value = don_john.value

    benedick.value += achilles.value
    OUT += chr(benedick.value)

    if beatrice.value > 0:
        scene3()

scene1()
scene2()
scene3()

print OUT
```

So, after a first rewrite you can extract the useful things, and write *another* Python script. For every character in the input strings get the ASCII number, subtract the ASCII value of the previous character, add 32 and print into output:

```python
INPUT = 'tu1|\h+&g\OP7@% :BH7M6m3g='

temp = 0
out = ''

for c in INPUT:
    i = ord(c) - 32
    i -= temp

    if i <= 0:
        i += 96
    
    out += chr(i + 32)
    temp = i

print out[::-1]
```

Run it to retrieve the flag.

### Flag
```
Its@MidSuMm3rNights3xpl0!t
```