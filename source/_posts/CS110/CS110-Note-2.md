---
title: CS110 Note [2]
mathjax: true
description: Arithmetic for Computers
date: 2024-02-23 09:37:52
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 笔记
---


# Using Bits to represent everything

$N$ bits $\leftrightarrow$ $2^N$ different values, which could be used to abstractly represent a set of $2^N$ different things in real world. 


# Hex, Decimal and Binary

## Hexadecimal

Hexadecimal, or "hex" for short, is a way of counting that uses 16 symbols instead of the usual 10 symbols (0-9) we use in everyday life. In hex, we use the numbers 0-9 and the letters A-F to represent values from 0 to 15.

Here's a quick comparison of decimal (base-10) and hexadecimal (base-16) counting:

|         |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
| ------- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Decimal |   0   |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |  10   |  11   |  12   |  13   |  14   |  15   |  16   |  17   |
| Hex     |   0   |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |   A   |   B   |   C   |   D   |   E   |   F   |  10   |  11   |


## Binary

Binary is also a way of counting, but it only uses two symbols: 0 and 1. This is because computers are made up of tiny switches that can only be in one of two states: on or off. We can use these switches to represent the binary numbers 0 and 1.

Here's a quick comparison of decimal (base-10) and binary (base-2) counting:

|         |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
| ------- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Decimal |   0   |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |  10   |  11   |  12   |  13   |  14   |  15   |  16   |
| Binary  |   0   |   1   |  10   |  11   |  100  |  101  |  110  |  111  | 1000  | 1001  | 1010  | 1011  | 1100  | 1101  | 1110  | 1111  | 10000 |


## Converting between Hex, Decimal and Binary

### **Decimal to Hex and Binary**

Converting hex and binary to decimal is easy. You just need to use the order of the digits and multiply each digit by the base of the system raised to the power of its position. For example, the hex number 2A3F can be converted to decimal like this:

$2A3F_{16} = 2 \times 16^3 + 10 \times 16^2 + 3 \times 16^1 + 15 \times 16^0 = 10783_{10}$

### **Hex and Binary to Decimal**

Converting decimal to hex and binary needs a little more complicated algorithm. 

```python
def decimal_to_base(base):
    def convert(n):
        if n < base:
            return str(n)
        else:
            return convert(n // base) + str(n % base)
    return convert

decimal_to_hex = decimal_to_base(16)
decimal_to_binary = decimal_to_base(2)
```

### **Hex and Binary Conversion**

Converting between hex and binary is quite easy. Why? 

By observing the table below, we can see that each hex digit corresponds to a 4-bit binary number:

|        |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |       |
| ------ | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Hex    |   0   |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |   A   |   B   |   C   |   D   |   E   |   F   |
| Binary |   0   |   1   |  10   |  11   |  100  |  101  |  110  |  111  | 1000  | 1001  | 1010  | 1011  | 1100  | 1101  | 1110  | 1111  |

So to convert a hex number to binary, we just need to convert each hex digit to its corresponding 4-bit binary number. For example, the hex number 2A3F can be converted to binary like this:

$2A3F_{16} = 0010 1010 0011 1111_{2}$

Since $2$ corresponds to $0010$, $A$ corresponds to $1010$, $3$ corresponds to $0011$ and $F$ corresponds to $1111$.

# Number Representation in Computer

## Integer Representation

Under the hood, the computer represents the an integer using bits, with binary. Since the convinience of converting between hex and binary, we usually use hex to represent the bits when we acutally view a binary number because the binary representaion is quite too long. 

If the integer is unsigned, we can use all the bits to represent the numeral. The direct conversion from binary to decimal is the correspongding number. When we need to represent a signed integer, things are getting more complex.

Typically, there are three ways to represent the sign of a integer in computer:
1. **Sign-Magnitude**: The leftmost bit is used to represent the sign of the integer. 0 for positive and 1 for negative.
    >For example, the 8-bit sign-magnitude representation of the integer 5 is $00000101$, and the representation of -5 is $10000101$.
