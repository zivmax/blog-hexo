---
title: CS110 Lab [1]
mathjax: false
date: 2024-03-26 11:13:11
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 作业
---

**Goals**

- Setup your Linux.
- Setup Gradescope.
- Learn some simple C programming.
- Learn basic linux commands.

<!--more-->

# Lab 1

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) @ [ShanghaiTech University](https://www.shanghaitech.edu.cn/)  


## How Checkoffs Work

You'll notice that at the end of (almost) every exercise, there is a section labelled "Check-off." The items in this section are what you must successfully demonstrate to your TA in order to receive credit for completing the lab. Once you finish **ALL of the exercises** , you should put your names AND ShanghaiTech email address on the checkoff list on the board, and a TA will come and check you off. Labs are graded out of x points, which will evaluate to 100%. A lab is considered on-time if it is turned in within a week of the lab session in which it is initially assigned to you. For example, the lab assigned to you in this weeks lab is this document, lab 1. Thus, the latest you may get lab 1 checked off is the beginning of your lab next week.

## Exercises

### Exercise 1: Have a 64bit Linux installed

As CS students you should have a **REAL** Linux (e.g. Ubuntu) installed. In CS110 and CS110P, we recommend you installing Ubuntu 20.04 LTS (64bit) on your computer. If you are using an X86 computer (Intel or AMD), you should install it as dual boot. If you are using an ARM-based computer (e.g. Apple Silicon), contact us immediately. Note that some part of the lab may not work on macOS!

Show your TA the working Linux with gcc installed.

### Exercise 2: Gradescope

Show the TA your gradescope account and that you finished HW1.

### Exercise 3: Linux command

The following commands/programs are very useful in Linux.

- cd
- ls
- pwd
- mkdir
- rm
- cp
- mv
- cat
- man

Explain the usage of each command to the TA.

### Exercise 4: Linux command, cont.

In this section, learn a new command/program by reading the manual with command `man`:

- grep

Unlike other operating systems, Linux users love package managers for installing and updating software. In Ubuntu, the package manager is called apt.

- Install an interesting package with apt, if you have no idea which to choose, try `cowsay`.
- Show the manual of `grep` and explain the usage to the TA.

At last, we understand that reading a large bunch of plain text in the terminal is not a good experience. However, this is necessary for programmers. Here we recommend you installing `tldr` to make your life easier.

### Exercise 5: Sizes

Write a C program that displays the sizes (in byte) of different data types. You are only allowed to have one `printf` in the program - it has to be used inside a precompiler macro. This macro can only have one argument. Each type should be output in a new line similar to this (with correct values of course):  
`Size of short: 3   Size of int: 5   `The sizes of the following types should be printed:  

- char
- short
- short int
- int
- long int
- unsigned int
- void *
- size_t
- float
- double
- int8_t
- int16_t
- int32_t
- int64_t
- time_t
- clock_t
- struct tm
- NULL

  
Compile using:  
`gcc -Wpedantic -Wall -Wextra -Wvla -Werror -std=c11`  
Use `-m32` to compile it for 32bit and `-m64` to compile it for 64bit. Note that you may need to install a package named `gcc-multilib` to compile with `-m32`.

#### Checkoff:

- Explain what `-Wpedantic`, `-Wall`, `-Wextra`, `-Wvla`, `-Werror` and `-std=c11` do.
- Show and explain your source code to the TA.
- Show the compilation of the 32bit and 64bit version to the TA.
- Run both programs and explain the sizes and differences.

---

The following TA(s) are responsible for this lab:

Suting Chen <`chenst` AT `shanghaitech.edu.cn`>