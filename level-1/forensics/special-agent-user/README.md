# Special Agent User
## Level 1 - forensics - 50 points

### Description
> We can get into the Administrator's computer with a browser exploit. But first, we need to figure out what browser they're using. Perhaps this information is located in a network packet capture we took: [data.pcap](./data/data.pcap). Enter the browser and version as "BrowserName BrowserVersion". NOTE: We're just looking for up to 3 levels of subversions for the browser version (ie. Version 1.2.3 for Version 1.2.3.4) and ignore any 0th subversions (ie. 1.2 for 1.2.0)

### Hints
> * Where can we find information on the browser in networking data? Maybe try [reading up on user-agent strings](http://www.useragentstring.com/).

### Solution

Like in [digital-camouflage](../digital-camouflage/README.md) we can define `data.pcap` format using `file` command:

```sh
$ file data.pcap
data.pcap: tcpdump capture file (little-endian) - version 2.4 (Ethernet, capture length 262144)
```

The key is part of the `User-Agent` string, we can retrieve all strings using `tshark`:

```sh
$ tshark -r data.pcap -Y http -Tfields -e http.user_agent | sort | uniq
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.137 Safari/4E423F
Wget/1.16 (linux-gnu)
```

The user agent string we want is the second one and the browser used is Chrome 34.0.1847.

### Flag
```
Chrome 34.0.1847
```