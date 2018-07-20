# looooong
## Level 1 - misc - 20 points

### Description
> I heard you have some "delusions of grandeur" about your typing speed. How fast can you go at shell2017.picoctf.com:30277?

### Hints
> * Use the nc command to connect!
> * I hear python is a good means (among many) to generate the needed input.
> * It might help to have multiple windows open

### Solution

Use the `nc` command to connect:

```
nc shell2017.picoctf.com 30277
To prove your skills, you must pass this test.
Please give me the 'j' character '728' times, followed by a single '7'.
To make things interesting, you have 30 seconds.
Input:
AAA
WRONG!
```

Connecting a couple of times reveals that we have to write a string with a defined pattern, a character N times followed by a single different character. The easiest method to solve is open two terminal windows, one running `nc` connection and the other one to generate the string using Python/bash/your favourite language.

Instead I preferred solve it automagically with a Python script:

```python
import re, socket

URL = 'shell2017.picoctf.com'
PORT = 30277
CHUNK_SIZE = 1024

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect((URL, PORT))

raw = sock.recv(CHUNK_SIZE)
req = raw.split('\n')[1]
match = re.match(r'Please give me the \'(.)\' character \'(\d+)\' times, followed by a single \'(.)\'', req, re.M|re.I)
resp = match.groups()[0] * int(match.groups()[1]) + match.groups()[2]

sock.send(resp + '\n')
print sock.recv(CHUNK_SIZE)
sock.close()
```

It will connect, parse the request, craft the string and print out the response with the flag:

```
$ python loooooong.py

You got it! You're super quick!
Flag: with_some_recognition_and_training_delusions_become_glimpses_493092611815c4e8f8eee8df7264c4c0
```

### Flag
```
with_some_recognition_and_training_delusions_become_glimpses_493092611815c4e8f8eee8df7264c4c0
```