2. **One's Complement**: The leftmost bit is used to represent the sign of the integer. 0 for positive and 1 for negative. The negative integer is represented by flipping all the bits of the positive integer.
    >For example, the 8-bit one's complement representation of the integer 5 is $00000101$, and the representation of -5 is $11111010$.
3. **Two's Complement**: The leftmost bit is used to represent the sign of the integer. 0 for positive and 1 for negative. The negative integer is represented by flipping all the bits of the positive integer and then adding 1 to the result.
    >For example, the 8-bit two's complement representation of the integer 5 is $00000101$, and the representation of -5 is $11111010 + 1 = 11111011$.


There are obvious shortcomings of first two methods.

For **sign-magnitude**:
- There are two representations for zero: $00000000$ and $10000000$. This makes the arithmetic operations more complicated. 
- When we add number, we need to check the sign bit and then perform the operation. Otherwise, this will happen:
    > $00000101(5) + 10000101(-5) = 10001010(-10)$.

For **one's complement**:
- Still, there are two representations for zero: $00000000$ and $11111111$.

So, in the end, the *two's complement* is the most widely used method to represent the sign of a integer in computer.

## Integer Extension

Inside the computer, there's chance that we need to extend the bits of an integer. Such as convert a 8-bit integer to a 16-bit integer. There are two ways to do this:
1. **Zero Extension**: The leftmost bit is used to represent the sign of the integer. 0 for positive and 1 for negative. The new bits are filled with 0.

1. **Sign Extension**: The leftmost bit is used to represent the sign of the integer. 0 for positive and 1 for negative. The leftmost bit is copied to the new bits.
    >For example, the 8-bit sign-magnitude representation of the integer -5 is $10000101$, and the 16-bit sign-magnitude representation of -5 is $11111111 10000101$.

Obviously, if we choose to use *two's complement* to represent the sign of a integer, the sign extension will be the only way to extend the bits of an integer. 


## Floating Point Representation

*Floating point* is used to represent the real number, compared to integer, real number sometime have the point. 

The term floating point is more like a relative name come from the idea of *fixed point*, which is used to represent real number too.

### Fixed Point

Like we represent sign and the numeral of a integer by using different specific bits, we can also represent the sign, the integer part and the fractional part of a real number by using different specific bits.

For example, we can use the first bit to represent the sign of the real number, the next 4 bits to represent the integer part and the last 3 bits to represent the fractional part.
> $10.1010_{2} = 1 \times 2^1 + 1 \times 2^{-1}  + 1 \times 2^{-3}  = 2.625_{10}$

We can see that the number of bits used to represent the fractional part is fixed, which is determined by how many bits we use to represent the fractional part. This is why it is called *fixed point*.

### Basic Floating Point

To understand the idea of *floating point*, we need to understand the idea of *scientific notation*.

In decimal scientific notation, a number is represented as a product of a mantissa and a power of 10. 
> For example, the number 1234 can be represented as $1.234 \times 10^3$.

In binary scientific notation, a number is represented as a product of a mantissa and a power of 2.
> For example, the number 10 can be represented as $1.01_2 \times 2_{10}^3$.

The idea of *floating point* is similar to the idea of *scientific notation*. We can represent a real number as a product of a mantissa and a power of 2.

The normalized format of scientific notation in binary is: 
$$1.x_{bin} \times 2^{y_{bin}}$$

Only $1$ on the left side of the point is needed for normalization. Just like in decimal scientific notation, we can represent the number $0.001$ as $1.0 \times 2^{-3}$ to make sure on the left side of the point there is only one nonzero digit.


For a 32 bits floating point, the structure of the representation is:
| sign  | exponent ($y$) | significand ($x$) |
| ----- | -------------- | ----------------- |
| 1 bit | 8 bits         | 23 bits           |

$$(-1)^\text{sign} \times x_{bin} \times 2^{y_{bin}}$$

