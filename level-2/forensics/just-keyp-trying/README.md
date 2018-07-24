# Just Keyp Trying
## level 2 - forensics - 80 points

### Description
> Here's an interesting capture of some data. But what exactly is this data? Take a look: [data.pcap](./data/data.pcap)

### Hints
> * Find out what kind of packets these are. What does the info column say in Wireshark/Cloudshark?
> * What changes between packets? What does that data look like?
> * Maybe take a look at [http://www.usb.org/developers/hidpage/Hut1_12v2.pdf?](http://www.usb.org/developers/hidpage/Hut1_12v2.pdf?)

### Solution

```
tshark -r data.pcap -Tfields -e usb.capdata -Y "!(usb.capdata == 00:00:00:00:00:00:00:00)" | cut -d : -f 1,3
```

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
