# Keyz
## Level 1 - cryptography - 20 points

### Description
> While webshells are nice, it'd be nice to be able to login directly. To do so, please add your own public key to ~/.ssh/authorized_keys, using the webshell. Make sure to copy it correctly! The key is in the ssh banner, displayed when you login remotely with ssh, to shell2017.picoctf.com

### Hints
> * There are plenty of tutorials out there. This one covers key generation: https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html
> * Then, use the web shell to copy/paste it, and use the appropriate tool to ssh to the server using your key

### Solution

Simply add your public key into `~/.ssh/authorized_keys` of your user on the server using web terminal.

You can find your public key using this command:

```sh
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3N[...]x69I1QfB Davide@Macinino.local
```

Once copied open the file `~/.ssh/authorized_keys` on web terminal with `vi` or `nano` and paste your key, save and exit.

Now you can connect directly to the server with `ssh`, the flag will be in the MOTD:

```
$ ssh -i ~/.ssh/id_rsa [username]@shell2017.picoctf.com
Congratulations on setting up SSH key authentication!
Here is your flag: who_needs_pwords_anyways
```

### Flag
```
who_needs_pwords_anyways
```