For this naive implementaion of floating point, the range of representation is:
$$
[-3.4 \times 10^{38},\;-1.2 \times 10^{-38}] \cup [1.2 \times 10^{-38},\;3.4 \times 10^{38}]
$$
which does not include the vital $0$.

### IEEE 754 Floating Point Standard

Compared to the naive implementaion of floating point, the IEEE 754 Floating Point Standard majorly differs by using bias to represent the exponent part and implict leading $1$ in significand part.

For a 32 bits floating point, the structure of the representation is:
| sign  | exponent ($y$) | significand ($x$) |
| ----- | -------------- | ----------------- |
| 1 bit | 8 bits         | 23 bits           |

$$(-1)^\text{sign} \times (1.x_{bin}) \times 2^{y_{bin} - 127}$$

The range of representation is:
$$
[-3.4 \times 10^{38},\;-1.2 \times 10^{-38}] \cup [1.2 \times 10^{-38},\;3.4 \times 10^{38}]
$$
which does still not include the vital $0$.

The bias helps using unsigned integer to represent the exponent part, which makes the comparison between two floating point numbers easier and simplifies the hardware design.

The implict leading $1$ makes the numeral part from 23 bits to 24 bits, which makes the representation more accurate. We can make the leading $1$ implict is because **any normlized floating point number's significand has a leading $1$**. So if we always maintain the floating point number in normalized form, we assume the leading $1$ is always there.

Also to notice that after hiding the leading $1$, the left part of the significand is usually called *mantissa*.

For a 64 bits floating point, the structure is similar:
| sign  | exponent ($y$) | significand ($x$) |
| ----- | -------------- | ----------------- |
| 1 bit | 11 bits        | 52 bits           |

$$(-1)^\text{sign} \times (1.x_{bin}) \times 2^{y_{bin} - 1023}$$

The range of representation is:
$$
[-1.8 \times 10^{308},\;-2.2 \times 10^{-308}] \cup [2.2 \times 10^{-308},\;1.8 \times 10^{308}]
$$

Notice now our floating point only use normalized representation, but infact if we accept the denormalized representation, we can represent even smaller numbers.

The smallest normalized floating points of 32bits is:
$$
1.0000\ 0000\ 0000\ 0000\ 0000\ 000_{bin} \times 2^{-126} \approx 1.4 \times 10^{-45}
$$

But the smallest denormalized floating points of 32bits is:
$$
0.0000\ 0000\ 0000\ 0000\ 0000\ 001_{bin} \times 2^{-126} = 1.0_{bin} \times 2^{-149} \approx 1.4 \times 10^{-45}
$$

Thus in IEEE 754, **When the exponent part is all $0$, the number is a denormalized number**. The significand part is interpreted as $0.x_{bin}$ (No leading $1$), and the exponent part is interpreted as $-126$ for a 32 bits floating point and $-1022$ for a 64 bits floating point. So the smallest positive number we can represent is $2^{-126}$ (about $1.4 × 10^{-45}$) for a 32 bits floating point and $2^{-1022}$ (about $5.0 × 10^{-324}$) for a 64 bits floating point.

In the following Chapters, we'll use IEEE 754 Floating Point Standard.


### Special Values

In the IEEE 754 Floating Point Standard, there are some special values to represent some special real numbers.

Bellow is the table of the special values for a 32 bits floating point:
| Single precision |          | Double precision |          |   Object represented    |
| :--------------: | :------: | :--------------: | :------: | :---------------------: |
|     Exponent     | Fraction |     Exponent     | Fraction |                         |
|        0         |    0     |        0         |    0     |            0            |
|        0         | Nonzero  |        0         | Nonzero  |  ± denormalized number  |
|      1–254       | Anything |      1–2046      | Anything | ± floating-point number |
|       255        |    0     |       2047       |    0     |       ± infinity        |
|       255        | Nonzero  |       2047       | Nonzero  |   NaN (Not a Number)    |


# Arithmetic in Computers

## Integer Arithmetic

### Everything is Addition

