---
title: CS110 Project 1.2
mathjax: false
description: A simple 2D-Convolution algorithm in C and RISC-V and a padded 2D-Convolution in RISC-V.  (Individual Project)
date: 2024-04-02 11:26:20
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 项目
---

# Project 1.2: 2D-Convolution in C and RISC-V (Individual Project)

[Computer Architecture I](https://robotics.shanghaitech.edu.cn/courses/ca/22s/) [ShanghaiTech University](http://www.shanghaitech.edu.cn/)

{% post_link CS110/CS110-Project-1-1 Project 1.1 %} Project 1.2

  

## IMPORTANT INFO - PLEASE READ
The projects are part of your design project worth 2 credit points. As such they run in parallel to the actual course. So be aware that the due date for project and homework might be very close to each other! Start early and do not procrastinate.

## Introduction

In project 1.2, you will implement a simple 2D-Convolution algorithm in C and RISC-V and a padded 2D-Convolution in RISC-V. You can get the template code [here](https://classroom.github.com/a/cQZ__Y51)

## Background

### 2D-Convolution

2D-Convolution is the convolution applied using two matrices, image and kernel. The progress of applying 2D-Convolution is shown below.

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p1.2-web/2D-Convolution.gif)

The left most matrix is the image matrix and the middle matrix is the kernel matrix. Each step is a product-and-sum. We need to product the corresponding elements and sum the result up. Or you can refer to the mathematical representation below, where K_w and K_l represents width and length of the kernel matrix, respectively.

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p1.2-web/convolution-math.png)

### Zero Padding 2D-Convolution

As shown in the above animation, the result matrix is not the same size with the image matrix. To make them the same size, we can add a circle of zero around the image matrix, as shown below. Then we can get a same size result matrix. This is called Zero Padding Convolution.

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p1.2-web/Zero-Padding.gif)

The number of zero to add at right and left of the image matrix is (kernel_length-1)/2. The number of zero to add at top and buttom of the image matrix is (kernel_width-1)/2.

## Implementation

### Part1: 2D-Convolution in C

In part1, you are required to implement a 2D-Convolution using C.

#### Input

A reference input is already provided in the `input.txt` file. The first line indicates the length and width of image matrix. The following lines are the image matrix. After the image matrix, there will be the length and width of the kernel matrix, followed by the kernel matrix. Each line will end with a \n

```assembly
5 3
4 -13 -6 -24 11 
12 -22 -13 21 -27 
-21 0 -28 11 -30 
3 2
5 6 4 
-2 9 -3 

```

#### Output

You need to output the result matrix. The output format looks like below. There should be a \n at the end of each line and a space after each element (the last element in a line also needs to be followed by a space)

```
-265 -333 166 
2 -389 198 
    
```

### Part2: 2D-Convolution in RISC-V

In part2, you are required to implement a 2D-Convolution using RISC-V. To do mulitiplication in RISC-V, you could use mul rd rs1 rs2 instruction to store rs1 * rs2 into rd. See more in the green card.

#### Input

A reference input is already provided to you in the `input.S` file. The input for final tests will be the same format as the provided input except the matrix value.

In this part, we only promise that the length and width of kernel matrix will not be larger than that of image matrix. The image and kernel may not be a square and the length and width of the kernel may not be odd.

```assembly

.data
# length of image matrix
.globl image_length
image_length:
	.word 5
# width of image matrix
.globl image_width
image_width:
	.word 5
# image matrix
# -2    12   14   28   -13
#  1    11   3   -26    20
# -8    30   5    29   -24
#  27   4   -29   25   -13
# -27  -1   -21   17    5
.globl image
image:
	.word -2 12 14 28 -13 1 11 3 -26 20 -8 30 5 29 -24 27 4 -29 25 -13 -27 -1 -21 17 5 
# length of kernel matrix
.globl kernel_length
kernel_length:
	.word 2
# width of kernel matrix
.globl kernel_width
kernel_width:
	.word 2
# kernel matrix
# 0 3
# 0 6
.globl kernel
kernel:
	.word 0 3 0 6 
```

#### Output

You need to output the result matrix. The output format is the same with part1

It's usually the duty of the supervisor (operating system) to deal with input/output and halting program execution. Venus, being a simple emulator, does not offer us such luxury, but supports a list of primitive [environmental calls](https://github.com/ThaumicMekanism/venus/wiki/Environmental-Calls). You could use ecall to ask Venus for some specific functions. The following functions could be helpful.

| ID (`A0`) | NAME            | DESCRIPTION                         |
| --------- | --------------- | ----------------------------------- |
| 1         | print_int       | prints integer in `a1`              |
| 10        | exit            | ends the program with return code 0 |
| 11        | print_character | prints ASCII character in `a1`      |

- Each element in the result matrix ends with a space (ASCII: 32).
- Each row of the result matrix ends with a `\n` (ASCII: 10).

### Part3: Padded 2D-Convolution in RISC-V

In part3, you are required to implement Zero-Padding Convolution using RISC-V.

#### Input

The input format will be the same with part2. In part3, to simplify the progress, we promise that the kernel matrix will be a square and the length of it will be odd.

#### Output

The output format should be the same with part1.

## Test

The command that we use to test your program's correctness is

```shell
diff <your_transformed_output> <reference_output>
```

You can also test your result using this command.

## Execution

### Build & Execute C Program

1. Run make to compile the code and the executable file will be main. The Makefile compile all C source files together, so you can add any source files you want
2. To run your code, type ./main input_file output_file . input_file contains the image and kernel matrix. output_file is where you output your results to.
3. Run make test to test your codes with input.txt and your output file will be C_program.out

### Run RISC-V Program

You need java to run the venus in terminal. Try to run sudo apt install openjdk-17-jre to download java-17.

Make sure that `venus-jvm-latest.jar`, `Convolution.S/Padding-Convolution.S` and `input.S` reside in the same directory. To run your program locally and write the output to `RISCV_result.txt`, use the following command. (Note that this command will overwrite the result file even if there's something there)

```shell
java -jar venus-jvm-latest.jar Convolution.S > RISCV_result.txt
```

To debug your program online, you might want to replace `.import input.S` in `Convolution.S` with the content of `input.S`.

## Tips

- In all the tests, you don't need to consider the mulitiplication or addition overflow.
- You can use any risc-v instructions as long as the venus can recognize them.
- Handwritten assembly are postfixed with extension `.S` to distinguish from compiler generated assembly `.s`
- You can learn more about how to use ecall from [here](https://github.com/61c-teach/venus/wiki/Environmental-Calls).
- We will test your program using RISC-V emulator [venus](http://autolab.sist.shanghaitech.edu.cn/venus/). Actually almost all things you need can be learnt from [venus Wiki](https://github.com/ThaumicMekanism/venus/wiki).
- Learn save and load from memory using RISC-V.
- Be careful about the calling convention, it will make life easier.
- Write comments.
- The test cases are very friendly! Don't focus too much on the edge cases, focus on the correctness on the common cases.

## Submission

You should submit your code via Github. Please follow the guidance in Gradescope to submit your codes on Github. Please make sure you do not replace .import input.S with something else.

---

In Project 1.2 are,

Chundong Wang <`wangchd` AT `shanghaitech.edu.cn`>  
Siting Liu <`liust` AT `shanghaitech.edu.cn`>  

and,

Linjie Ma <`malj` AT `shanghaitech.edu.cn`>  
Xinxin Yu <`yuxx` AT `shanghaitech.edu.cn`>  

  
Last modified: 2024-04-01