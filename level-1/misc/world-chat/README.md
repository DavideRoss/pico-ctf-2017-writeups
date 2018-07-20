# World Chat
## Level 1 - misc - 30 points

### Description
> We think someone is trying to transmit a flag over WorldChat. Unfortunately, there are so many other people talking that we can't really keep track of what is going on! Go see if you can find the messenger at shell2017.picoctf.com:48145. Remember to use Ctrl-C to cut the connection if it overwhelms you!

### Hints
> * There are cool command line tools that can filter out lines with specific keywords in them. Check out 'grep'! You can use the '|' character to put all the output into another process or command (like the grep process)

### Solution

If you connect with `nc` to the URL specified in the description you will be steamrolled by the messages of the users. To print only useful messages you can use the `grep` command:

```
$ nc shell2017.picoctf.com 48145 | grep flag
16:58:09 ihazflag: that girl from that movie totally understands me and my pet sloth to drink your milkshake
16:58:09 ihazflag: A silly panda would like to meet you to make a rasberry pie
16:58:10 whatisflag: my homegirlz want to see me to understand me
16:58:10 personwithflag: Only us give me hope for the future of humanity
16:58:10 noihazflag: your dad has attacked my toes to generate fusion power
16:58:10 ihazflag: A huge moose would like to meet you to drink your milkshake
16:58:11 whatisflag: A small moose wants to steal my sloth to generate fusion power
16:58:11 ihazflag: Anyone but me is our best chance to understand me
16:58:11 ihazflag: We want to see me to help me spell 'raspberry' correctly
16:58:11 noihazflag: your dad would like to meet you to understand me
16:58:11 noihazflag: We will never be able to drink your milkshake
16:58:12 flagperson: this is part 1/8 of the flag - 748a
16:58:12 flagperson: this is part 2/8 of the flag - 3a37
16:58:13 noihazflag: We will never be able to help me spell 'raspberry' correctly
16:58:13 ihazflag: Only us are the best of friends for what, I do not know
```

There are still junk messages, but we learned that the flag is divided in 8 parts, so we can `grep` using `flagperson` as condition and get the full flag:

```
$ nc shell2017.picoctf.com 48145 | grep flagperson
17:01:40 flagperson: this is part 1/8 of the flag - 748a
17:01:41 flagperson: this is part 2/8 of the flag - 3a37
17:01:45 flagperson: this is part 3/8 of the flag - ce62
17:01:52 flagperson: this is part 4/8 of the flag - e537
17:01:58 flagperson: this is part 5/8 of the flag - 4552
17:01:59 flagperson: this is part 6/8 of the flag - c31f
17:02:02 flagperson: this is part 7/8 of the flag - 5319
17:02:04 flagperson: this is part 8/8 of the flag - 30dc
```

Now we have all the parts, just join them with a text editor and we have the challenge.

### Flag
```
748a3a37ce62e5374552c31f531930dc
```