Thanks to the "strange" way we represent the integers we learn above, for both addition and substraction, so long as we consider them as two signed integers' addtion, we just need to add their binary representation directly, then we get the answer.

For example, let's consider `3 - 4`, which should be condsidered as `3 + -4`:
```
    00000000 00000000 00000000 00000110 (3)
+   11111111 11111111 11111111 11111001 (-4) 
----------------------------------------
    11111111 11111111 11111111 11111111 (-1)
```

If you're unfamiliar with binary addition and can not understand the above arithmetic, go check [this video](https://www.bilibili.com/video/BV1EW411u7th?p=4&vd_source=fdfd8451279302b7750ea2a395a2fe38).

As for other types of addtion and subtraction, they are the same.

### Overflow and Underflow

What will happen if we do calculation like this?
```
    01111111 11111111 11111111 11111111 (2^31 - 1)
+   00000000 00000000 00000000 00000001 (1)
----------------------------------------
```

By arithmetic in binary, the result should be:
```
    10000000 00000000 00000000 00000000 (-2^31)
```

Notice the intepretation of the result is not $2^31 - 1 = 2^31$, but $-2^31$. This is called *overflow*. And it's due to the limitation of the bits'amount we use to represent the integer. **We try to represent a number that is larger than the largest number we can represent.**

Correspondingly, if we do calculation like this:
```
    10000000 00000000 00000000 00000000 (-2^31)
-   00000000 00000000 00000000 00000001 (1)
----------------------------------------
```

The result should be:
```
    01111111 11111111 11111111 11111111 (2^31 - 1)
```

This is called *underflow*. The reason is the same as overflow. **We try to represent a number that is smaller than the smallest number we can represent.**


### Naive Multiplication

Again, Let's recall how we make multiplication by hand in decimal:
```
      123
    x 456
    -----
      738
     615
    492
  -------
    56088
```

The multiplication in binary is the same as in decimal. Just use binary instead of decimal.
```
       1101 (13)
     x 1011 (11)
     -------
       1101
      1101
     0000
    1101  
  ----------
   10001111 (143)
```


Did you notice the pattern? Since we only have $1$ nad $0$ in binary, the multiplication is just the summation of multiple shifted version of the multiplicand or zeros, and the shift is determined by the multiplier's bits.


Thus we could come up with a algorithm to do the multiplication in binary:
```python
def bin_mul(multiplicand, multiplier):
    # Result is 128-bit
    result = 0 
    
    for i in range(64):
        if multiplier[-1] == 1:
            result += multiplicand
        # Multiplicand is 128-bit in the end
        multiplicand <<= 1 
        # Multiplier is 64-bit initially and 0-bit in the end
        multiplier >>= 1 
    
    return result
```

### Refined Multiplication

The multiplication above is effective, but **only works for positive integers**. And it actually **prodeces many 128-bit intermediate results**. The variable to store result is 128-bit, the multiplicand should have 128-bit variable to store too. And the hardware to perform the addition is also should be 128-bit.

By refining the algorithm, we can just use single 129-bit variable, and other variables are all 64-bit. Plus, fix the sign problem.

```python
def bin_mul(multiplicand, multiplier):
    # Result is 129-bit, right half is the multiplier
    result = [zeros(65), multiplier] 

    for i in range(64):
        if result[-1][-1] == 1:
            # Multiplicand is 64-bit, result[0] for overflow bit
            result[0:64] += multiplicand 
        result =>> 1 
```

### Naive Division
The reciprocal operation of multiply is divide, an operation that is even less frequent
and even quirkier. It even offers the opportunity to perform a mathematically
invalid operation: dividing by $0$.

Divide's two operands, called the *dividend* and *divisor*, and the result, called
the *quotient*, are accompanied by a second result, called the *remainder*. Here is
another way to express the relationship between the components:
$$
    Dividend = Quotient \times Divisor + Remainder
$$

An example division by hand in decimal:
```
     123
   ------
 5 | 616
    -5
 --------
     11
    -10
 --------
      16
     -15
 --------
       1
```
which means $616 \div 5 = 123 \cdots 1$.

