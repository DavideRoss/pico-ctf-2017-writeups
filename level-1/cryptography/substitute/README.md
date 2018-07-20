# Substitute
## Level 1 - cryptography - 40 points

### Description
> A wizard (he seemed kinda odd...) handed me [this](./data/cipher.txt). Can you figure out what it says?

### Hints
> * There are tools that make this easy this.

### Solution

At a glance you can recognize some distinctive pattern of a substitution cipher, as the title suggests. We can focus on some snippets to estabilish which algorithm they used. In the middle of the text there is a string arranged in a pattern similar to a date:

```
RTETDZTK 6, 1995
```

We can perform some [frequency analysis](https://en.wikipedia.org/wiki/Frequency_analysis), suppose some words and start to build a pattern to use in the `tr` (translate) command:

```
$ cat cipher.txt | tr '[RTEDZK]' '[DECMBR]'
MIE YSAU OL OYGFSBMGDERFCRBHMGCALSOQEMIOL...
```

Then we can continue iterating over translated text and make more suppositions. These were those of my path:

| Original | Translation |
| --- | --- |
| **FGX**EMBER | **NOV**EMBER |
| '**L** | '**S** |
| **B**OVES | **M**OVES |
| MODERN**O**SM | MODERN**I**SM |
| **MI**E | **TH**E |
| O**Y** | O**F** |
| O**W**TSIDE | O**U**TSIDE |
| FLA**U** | FLA**G** |
| B**B** | B**Y** |
| O**HH**ORTUNITIES | O**PP**ORTUNITIES |
| **C**OR**Q** | **W**OR**K** |
| E**V**PRESSION | E**X**PRESSION |

After enough iterations I had this command:

```
$ cat cipher.txt | tr '[RTEDZKFGXLOMIYWSUBHCQV]' '[DECMBRNOVSITHFULGYPWKS]'
HE FLAG IS IFONLYMODERNCRYPTOWASLIKETHIS...
```
A more sane method to retrieve the flag is using [quipquip.com](https://quipqiup.com/), paste the cipher and wait for completion.

### Flag
```
IFONLYMODERNCRYPTOWASLIKETHIS
```