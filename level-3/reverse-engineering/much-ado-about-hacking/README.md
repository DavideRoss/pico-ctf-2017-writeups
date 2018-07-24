# Much Ado About Hacking
## level 3 - reverse engineering - 165 points

### Description
> In a lonely file, you find [prose](./data/MuchAdoAboutHacking.spl) written in an interesting style. What is this Shakespearean play? What does it have to say? How could one get it to produce this [ending](./data/ending.txt)?

### Hints
> * Some would say that Shakespearean english is an... esoteric language
> * I swear that this play compiles. However, there are different versions of the shakespeare language. If you get errors when you run spl2c on MuchAdoAboutHacking, then you need to use a different version of the language. There is a fixed version of the language here: https://stackoverflow.com/questions/1948372/compiling-and-executing-the-shakespeare-programming-language-translator-spl2c-on

### Solution

```python
import string

INPUT = 'tu1|\h+&g\OP7@% :BH7M6m3g='
INPUT_INDEX = 0
OUT = ''

class Variable:
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

benedick = Variable()
beatrice = Variable()
don_pedro = Variable()
don_john = Variable()
achilles = Variable()
cleopatra = Variable()

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