Then let's see how we do the division in binary:
```
               1001 (9)
          ----------
 1000 (8) | 1001010 (74)
           -1000
 -------------------
               10
               -0
 -------------------
               101
                -0
 -------------------
               1010
              -1000
 -------------------
                 10 (2)
```

The pattern is similar to the multiplication. The division is just the subtraction of multiple shifted version of the divisor or zeros, and the shift is determined by the dividend's bits.

Therefore, an algorithm to do the division in binary is:
```python
def bin_div(dividend, divisor):
    quotient = 0 # 64-bit
    remainder = [zeros(64), dividend] # 128-bit 
    divisor = [divisor, zeros(64)] # 128-bit
    
    for i in range(64):
        if remainder >= divisor:
            remainder -= divisor
            quotient <<= 1
            quotient += 1
        else:
            quotient <<= 1
            quotient += 0

        divisor >>= 1
    
    return quotient, remainder
```

### Refined Division

Like the naive multiplication, the naive division is also effective, but **only works for positive integers**. And we can just use single 129-bit variable to store the result, and other variables are all 64-bit. Plus, fix the sign problem.

```python
def bin_div(dividend, divisor):
    remainder = [zeros(65), abs(dividend)] # 129-bit
    
    for i in range(64):
        if remainder[0:64] >= abs(divisor):
            remainder[0:64] -= abs(divisor)
            remainder <<= 1
            remainder += 1
        else:
            remainder <<= 1
            remainder += 0
    
    quotient = remainder[0:64]
    remainder = remainder[65:128]

    if sign(dividend) != sign(divisor):
        quotient = -quotient

    if sign(dividend) != sign(remainder):
        remainder = -remainder

    return quotient, remainder
```



## Floating Point Arithmetic

### Arithmetic in Practical Life

Recall how we do the arithmetic of numbers represented by scitific notation:

1. Assumming we are adding $1.23 \times 10^3$ and $4.56 \times 10^2$:

2. First we will "align" the two number, letting them using the same power of $10$
    >$$
    >    \begin{align*}
    >        1.230 \times 10^3 = 1.230 \times 10^3& \\
    >        4.560 \times 10^2 = 0.456 \times 10^3&
    >    \end{align*}
    >$$

3. Then we can add the two number's significand directly:
    >$$
    >    \begin{align*}
    >         1.230 \times 10^3& \\
    >        +\ 0.456 \times 10^3& \\
    >        \hline
    >        1.686 \times 10^3& 
    >    \end{align*}
    >$$

4. Then we check if the result is normalized, if not, we normalize it, but in this case, the result is already normalized.

### Arithmetic in Binary 

In fact the arithmetic of floating point numbers in computer is exactly the same as the arithmetic of floating point numbers in practical life. Just use binary instead of decimal.

For example, let's consider $1.01_2 \times 2^3$ and $1.11_2 \times 2^2$:

1. Align the two numbers (Notcie the exponent part):
    >$$
    >    \begin{align*}
    >        1.010 \times 2^3 = 1.010 \times 2^3& \\
    >        1.110 \times 2^2 = 0.111 \times 2^3&
    >    \end{align*}
    >$$

2. Add the two numbers' significand:
    >$$
    >    \begin{align*}
    >         1.010 \times 2^3& \\
    >        +\ 0.111 \times 2^3& \\
    >        \hline
    >        10.001 \times 2^3& 
    >    \end{align*}
    >$$


3. Normalize the result:
    >$$
    >    \begin{align*}
    >        10.001 \times 2^3 = 1.0001 \times 2^4&
    >    \end{align*}
    >$$


### Arithmetic in IEEE 754 Floating Point Standard

Now we know how to do the arithmetic of floating point numbers in computer, but there are still some details we need to pay attention tom when we do the arithmetic in IEEE 754 Floating Point Standard.

This time, we still consider $1.01_2 \times 2^3$ and $1.11_2 \times 2^2$:

