---
title: CS110 Note [1]
mathjax: true
description: Computer Abstractions and Technology
date: 2024-02-23 09:37:51
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 笔记
---


# Eight Great Ideas in Computer Architecture

- **Design for Moore's Law**   
    > As computer designs can take years, the resources available per chip can easily double or quadruple between the start and finish of the project. Like a skeet shooter, computer architects must anticipate where the technology will be when the design finishes rather than design for where it starts. 

- **Use Abstraction to Simplify Design**
    > The complexity of computer systems makes it impossible to understand every detail of the hardware and software. Abstraction is a way to manage this complexity. By abstracting a system, we can ignore the irrelevant details and focus on the important ones.

- **Make the Common Case Fast**
    > Computer architects must design systems that are fast for most of the tims. This is often achieved by making the common case fast and the rare case correct. Ironically, the common case is often easier to make fast than the rare case.

- **Performance via Parallelism**
    > The performance of a computer system can be improved by doing more than one thing at a time. This is called parallelism. There are many forms of parallelism, including instruction-level parallelism, data parallelism, and task parallelism.

- **Performance via Pipelining**
    > Pipelining is a technique for implementing parallelism within a single processor. It works by breaking down the execution of instructions into a series of independent steps. This allows multiple instructions to be in progress at once. But if we only foucsing on what is piplining, we can image a human assembly line.

- **Performance via Prediction**
    > If a case could be faster on average by guessing the outcome, then it is worth the cost of the guess. It mostly happen when a case is easy to guess and the cost of wrong guess is low.

- **Hierarchy of Memories**
    > The memory system is organized in a hierarchy of different storage devices. Each device in the hierarchy is larger, slower, and cheaper per byte than the next smaller, faster, and more expensive device. The memory hierarchy is a key to understanding how computer systems achieve good performance at a reasonable cost.

- **Dependability via Redundancy**
    > Dependability is the ability to deliver correct service in the presence of faults. Redundancy is a key technique for achieving dependability. Redundancy can be applied to the hardware, the software, and the information. Just like the RAID in hard disk.

# Bellow Your Program

## From Application Software to Hardware

- **Application Software**
    > Application software is the software that is used by the users to perform specific tasks. It is used to solve a particular problem or to do a specific task. For example, word processing, spreadsheet, and database management system.

- **System Software**
    > System software is a type of computer program that is designed to run a computer's hardware and application programs. It is designed to provide a platform for running application software. There are many types of systems software, but two types of systems software are central to every computer system today: an operating system and a compiler. 

- **Hardware**
    > Hardware is the physical components of a computer system. It includes the monitor, keyboard, mouse, and the computer itself. The computer is made up of the CPU, memory, and storage devices.

## From High-Level Language to Machine Language

To speak with a machine, we need to be able to represent 2 states: 0 and 1, since the easiest signals for computers to understand are on and off, and so the computer alphabet is just two letters. This is called *binary*.  

But it is hard to write a program in binary. So we need a language that is easier to write and understand. This is called high-level language. But the computer can only understand binary. So we need a way to translate high-level language to machine language. This is called compiler.

Typically, compilers first translate high-level language to assembly language, and then translate assembly language to machine language.

Machine language directly read and execute by the computer, and assembly language is a direct hunman-readable representation of machine language.

High-level language $\to^{compiles}$ Assembly language $\to^{assembles}$ Machine language 

Example of compiling and assembling:

1. Source code:
    ```c
    #include <stdio.h>

    int main(void) 
    {
        printf("Hello, World!\n");
        return 0;
    }
    ```

