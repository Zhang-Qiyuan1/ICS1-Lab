# 答案

张启元 20204201541

1

| Binary | Octal | Decimal | Hexadecimal |
|:--------:|:-------:|:---------:|:-------------:|
|101 0101 0110| 2526| 1366| 556|
| 1 1111 1111| 777| 511| 0x1ff|
|1 1010 0101| 645| 421|0x1c5|
| 111 1101 1111|3737 |2015|0x7df |
|100 0000 1101 | 2015 | 1037| 0x40d|

2

a) A&B = 0x3A

b) A|B = 0xFF

c) A^B = 0xc5

d) ~A|~B = 0xc5

e) A&&B = 1

f) A||B = 1

3

```C
#include <stdio.h>
int func(int x){
    int y = 0;
    int z = 0xFF;
    if (!(x ^ ~y) || !(x ^ y) || (x & z) || (x ^ ~z)){
        return 1;
    }
    return 0;
}
```

4

```C
#include <stdio.h>
int func(int x, int y){
    int z = 0xFFFF
    int a = x & z;
    int b = y & z;
    b = y - b;
    return a + b;
}
```

附：
![picture1](/home/zhangqiyuan/ICS1-Lab/hw1.jpg)