1. Represent the two numbers in IEEE 754 Floating Point Standard:
    ```
        0 10000010 01000000000000000000000 (1.01 * 2^3)
    +   0 10000001 11000000000000000000000 (1.11 * 2^2)
    ```


1. Add the implicit leading $1$ to the significand part.
    ```
        0 10000010 (1)01000000000000000000000 (1.01 * 2^3)
    +   0 10000001 (1)11000000000000000000000 (1.11 * 2^2)
    ```

    Notcie now that the mantiassa part become the significand part. From 23-bits to 24-bits.


1. Align the two numbers (Notcie the exponent part):
    ```
        0 10000010 (1)01000000000000000000000 (1.010 * 2^3)
    +   0 10000010 (0)11100000000000000000000 (0.111 * 2^3)
    ```

1. Add the two numbers' significand:
    ```
        0 10000010 (1)01000000000000000000000 (1.010 * 2^3)
    +   0 10000010 (0)11100000000000000000000 (0.111 * 2^3)
    --------------------------------------
        0 10000010 (10)0010000000000000000000 (10.001 * 2^3)
    ```

    Notice that the significand part is overflowed. We actuall dropped a `0` in the end of the significand.  

1. Normalize the result:
    ```
        0 10000011 (1)00010000000000000000000 (1.0001 * 2^4)
    ```

    Notice that the exponent part is increased by $1$.

1. The final representation:
    ```
        0 10000011 00010000000000000000000 (1.0001 * 2^4)
    ```

    After making the leading $1$ implict again, we get the actual bits in the hareware to represent floating point in IEEE 754.


### Acurate Arithmetic in IEEE 754

In the above example, we can see that there's many chance that we'll drop some bits in the significand part. when the droped bits are non zero, the result will be distorted.

For example, let's consider we are normalizing such a number:
```
    0 10000010 (10)1111111111111111111111 (10.1111111111111111111111 * 2^3)
```

After normalization, the result should be:
```
    0 10000011 (1)01111111111111111111111 (1.01111111111111111111111 * 2^4)
```

And the normalized result is smaller than the actual result by $0.00000000000000000000001 \times 2^3$ (about $8 \times 10^{-23}$) since we dropped the last bit of the unnormalized significand.


To reduce the loss caused by the drop of bits, we typically use the following method to do the arithmetic in IEEE 754:

- Using 2 buffer bits during the calculation to extend the significand part temporarily.
    - The first buffer bit is called *guard bit*.
    - The second buffer bit is called *round bit*.

- Using a extra bits to record if there's any bit storing $1$ was dropped.
    - This bit is called *sticky bit*.

Here's a demonstration of the method:

1. Before calculating the mantissa, we add three extra bits to preserve the precision of the result. The three extra bits are the guard bit, the round bit, and the sticky bit.  

    Here is a brief explanation of these bits:

    ```
    Mantissa of the number (assume normalized):
        1.XXXXXXXXXXXXXXXXXXXXXXX   0   0   0

        ^         ^                 ^   ^   ^
        |         |                 |   |   |
        |         |                 |   |   -  sticky bit (s)
        |         |                 |   -  round bit (r)
        |         |                 -  guard bit (g)
        |         -  23-bit mantissa from a representation
        -  hidden bit
    ```

2. The sticky bit is an indication of whether there are any non-zero bits to the right of the round bit.
    ```
                                             gr s
    Initial:        1.1100000000000000001010000 0
    Shift 1 bit:    0.1110000000000000000101000 0
    Shift 2 bits:   0.0111000000000000000010100 0
    Shift 3 bits:   0.0011100000000000000001010 0
    Shift 4 bits:   0.0001110000000000000000101 0
    Shift 5 bits:   0.0000111000000000000000010 1
    Shift 6 bits:   0.0000011100000000000000001 1
    Shift 7 bits:   0.0000001110000000000000000 1
    Shift 8 bits:   0.0000000111000000000000000 1
    ```
