# Coffee
## level 3 - reverse engineering - 115 points

### Description
> You found a suspicious USB drive in a jar of pickles. It contains this [file](./data/freeThePickles.class) file.

### Hints
> * Is there a way to get the source of the program?

### Solution
```
$ file freeThePickles.class
freeThePickles.class: compiled Java class data, version 52.0 (Java 1.8)
```

```
$ strings freeThePickles.class
error: /Library/Developer/CommandLineTools/usr/bin/strings: fat file: freeThePickles.class truncated or malformed (offset plus size of cputype (4590080) cpusubtype (7432) extends past the end of the file)
```

strings - => strings search in all file

```
strings - freeThePickles.class
<init>
Code
LineNumberTable
get_flag
()Ljava/lang/String;
StackMapTable
main
([Ljava/lang/String;)V
SourceFile

problem.java

&Hint: Don't worry about the schematics
 eux_Z]\ayiqlog`s^hvnmwr[cpftbkjd
 Zf91XhR7fa=ZVH2H=QlbvdHJx5omN2xc

java/lang/String

Nothing to see here
problem
java/lang/Object
getBytes
()[B
java/lang/System
Ljava/io/PrintStream;
java/util/Base64
getDecoder
Decoder

InnerClasses
()Ljava/util/Base64$Decoder;
java/util/Base64$Decoder
decode
([B)[B
java/util/Arrays
toString
([B)Ljava/lang/String;
java/io/PrintStream
println
(Ljava/lang/String;)V
([B)V
Zd3T
```

[http://www.javadecompilers.com/](http://www.javadecompilers.com/)

```java
import java.util.Base64.Decoder;

public class problem {
  public problem() {}
  
  public static String get_flag() { String str1 = "Hint: Don't worry about the schematics";
    String str2 = "eux_Z]\\ayiqlog`s^hvnmwr[cpftbkjd";
    String str3 = "Zf91XhR7fa=ZVH2H=QlbvdHJx5omN2xc";
    byte[] arrayOfByte1 = str2.getBytes();
    byte[] arrayOfByte2 = str3.getBytes();
    byte[] arrayOfByte3 = new byte[arrayOfByte2.length];
    for (int i = 0; i < arrayOfByte2.length; i++)
    {
      arrayOfByte3[i] = arrayOfByte2[(arrayOfByte1[i] - 90)];
    }
    System.out.println(java.util.Arrays.toString(java.util.Base64.getDecoder().decode(arrayOfByte3)));
    return new String(java.util.Base64.getDecoder().decode(arrayOfByte3));
  }
  
  public static void main(String[] paramArrayOfString) {
    System.out.println("Nothing to see here");
  }
}
```

```python
import base64

STR1 = 'eux_Z]\\ayiqlog`s^hvnmwr[cpftbkjd'
STR2 = 'Zf91XhR7fa=ZVH2H=QlbvdHJx5omN2xc'

out = ''

for i, c in enumerate(STR2):
    out += STR2[ord(STR1[i]) - 90]

print base64.b64decode(out)
```