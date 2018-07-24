# Connect The Wigle
## level 3 - forensics - 140 points

### Description
> Identify the data contained within [wigle](./data/wigle) and determine how to visualize it. Update 16:26 EST 1 Apr If you feel that you are close, make a private piazza post with what you have, and an admin will help out.

### Hints
> * Perhaps they've been storing data in a database. How do we access the information?
> * How can we visualize this data? Maybe we just need to take a step back to get the big picture?
> * Try zero in the first word of the flag, if you think it's an O.
> * If you think you're super close, make a private piazza post with what you think it is.

### Solution

```
$ file wigle
wigle: SQLite 3.x database, last written using SQLite version 3008007
```

```
sqlite3 wigle
SQLite version 3.19.4 2017-08-18 19:28:12
Enter ".help" for usage hints.
sqlite> .tables
android_metadata  location          network
```

```
sqlite> select * from location;
0|6b:bf:fe:7f:56:ad|-85|-30.0|94.04|147.0|6.0|1490964952
1|c8:82:08:70:f3:2e|-93|-29.99|94.04|138.0|8.0|1490956968
2|9f:8e:9a:cb:9f:5d|-94|-29.98|94.04|134.0|8.0|1490956758
3|5b:ac:bb:09:f3:41|-70|-29.98|94.045|148.0|12.0|1490960063
4|1a:47:5f:1b:c9:0f|-79|-29.98|94.05|143.0|4.0|1490965978
5|a1:14:c9:f6:94:41|-82|-29.97|94.04|135.0|16.0|1490955568
6|c9:61:96:ca:43:7b|-77|-29.96|94.04|147.0|16.0|1490959478
7|16:a2:7b:e1:32:df|-72|-29.96|94.05|140.0|8.0|1490953051
8|ad:cc:f8:d1:9c:8c|-83|-29.96|94.06|139.0|12.0|1490961541
9|92:03:c5:34:7a:18|-71|-30.0|94.14|136.0|16.0|1490965714
10|e5:2d:5e:57:f6:60|-90|-29.99|94.14|143.0|6.0|1490961929
11|9b:f5:67:43:f4:bd|-93|-29.98|94.14|138.0|4.0|1490964454
12|f8:c7:f3:ab:bd:4e|-88|-29.97|94.14|134.0|8.0|1490963904
13|23:72:8e:34:89:1b|-90|-29.96|94.14|141.0|6.0|1490956302
14|ed:94:48:2a:5d:5f|-86|-29.95|94.14|140.0|12.0|1490951611
15|bf:19:ef:ec:7f:6b|-91|-30.0|94.16|147.0|6.0|1490963522
16|92:82:da:93:3a:8f|-71|-30.0|94.15|142.0|16.0|1490951546
...
```

```
sqlite3 wigle "SELECT lat, lon, _id FROM location" | tr "|" "," | pbcopy
```

### Flag
```
FLAG{F0UND_M3_33DE7930}
```