2. Assembly code:
    ```asm
        .file	"hello.c"
        .intel_syntax noprefix
        .text
        .def	__main;	.scl	2;	.type	32;	.endef
        .section .rdata,"dr"
    .LC0:
        .ascii "Hello, World!\0"
        .text
        .globl	main
        .def	main;	.scl	2;	.type	32;	.endef
        .seh_proc	main
    main:
        push	rbp
        .seh_pushreg	rbp
        mov	rbp, rsp
        .seh_setframe	rbp, 0
        sub	rsp, 32
        .seh_stackalloc	32
        .seh_endprologue
        call	__main
        lea	rax, .LC0[rip]
        mov	rcx, rax
        call	puts
        mov	eax, 0
        add	rsp, 32
        pop	rbp
        ret
        .seh_endproc
        .ident	"GCC: (MinGW-W64 x86_64-ucrt-posix-seh, built by Brecht Sanders) 13.2.0"
        .def	puts;	.scl	2;	.type	32;	.endef
    ```

3. Machine code:
    ```bin
    000000000010010101010101000001100
    110101010101010100000111101010100
    ...
    ...
    ```

# Components of a Computer

## The Big Picture
The five classic components of a computer are briefly described below. They are:

- **Input**: 
    > This is the process of entering data and programs into the computer system. You should know that computer is an electronic machine like any other machine which takes as inputs raw data and performs some processing giving out processed data.

- **Output**: 
    > The result produced by a computer. It can be called as the processed data given by after data processing.

- **Memory**: 
    > This is the part of the computer that stores data. It stores data, intermediate results, and instructions (program).

- **Datapath**: 
    > A datapath is a collection of functional units such as arithmetic logic units (ALUs) or multipliers that perform data processing operations, registers, and buses. 

- **Control**: 
    > The component of the processor that commands the datapath, memory, and I/O devices according to the instructions of the program.

The processor (Control and Datapath) gets instructions and data from memory. Input writes data to memory, and output reads data from memory. Control sends the signals that determine the operations of the datapath, memory, input, and output.

Occasionally, people call the processor the CPU, for the more bureaucratic-sounding central processor unit.


## Slightly deeper into the Memory

### Volatile Memory
Storage, such as DRAM, that retains data only if it is receiving power.

- **Registers**: 
    > Registers are the fastest storage location in the memory hierarchy. They are used to store data that is being used by the CPU. The CPU can access the data in the registers very quickly. The number of registers in a CPU is limited. The registers are used to store the data that is being used by the CPU. 

- **Cache**:
    > Cache is a small and fast memory that is located between the CPU and the main memory. It is used to store the frequently accessed data and instructions. The cache memory is faster than the main memory. 
    >
    > Cache is built using a different memory technology, static random access memory (SRAM).

- **Main Memory**:
    > Main memory is the primary memory of the computer. It is used to store the data and instructions that are being used by the CPU. The main memory is slower than the cache memory. 
    >
    > Main memory is built using a different memory technology, dynamic random access memory (DRAM).

### Nonvolatile Memory
A form of memory that retains data even in the absence of a power source and that is used to store programs between runs. 

Such as flash memory, that retains data even when it is not receiving power.

- **Hard Disk**:
    > A form of nonvolatile secondary memory composed of rotating platters coated with a magnetic recording material. Because they are rotating mechanical devices, access times are about 5 to 20 milliseconds and cost per gigabyte in 2012 was \$0.05 to \$0.10.

- **Flash Memory**:
    > A nonvolatile semiconductor memory. It is cheaper and slower than DRAM but more expensive per bit and faster than magnetic disks. Access times are about 5 to 50 microseconds and cost per gigabyte in 2012 was \$0.75 to \$1.00


Most of the nonviolatile memory belongs to the secondary memory, which is used to store programs and data between runs.

# Performance

## Defining Performance

Typically, we use response time or throughput to measure the performance of a computer system.

- **Response Time (Execution time)**: 
    > The time it takes to complete some task. For example, the time it takes to load a web page or the time it takes to execute a program.

- **Throughput (Bandwidth)**:
    > The number of tasks completed per unit of time. For example, the number of web pages loaded per second or the number of programs executed per hour.


In the first few chapters, we will focus on the response time.


## Measuring Performance

We use time to measure the performance of a computer system. But the straightforward time (response time) actually contains many factors, such as memory access, I/O activities, and so on.

Also, a processor may simultaneously execute multiple programs, so the time to execute a program is not the same as the time to execute a program on a dedicated processor.

