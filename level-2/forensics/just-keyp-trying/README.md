# Just Keyp Trying
## level 2 - forensics - 80 points

### Description
> Here's an interesting capture of some data. But what exactly is this data? Take a look: [data.pcap](./data/data.pcap)

### Hints
> * Find out what kind of packets these are. What does the info column say in Wireshark/Cloudshark?
> * What changes between packets? What does that data look like?
> * Maybe take a look at [http://www.usb.org/developers/hidpage/Hut1_12v2.pdf?](http://www.usb.org/developers/hidpage/Hut1_12v2.pdf?)

### Solution

Here we have another packet capture file. Opening with Wireshark reveals that they are USB packets, from a keyboard to a device. We can retrieve the key pressed filtering data and using the conversion table linked in the hints (page 53).

First thing, every other packet have no data (`00:00:00:00:00:00:00:00`) and they are simply the release key event, we can filter out them. The actual pressed key will be found in the third byte, but we have to keep also the first one: is the byte for modifiers key, so also the shift key.

We can use `tshark` and `cut` to get the data filtered and formatted:

```
$ tshark -r data.pcap -Tfields -e usb.capdata -Y "!(usb.capdata == 00:00:00:00:00:00:00:00)" | cut -d : -f 1,3
00:09
00:0f
00:04
00:0a
...
```

Then we can pipe them into this Python script to translate the bytes into the flag (and some debug code):

```python
import sys

CONVERSION_TABLE = {
    '09': 'f',
    '0f': 'l',
    '04': 'a',
    '0a': 'g',
    '00': '?',
    '2f': '{',
    '13': 'p',
    '15': 'r',
    '20': '3',
    '22': '5',
    '2d': '_',
    '27': '0',
    '11': 'n',
    '1a': 'w',
    '07': 'd',
    '16': 's',
    '1e': '1',
    '21': '4',
    '08': 'e',
    '30': '}',
    '06': 'c'
}

str_mod = ''
str_char = ''
str_byte = ''

for line in sys.stdin:
    data = line.replace('\n', '').split(':')
    char = data[1]

    if data[0] == '01' or char == '00':
        continue

    str_mod += data[0] + ' '
    str_byte += char + ' '

    if char in CONVERSION_TABLE:
        str_char += CONVERSION_TABLE[char] + '  '
    else:
        str_char += '?  '

print str_mod
print str_byte
print str_char
print str_char.replace(' ', '')
```

```
$ tshark -r data.pcap -Tfields -e usb.capdata -Y "!(usb.capdata == 00:00:00:00:00:00:00:00)" | cut -d : -f 1,3 | python usb.py
00 00 00 00 20 00 00 00 00 00 20 00 00 00 00 00 00 00 20 00 00 00 00 00 00 00 00 20
09 0f 04 0a 2f 13 15 20 22 22 2d 27 11 1a 04 15 07 16 2d 20 04 1e 27 1e 20 21 08 30
f  l  a  g  {  p  r  3  5  5  _  0  n  w  a  r  d  s  _  3  a  1  0  1  3  4  e  }
flag{pr355_0nwards_3a10134e}
```

### Flag
```
flag{pr355_0nwards_3a10134e}
```