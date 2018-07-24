# Little School Bus
## Level 2 - forensics - 75 points

### Description
> Can you help me find the data in this [littleschoolbus.bmp](./data/littleschoolbus.bmp)?

### Hints
> * Look at least significant bit encoding!!

### Solution

The file is a bitmap image, so we can extract each pixel without any encoding, just straight data. The hint suggests us to search for [least significant bit encoding](https://en.wikipedia.org/wiki/Steganography#Example_from_modern_practice), a type of steganography that hide the message in the last bit of every pixel (8 bit per color, 24 bit total).

To retrieve the message I wrote a simple Python script that read the file eight bytes at a time, foreach byte take the least significant bit, then after the eighth byte append the character from the eight bits to a string. Once the file is finished we will have the flag (and a lot of gibberish data):

```python
import struct, os

HEADER_SIZE = 54
DELIMITER = '$'
FILE = 'littleschoolbus.bmp'

fh = open(FILE, 'rb')
fh.seek(HEADER_SIZE)

pixel_count = (os.stat(FILE).st_size - HEADER_SIZE) / 3
bin_string = ''

for i in range(0, pixel_count / 8):
    pixel = fh.read(8)
    pack = struct.unpack('cccccccc', pixel)
    char = ''

    for b in pack:
        char += str(ord(b) & 1)

    bin_string += chr(int(char, 2))

print bin_string
```

```
$ python bus.pu
flag{remember_kids_protect_your_headers_c1a3};lv^Y#Yd=9l;Nm$l[dw|mmO4$"<mݡ/1mb...
```

### Flag
```
flag{remember_kids_protect_your_headers_c1a3}
```