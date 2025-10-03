## 答案
---
张启元 2024201541

1

|                      | little-endian | big-endian |
| -------------------- | ------------- | ---------- |
| show_bytes(valp, 1); |   33            |     14       |
| show_bytes(valp, 2); |       3302        |       140A     |
| show_bytes(valp, 4); |       33020A14        |       140A0233     |

2

| Fractional value | Binary representation | Decimal representation |
|------------------|-----------------------|------------------------|
| 1/8              |     0.001       |          0.125      |
| 3/4              |     0.11      |     0.75        |
| 43/16                 | 10.1011        |   2.6875             |
| 25/16            |    1.1001         |   1.5625            |
|   51/16       |       11.0011            | 3.1875                 |

3

A. E = 2, M = 1.01, f = 1, V = (-1)^0 \* 1.01 \* 2^2,
B. E = n, M = ((1 << n) - 1) \* 2 ^ (-n + 1), f = (1 << (n - 1)) - 1, V = (1 << n) - 1,
C. E = 2 ^ (K - 1) - 2, M = 1,  f = 0, V = 2 ^ (2 ^ (K - 1) - 2).

4

| Format A Bits | Format A Value | Format B Bits | Format B Value |
|---------------|----------------|---------------|----------------|
| 1 01110 001   |-9/16 | 1 0110 0010   | -9/16 |
| 0 10110 101   |208  | 0 1110 1010 |  208  |
| 1 00111 110   | -7/1024 | 1 0000 0111 | -7/1024    |
| 0 00000 101   | 5/131072    |  0 0000 0001 |  1/1024  |
| 1 11011 000   | -4096 |  1 1110 1111   | -248   |
| 0 11000 100   |   768  | 0 1111 0000    | 无穷大 |

5

A. 恒为1。因为int转换为float时，当x足够大时，float的23位fraction无法完全表示int的有效位，因此会丢失精度；而x转换为dx时不会丢失精度，但同理，dx转换为(float)dx时也会丢失精度，二者丢失的精度是相同的（都是由于fraction位数丢失的），所以二者是完全相同的。
B. 不恒为1。当x - y溢出int的范围时，dx - dy不会溢出double的范围，这时二者不相同。比如，x = T_min, y = T_max, x - y = 100...00 = -1，dx = 0 10000011101 11....1100....00，dy = 1 10000011101 11....1100....00, dx - dy = 0 10000011110 111....1000..0 > 0,故二者不相等。
C. 恒为1。由于dx, dy, dz都由int类型转化而来，而3 \* x仍然在double的表示范围之内，故不会出现溢出情况，也就不会产生误差。
D. 不恒为1。由于double的fraction只有52位，所以当dx \* dy溢出时，会导致左右不相等。比如，dx = 2 ^ 26 + 1， dy = 2 ^ 27 + 1, dz = 3, (dx \* dy) \* dz = 2 ^54 + 2 ^ 53 + 2 ^ 29 + 2 ^ 26, dx \* (dy \* dz) = 2 ^54 + 2 ^ 53 + 2 ^ 29 + 2 ^ 26 + 3, 在舍入规则下舍入为2 ^54 + 2 ^ 53 + 2 ^ 29 + 2 ^ 26 + 4，二者不相等。
E. 不恒为1。当x == 0时除法不成立，故不恒为1。

6

```c
typedef unsigned float_bits;
/* Compute |f|. If f is NaN, then return f. */
float_bits float_absval (float_bits f){
    unsigned sign = f & 0x80000000;
    unsigned exp = (f >> 23) & 0xFF;
    unsigned frac = f & 0x7FFFFF;

    if (exp == 0xFF && frac != 0){
        return f;
    }
    return f & 0x7FFFFFF;
}
```

7

```c
/* Compute 2*f. If f is NaN, return f. */
float_bits float_twice(float_bits f){
    unsigned sign = f & 0x80000000;
    unsigned exp = (f >> 23) & 0xFF;
    unsigned frac = f & 0x7FFFFF;

    if (exp == 0xFF) return f;
    if (exp == 0) {
        if (frac & 0x800000) {
            exp = 1;
            frac <<= 1;
        }else frac <<= 1;
    }
    else {
        exp += 1;
        if (exp == 0xFF) frac = 0;
    }
    return sign | (exp << 23) | frac;
}
```