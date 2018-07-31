# Missing Identity
## level 2 - master challenge - 100 points

### Description
> Turns out, some of the files back from Master Challenge 1 were corrupted. Restore this one [file](./data/file) and find the flag. Update 16:26 EST 1 Apr If you feel that you are close, make a private piazza post with what you have, and an admin will help out. The flag starts with the character z.

### Hints
> * What file is this?
> * What do you expect to find in the file structure?
> * All characters in the file are lower case or numberical. There will not be any zeros.

### Solution

We have an unkown file to restore, let's check if `file` tells something useful:

```
$ file file
file: data
```

Just generic data, nothing useful. At this time I took the wrong path trying extracting recursively with `binwalk`, there are a lot of files. After a dozen of tentatives I gave up and run `binwalk` without extracting on `file`:

```
$ binwalk file

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
43            0x2B            PNG image, 627 x 60, 8-bit/color RGB, non-interlaced
84            0x54            Zlib compressed data, default compression
36058         0x8CDA          Zip archive data, at least v2.0 to extract, compressed size: 60762, uncompressed size: 60760, name: nottheflag1.png
96865         0x17A61         Zip archive data, at least v2.0 to extract, compressed size: 54479, uncompressed size: 54459, name: nottheflag2.png
151389        0x24F5D         Zip archive data, at least v2.0 to extract, compressed size: 59917, uncompressed size: 59901, name: nottheflag3.png
211351        0x33997         Zip archive data, at least v2.0 to extract, compressed size: 47730, uncompressed size: 47715, name: nottheflag4.png
259126        0x3F436         Zip archive data, at least v2.0 to extract, compressed size: 58939, uncompressed size: 58919, name: nottheflag5.png
318110        0x4DA9E         Zip archive data, at least v2.0 to extract, compressed size: 49653, uncompressed size: 49689, name: nottheflag6.png
367808        0x59CC0         Zip archive data, at least v2.0 to extract, compressed size: 61868, uncompressed size: 61848, name: nottheflag7.png
430202        0x6907A         End of Zip archive
```

What's the last entry saying? End of Zip archive? Why `file` cannot detect it? Let's try `unzip` it:

```
$ unzip file
Archive:  file
file #1:  bad zipfile offset (local header sig):  0
  inflating: nottheflag1.png
  inflating: nottheflag2.png
  inflating: nottheflag3.png
  inflating: nottheflag4.png
  inflating: nottheflag5.png
  inflating: nottheflag6.png
  inflating: nottheflag7.png
```

Ok, we have an error on the first file, there is some kind of header signature broken. Inspect it with `xxd`:

```
$ xxd file | head
00000000: 5858 5858 5858 0000 0800 2344 7f4a 6a58  XXXXXX....#D.JjX
00000010: bd98 b48c 0000 a58c 0000 0800 0000 666c  ..............fl
00000020: 6167 2e70 6e67 004e 40b1 bf89 504e 470d  ag.png.N@...PNG.
00000030: 0a1a 0a00 0000 0d49 4844 5200 0002 7300  .......IHDR...s.
00000040: 0000 3c08 0200 0000 8243 abc9 0000 8c6c  ..<......C.....l
00000050: 4944 4154 789c dcfd 5793 ad5b 771e 868d  IDATx...W..[w...
00000060: 31d3 1b56 eab8 e3d9 277e 1120 1820 4022  1..V....'~. . @"
00000070: 4c52 8698 4452 b264 89a6 252b dcb8 5c2e  LR..DR.d..%+..\.
00000080: 5fb9 6cdf e80f f846 572e 5fd8 55ae 7239  _.l....FW._.U.r9
00000090: 48b6 cb14 254b b224 a254 2241 db22 0951  H...%K.$.T"A.".Q
```

Ok, the `XXXXXX` bytes are not supposed to be there. We have to replace them with the correct [file signature](https://en.wikipedia.org/wiki/List_of_file_signatures), in our case `50 4b 03 04`. To do that we can use an hex editor or perform the stunt and use `dd`:

```
$ printf '\x50\x4b\x03\x04' | dd of=file bs=1 conv=notrunc
4+0 records in
4+0 records out
4 bytes transferred in 0.000363 secs (11016 bytes/sec)
$ file file
file: Zip archive data
$ binwalk file

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Zip archive data, compressed size: 36020, uncompressed size: 36005, name: flag.png
36058         0x8CDA          Zip archive data, at least v2.0 to extract, compressed size: 60762, uncompressed size: 60760, name: nottheflag1.png
96865         0x17A61         Zip archive data, at least v2.0 to extract, compressed size: 54479, uncompressed size: 54459, name: nottheflag2.png
151389        0x24F5D         Zip archive data, at least v2.0 to extract, compressed size: 59917, uncompressed size: 59901, name: nottheflag3.png
211351        0x33997         Zip archive data, at least v2.0 to extract, compressed size: 47730, uncompressed size: 47715, name: nottheflag4.png
259126        0x3F436         Zip archive data, at least v2.0 to extract, compressed size: 58939, uncompressed size: 58919, name: nottheflag5.png
318110        0x4DA9E         Zip archive data, at least v2.0 to extract, compressed size: 49653, uncompressed size: 49689, name: nottheflag6.png
367808        0x59CC0         Zip archive data, at least v2.0 to extract, compressed size: 61868, uncompressed size: 61848, name: nottheflag7.png
430202        0x6907A         End of Zip archive
```

Bingo! Now we have the zip file uncorrupted, `unzip` it and read the flag from the picture.

### Flag
```
zippidydooda49688958
````
