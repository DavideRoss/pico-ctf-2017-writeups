# ComputeAES
## Level 1 - cryptography - 50 points

### Description
> You found this [clue](./data/clue.txt) laying around. Can you decrypt it?

### Hints
> * Try online tools or python

### Solution

On this challenge we have to decrypt some data encrypted with [AES](https://it.wikipedia.org/wiki/Advanced_Encryption_Standard). We can use `openssl` command, but before we have to the key in hex format with this command:

```sh
$ echo `echo 6v3TyEgjUcQRnWuIhjdTBA== | base64 --decode` | xxd -g 0 -p | sed 's/..$//'
eafdd3c8482351c4119d6b8886375304
```

The `sed` stage is required because there is a `\n` that we don't want.

Then we use `openssl` and the key given from the previous command to decrypt the cipher:

```sh
$ echo "rW4q3swEuIOEy8RTIp/DCMdNPtdYopSRXKSLYnX9NQe8z+LMsZ6Mx/x8pwGwofdZ" | openssl enc -d -aes-128-ecb -nopad -nosalt -base64 -iv 0 -K eafdd3c8482351c4119d6b8886375304
flag{do_not_let_machines_win_983e8a2d}__________% 
```

### Flag

```
flag{do_not_let_machines_win_983e8a2d}
```