Hence, we often use CPU time to measure the performance, which is the time the CPU spends computing for a specific task. It can be further divided into user CPU time (only program time) and system CPU time (including OS time).

We'll use response time to measure the *system performance*, and use user CPU time to measure the *CPU performance*.

## Understanding Program Performance

Different programs have different performance requirements. For example, a web server needs to handle many requests per second, so it needs high throughput. A video game needs to render many frames per second, so it needs high throughput. A word processor needs to respond quickly to user input, so it needs low response time.

To improve the performance of a program, one must have a clear definition of what performance metric matters and then proceed to find the bottleneck of the program.


## CPU performance and Its Factors

### Clock

CPU has some basic utilities to perform when doing computions. For example, it needs to fetch instructions, decode instructions, execute instructions, and write results.

The *clock* set the speed of these basic actions. One tick of the clock is called a *clock cycle*, which represents CPU done one or multiple basic action.

The *clock rate* is the number of cycles per second. It is measured in hertz (Hz). The clock rate is also called the clock frequency.

### CPU time formula

The CPU time for a program is determined by the number of clock cycles, the clock cycle time, and the number of instructions.

$$
\text{CPU time} = \text{Clock cycles} \times \text{Clock cycle time} = \frac{\text{Clock cycles}}{\text{Clock rate}}
$$




### Instruction Performance

A program is a sequence of instructions. Each instruction needs multiple clock cycles to execute. The average number of clock cycles for an instruction is called the *CPI* (cycles per instruction).

So we can compute the clock cycles for a program by the following formula:
$$
\text{Clock cycles} = \text{CPI} \times \text{Instructions}
$$

Combining with the CPU time formula, we can get:
$$
\text{CPU time} = \text{CPI} \times \text{Instructions} \times \text{Clock cycle time} = \frac{\text{CPI} \times \text{Instructions}}{\text{Clock rate}}
$$


### How Program components affect CPU performance

| Hardware or software component | Affects what?                      | How?                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------ | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Algorithm                      | Instruction count, CPI             | The algorithm determines the number of source program instructions executed and hence the number of processor instructions executed. The algorithm may also affect the CPI, by favoring slower or faster instructions. For example, if the algorithm uses more divides, it will tend to have a higher CPI.                                                                                      |
| Programming language           | Instruction count, CPI             | The programming language certainly affects the instruction count, since statements in the language are translated to processor instructions, which determine instruction count. The language may also affect the CPI because of its features; for example, a language with heavy support for data abstraction (e.g., Java) will require indirect calls, which will use higher CPI instructions. |
| Compiler                       | Instruction count, CPI             | The efficiency of the compiler affects both the instruction count and average cycles per instruction, since the compiler determines the translation of the source language instructions into computer instructions. The compiler's role can be very complex and affect the CPI in varied ways.                                                                                                  |
| Instruction set architecture   | Instruction count, clock rate, CPI | The instruction set architecture affects all three aspects of  CPU performance, since it affects the instructions needed  for a function, the cost in cycles of each instruction, and the  overall clock rate of the processor.                                                                                                                                                                 |

# Power Wall

The power used by a computer is divided into two parts: dynamic power and static power.

## Dynamic Power
The dominant technology for integrated circuits is called CMOS (complementary metal oxide semiconductor). For CMOS, the primary source of energy consumption is so-called dynamic energy—that is, energy that is consumed when transistors switch states from 0 to 1 and vice versa. 

The dynamic energy depends on the capacitive loading of each transistor, the voltage and the frequency of switching:
$$
\text{Dynamic energy} = \frac{1}{2} \times \text{Capacitance} \times \text{Voltage}^2 \times \text{Frequency of switching}
$$

## Static Power
Although dynamic energy is the primary source of energy consumption in CMOS, static energy consumption occurs because of leakage current that flows even when a transistor is off. In servers, leakage is typically responsible for 40% of the energy consumption. Thus, increasing the number of transistors increases power dissipation, even if the transistors are always off. A variety of design techniques and technology innovations are being deployed to control leakage, but it's hard to lower voltage further.