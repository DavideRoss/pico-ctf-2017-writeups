# ComputeRSA
## Level 1 - cryptography - 50 points

### Description
> RSA encryption/decryption is based on a formula that anyone can find and use, as long as they know the values to plug in. Given the encrypted number 150815, d = 1941, and N = 435979, what is the decrypted number?

### Hints
> * decrypted = (encrypted) ^ d mod N

### Solution

The hint contains the full solution, we can retrieve the decrypted number with Python:

```sh
$ python -c "print pow(150815, 1941) % 435979"
133337
```

### Flag
133337