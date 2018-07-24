# HashChain
## level 3 - cryptography - 90 points

### Description
> We found a service hiding a flag! It seems to be using some kind of MD5 Hash Chain authentication to identify who is allowed to see the flag. Maybe there is a flaw you can exploit? [hcexample.py](./data/hcexample.py) has some example code on how to calculate iterations of the MD5 hash chain. Connect to it at shell2017.picoctf.com:47404!

### Hints
> * Connect from the shell with nc. Read up on how Hash Chains work and try to identify what could make this cryptosystem weak.

### Solution

An hash chain is a function that hashes *n* times the same input. Use `nc` to connect to `shell2017.picoctf.com:47404`:

```
$ nc shell2017.picoctf.com 47404

*******************************************
***            FlagKeeper 1.1           ***
*  now with HASHCHAIN AUTHENTICATION! XD  *
*******************************************

Would you like to register(r) or get flag(f)?

r/f?
```

Choosing `r` give you your ID and ask for the hash before to validate:

```
Hello new user! Your ID is now 1112 and your assigned hashchain seed is 20d135f0f28185b84a4cf7aa51f29500
Please validate your new ID by sending the hash before this one in your hashchain (it will hash to the one I give you):
b7793b04ca5185eacd338d0c6660cf0b
```

They give us our ID, the seed and the final hash, which we have to find the previous one. An important thing to notice is that `md5(ID) == seed`, in this case `md5(1112) == 20d135f0f28185b84a4cf7aa51f29500`, it will be useful later in the challenge. We can retrieve the previous hash using a `while` cycle with Python:

```python
import md5

seed = '1112'
input = 'b7793b04ca5185eacd338d0c6660cf0b'

hashc = seed

while hashc != input:
    hashc = md5.new(hashc).hexdigest()
    print hashc
```

This will be the result:

```
$ python hash-chain.py
20d135f0f28185b84a4cf7aa51f29500
109133ae45b97b8cfb20ccc6f9988430
64b03a900cb2fc4aaa16d974b625125f
28e21966e19378ab80e7a83624ccfb7d
ef1f95d730ddf7bcc3fbec6e8ff16840
e838d39800d77c19938b86eb2b44e914
82c9203007e01ce0180d4c0d7bc275eb
236d4f18d92aa1379f6af7067a62fe7d
eaa5e682da1a02a3b8f2168bc91e5561
29fadc8785e07922d3b00376da8c142e
22b3e2387114370c37bc8c572ed989b5
814aa6c6bf5cfbf2483c1f3713b63ea3
c86ca6fbf183fd560e6ee34cd6386efc
a00117a52fb289141746c340b9fa69a3
6d4f66406508e7eb7217a112e0d21957
6548d388300937a246dc8f1a8989c519
29854cf9a6e88d3ff5bbf2b490ae4fc8
b7793b04ca5185eacd338d0c6660cf0b
```

The second-last hash is the correct one, `29854cf9a6e88d3ff5bbf2b490ae4fc8`, and submitting to `nc` will give you a confirmation and disconnect.

Connect again and choose `f`:

```
This flag only for user 48
Please authenticate as user 48
03eb039829fa0577162316b9341f33ad
```

Now you have the user ID and the final hash and we now that the seed is the hash of ID, so you have to find the previous hash using the Python script. Change parameters, run it and copy the second-last hash, and it will give you this message:

```
Hello user 48! Here's the flag: 987cd68afbdf22d946461f244bc14391
```

### Flag
```
987cd68afbdf22d946461f244bc14391
```
