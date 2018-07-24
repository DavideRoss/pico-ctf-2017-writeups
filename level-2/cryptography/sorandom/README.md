# SoRandom
## level 2 - cryptgraphy - 75 points

### Description
> We found [sorandom.py](./data/sorandom.py) running at shell2017.picoctf.com:55248. It seems to be outputting the flag but randomizing all the characters first. Is there anyway to get back the original flag?
> 
> Update (text only) 16:16 EST 1 Apr Running python 2 (same version as on the server)

### Hints
> * How random can computers be?

### Solution

Open the Python script, the flag string read from a file is ecrypted using some character shifting based on a random generated value. We can notice also that the seed is fixed, so we can generate the identical "random" numbers every time the script will be executed.

Starting with this assertions we can write a script to reverse the encryption once given the input. So connect with `nc`, copy the given "randomized" flag into the script and run it:

```python
import random, string

input = 'BNZQ:1o0yd5jk9h256wdjsu366t10787198h9'
unenc = ''

random.seed('random')

for c in input:
    if c.islower():
        unenc += chr((ord(c) - ord('a') - random.randrange(0, 26)) % 26 + ord('a'))
    elif c.isupper():
        unenc += chr((ord(c) - ord('A') - random.randrange(0, 26)) % 26 + ord('A'))
    elif c.isdigit():
        unenc += chr((ord(c) - ord('0') - random.randrange(0, 10)) % 10 + ord('0'))
    else:
        unenc += c

print unenc
```

```
$ python rev.py
FLAG:8d8ca6db4f519edeab492d55203467d9
```

### Flag
```
FLAG:8d8ca6db4f519edeab492d55203467d9
```