---
title: CS110 Homework [2]
mathjax: false
date: 2024-03-26 11:21:15
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 作业
---

***HW 1 on Gradescope!***

  
Fetch the homework [here](https://classroom.github.com/a/pwC6ijm6) on Github classroom, you will use Github for version control and Gradescope for submission.


<!--more-->

# Homework 2 of CS110 2024 Spring

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) @ [ShanghaiTech University](https://www.shanghaitech.edu.cn/)  

## Introduction

Please write a program that takes 32-bit binary strings as input, simulates the addition operation of IEEE 754 single-precision floating-point numbers, and outputs the binary representation of the sum.  
Before you begin, make sure that you understand the following concepts:

1. Sign bit (1 bit), exponent bits (8 bits), and mantissa bits (23 bits).
2. The IEEE 754 standard stores a biased exponent, the bias of normalized numbers is different from that of denormalized numbers.
3. There are several possible representations of zero, infinity, and NaN.

As stated in the lecture slides, the steps of the addition operation are as follows:

1. Align the exponents of the two floating-point numbers to make them equal.
2. Add the two mantissa to get the result mantissa.
3. Normalize the result mantissa to ensure that the number of mantissa bits meets the standard requirements.
4. If the result mantissa overflows, perform rounding and adjust the exponent.

If you have trouble understanding the concepts, you may try [this](http://weitz.de/ieee/) tiny calculator.  
Note that this calculator uses **round to nearest, ties to even** as the rounding method, which is different from the rounding method we use in this assignment.

## Specification

### Input

We guarantee that the input bitstring shall never be NaN, infinity, or zero.

### Precision

Before calculating the mantissa, we add three extra bits to preserve the precision of the result. The three extra bits are the guard bit, the round bit, and the sticky bit.  
Here is a brief explanation of these bits:

    Mantissa of the number (assume normalized):
     1.XXXXXXXXXXXXXXXXXXXXXXX   0   0   0

     ^         ^                 ^   ^   ^
     |         |                 |   |   |
     |         |                 |   |   -  sticky bit (s)
     |         |                 |   -  round bit (r)
     |         |                 -  guard bit (g)
     |         -  23-bit mantissa from a representation
     -  hidden bit

The guard bit and the round bit are the two bits immediately to the right of the least significant bit of the mantissa. They simply give two extra bits of precision. The sticky bit is an indication of whether there are any non-zero bits to the right of the round bit.

                                              g r s
    Initial:        1.11000000000000000010100 0 0 0
    Shift 1 bit:    0.11100000000000000001010 0 0 0
    Shift 2 bits:   0.01110000000000000000101 0 0 0
    Shift 3 bits:   0.00111000000000000000010 1 0 0
    Shift 4 bits:   0.00011100000000000000001 0 1 0
    Shift 5 bits:   0.00001110000000000000000 1 0 1
    Shift 6 bits:   0.00000111000000000000000 0 1 1
    Shift 7 bits:   0.00000011100000000000000 0 0 1
    Shift 8 bits:   0.00000001110000000000000 0 0 1

After the mantissa calculation and the normalization, the result mantissa should be rounded to 23 bits.

### Rounding

In this assignment, we use truncation as the rounding method.  
Truncation, so-called "round-towards-zero", is a method of rounding towards the nearest integer by dropping the fractional part of the number.

### Submission

Upload `FloatCalculate.c` and `FloatCalculate.h` to Gradescope and wait for the result.

### Notice

We have provided a `test.c` for you to generate new testcases and find the correct answer. You can use it to test your program.  
You might notice that a possible trick for bypassing the calculation is to convert the input bitstring to a (builtin) `float` and then use the `float` to do the rest of the calculation.  
However, this is **NOT ALLOWED** in this assignment. We will check the validity of your program both automatically and manually.  
On the autograder, we had banned those function calls that would change the rounding mode, like `fesetround(FE_TOWARDZERO)`. We have also banned inline assembly `asm`, `__asm`, and `__asm__` in the autograder.  
In the template, we have provided `CMakeLists.txt` and `Makefile` to help you compile the program. You can choose either of them. They will not be submitted to Gradescope anyway.

## Code quality

- Your code with be compiled with `-Wpedantic -Wall -Wextra -Wvla -Werror -std=c11`
- No memory leak is allowed, `Valgrind` will be used to detect memory leaks on runtime.

---

The following TA(s) are responsible for this homework:

Suting Chen <chenst AT shanghaitech.edu.cn>  
Li Zhu <zhuli2023 AT shanghaitech.edu.cn>  
Letong Han <hanlt AT shanghaitech.edu.cn>  

  

The description of three extra bits is adapted from "Floating Point Representation" by Karen Miller, 2006.