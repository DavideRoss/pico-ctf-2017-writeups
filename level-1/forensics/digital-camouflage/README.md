# Digital Camouflage
## Level 1 - forensics - 50 points

### Description
> We need to gain access to some routers. Let's try and see if we can find the password in the captured network data: [data.pcap](./data/data.pcap).

### Hints
> * It looks like someone logged in with their password earlier. Where would log in data be located in a network capture?
> * If you think you found the flag, but it doesn't work, consider that the data may be encrypted.

### Solution

With the `file` command we can confirm that the file is a `tcpdump` capture file:

```
$ file data.pcap
data.pcap: tcpdump capture file (little-endian) - version 2.4 (Ethernet, capture length 262144)
```

One of the most famous software to handle `.pcap` files is Wireshark. We can open the file with it, filter the `POST` requests with `http.request.method == "POST"` and look for the password.

We can solve it using `tshark`, the Wireshark CLI:

```sh
$ tshark -r data.pcap -Y "http.request.method==POST" -Tfields -e urlencoded-form.value
grassers,cHJ2cUJaTnFZdw==
```

The password has the final `==` revealing that it is a base64 encoded string, we can decode with this command:

```sh
$ echo cHJ2cUJaTnFZdw== | base64 -D
prvqBZNqYw%
```

Be careful to remove the final `%` added by the command.

### Flag
```
prvqBZNqYw
```