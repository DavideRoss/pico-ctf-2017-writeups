# Leaked Hashes
## level 2 - cryptography - 90 points

### Description
> Someone got hacked! Check out some service's password hashes that were leaked at [hashdump.txt](./data/hashdump.txt)! Do you think they chose strong passwords? We should check... The service is running at shell2017.picoctf.com:30767!

### Hints
> * See if you can crack any of the login credentials and then connect to the service as one of the users. What's the chance these hashes have actually already been broken by someone else? Are there websites that host those cracked hashes? Connect from the shell with nc.

### Solution

Connect with `nc`, the executable will ask for username and password. We have the dump of usernames and their hashed password, suspiciously similar to MD5 hashes. We can use an online [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) to checking if we can reverse an hash, I used [this](https://hashkiller.co.uk/md5-decrypter.aspx). Copy all the hashes with this command

```
$ cat hashdump.txt | cut -d : -f 2 | pbcopy
```

Then paste in the box, solve the captcha and submit. If you have any green result just check the username associated at that hash: now you have username and password.

Then connect again to `nc` and submit credentials:

```
$nc shell2017.picoctf.com 30767
enter username:
lilli
lilli's password: f4n0n
welcome to shady file server. would you like to access the cat ascii art database? y/n y
[...]
     /\__/\
    /`    '\
  === 0  0 ===
    \  --  /    - flag is 073983e70c4fa6c41215007b9e723c50

   /        \
  /          \
 |            |
  \  ||  ||  /
   \_oo__oo_/#######o
[...]
```

Now you have a lot of cute ASCII cats and the flag.

### Flag
```
073983e70c4fa6c41215007b9e723c50
```