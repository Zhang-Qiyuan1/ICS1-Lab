# 答案
---
张启元 2024201541

1

|**Value**|**Two's complement**|
|:---:|:---:|
|37| 00100101|
|-15| 11110001|
| 85|01010101|
| -86|10101010|

2

|**Expression**|**Binary Representation**|
|:--:|:--:|
|us|1101 |
|ui|1100 1100|
|us << 1|0110|
|i >> 2|1111 0011|
|ui >> 2|0011 0011|
|(short) i|1100|
|(int) s|1111 1101|

3

```C
#include <stdio.h>
int uadd_ok(unsigned x, unsigned y){
    unsigned sum = x + y;
    return (sum < x || sum < y);
}
```

附：

![picture1](/imgs/hw2.png)