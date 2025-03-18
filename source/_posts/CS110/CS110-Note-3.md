---
title: CS110 Note [3]
date: 2024-02-24 16:32:40
description: "Instructions: Language of the Computer"
mathjax: true
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 笔记
---

# Operations of the Computer Hardware

## Instruction set

In the introduction, we have learned that the CPU have the basic instructions to perform, like `add`, `subtract`, `load`, `store`, `branch`, `jump`, etc. These instructions are the basic operations of the computer hardware.

By combining these basic operations, we can perform more complex operations, like `multiplication`, `division`, `array access`, `function call`, etc.

However, how to define the most basic operations of the computer hardware? Actually there's no standard answer to this question. We have many different designs which all ahiceve the same goal: using the basic operations to perform complex operations.

Different CPU using different instruction set have different basic operations could be performed. In my notes, I will always use the RISC-V instruction set as the example.

## Assembly Language

In the introduction, we also learned that the programming language to directly describe these operations is called *assembly language*.

> Here's a example of the RISC-V assembly language:
> 
> ```asm
> add x1, x2, x3
> ```
> 
> This instruction means that the value of `x2` and `x3` will be added together, and the result will be stored in `x1`.

Each instruction set have its own assembly language. Below is the little more detailed introduction of the RISC-V instruction set.

The table shows the **basic operations** of the RISC-V assembly language:
| Operation |       Name       | Example            | Description                     |
| :-------: | :--------------: | ------------------ | ------------------------------- |
|   `add`   |       Add        | `add x1, x2, x3`   | `x1 = x2 + x3`                  |
|   `sub`   |     Subtract     | `sub x1, x2, x3`   | `x1 = x2 - x3`                  |
|  `addi`   |  Add immediate   | `addi x1, x2, 100` | `x1 = x2 + 100`                 |
|   `ld`    | Load doubleword  | `lw x1, 100(x2)`   | `x1 = Memory[x2+100]`           |
|   `sd`    | Store doubleword | `sw x1, 100(x2)`   | `Memory[x2+100] = x1`           |
|   `beq`   | Branch if equal  | `beq x1, x2, 100`  | `if (x1 == x2) go to PC += 100` |
|   `jal`   |  Jump and link   | `jal x1, 100`      | `x1 = PC+4; go to PC += 100`    |

Clearly the tables are not complete, for the full list of the RISC-V assembly language, go check the [RISC-V is a Doc](https://msyksphinz-self.github.io/riscv-isadoc/html/).

> ***Tips:*** *(useful for reading Doc)*
>
>The base of RISC-V ISA is defined by the integer instruction set, which can be either 32-bit, 64-bit. This is indicated by the number following "RISC-V". For example:
>
>- **RISC-V32:** This specifies the 32-bit integer instruction set.
>- **RISC-V64:** This specifies the 64-bit integer instruction set.
>
> RISC-V is designed to be modular, allowing for various standard extensions that provide additional functionality beyond the base integer instruction set. These extensions are indicated by letters following the base designation. Some of the standard extensions include:
> 
> - **I:** Integer. This is the base integer instruction set and is always present. It includes instructions for arithmetic, logical operations, and control flow.
>
> - **M:** Multiply and Divide. This extension adds instructions for multiplication and division.
>
> - **A:** Atomic. This extension adds instructions for atomic memory operations, which are crucial for multi-threading and concurrency.
>
> - **F:** Single-Precision Floating-Point. This extension adds instructions for single-precision (32-bit) floating-point arithmetic.
>
> - **D:** Double-Precision Floating-Point. This extension adds instructions for double-precision (64-bit) floating-point arithmetic.
>
> - **C:** Compressed. This extension adds compressed instructions that are shorter than the standard instructions, allowing for more compact code. These instructions are also called psuedo-instructions.


# Operands of the Computer Hardware

The table shows the **operands** of the RISC-V instruction set:
|      Operand      |      Description      |
| :---------------: | :-------------------: |
|   `x1` to `x31`   |    $32$ Registers     |
| `(0)` to `(2^61)` | $2^{61}$ Memory words |



## Register Operands

Registers are the smallest and fastest memory, and they are used to store the intermediate results of the computation or the most frequent accessed data. 

Programs often have more variables than a computer's registers can hold. To manage this, compilers prioritize keeping frequently used variables in registers, relegating others to memory. This involves transferring variables between registers and memory as needed. The act of moving less frequently used variables to memory is known as *spilling registers*.

Because registers are embeded in the CPU, so the CPU can access the registers directly. Thus the access time is very short (faster than 0.25ns). Each register can store 64 bits of data. Since 64 bits is so often used, this size is called a *doubleword*. (while 32 bits is called a *word*.)

As the CPU can directly access the registers, we can surely use the registers in out assmebly code. The RISC-V instruction set have 32 registers, and they are named from `x1` to `x31`. So when we use those variables in the assembly code, we are using the register.

The follwing C code:
```c
f = (g + h) - (i + j);
```
should be compiled to the following RISC-V assembly code:
```asm
add x1, x2, x3
add x4, x5, x6
sub x7, x1, x4
```
If `f`, `g`, `h`, `i`, `j` are assigned to `x7`, `x2`, `x3`, `x5`, `x6` respectively. And `x1`, `x4` are used as the temporary registers.


An important thing to notice is that, **only on registers can arithmetic operations be performed**.

## Memory Operands

Limited amount of registers can not be used to construct the essential data structure, which sotres most of the data that programmer handles. But computer's main memory contains billions of *bytes*. Hence many of the data are kept in the main memory. 

Since 8 bits is so often used, this size is called a *byte*. And the memory is organized as a sequence of bytes. Each byte has a unique address, and the address is used to locate the data in the memory.

As we mentioned before, only registers can be used to perform arithmetic operations. So when we want to perform arithmetic operations on the data in the memory, we have to load the data from the memory to the registers, perform the operations, and then store the result back to the memory. Such instructions are called *data transfer instructions.*

To access the memory, we have to use the memory address. The memory address is a 64-bit number, and it is used to locate the data in the memory. The memory address is also called *pointer*. We can abstractly think of the memory as an one dimensional array, and the memory address is the index of the array. (Under the hood, the memory is actually a 2D matrix, the address contains the row and column info.)

The following C code:
```c
A[12] = h + A[8];
```
should be compiled to the following RISC-V assembly code:
```asm
ld x1, 64(x2)  // Temporarily store A[8] in reg x1
add x1, x1, x3  // Temporarily store h + A[8] in reg x1
sd x1, 96(x2)  // Store the result in A[12]
```
If variable `h` is assigned to `x3`, and the *base address* of the array `A` is assigned to `x2`.

The register `x2` is used to store the base address of an array, so it is called the *base register*. And the number `64` is called the *offset*. The offset is used to locate the specific element in the array. 

The offset in the assembly code of `A[8]` is `64` because the size of a doubleword is 8 bytes and one unique address corresponds to one byte, so the offset is `8 * 8 = 64`. The offset of `A[12]` is `12 * 8 = 96`. 

## Constant or Imemdiate Operands

Many times we need to perform arithmetic operations with a constant. To utilize the constant in the assembly code, a primitive way is to load the constant to a register, and then perform the operations. 
```asm
ld x1, Addr4(x3) // Load the constant 4 to x1
add x2, x1, x4 // x2 = x1 + x4 (where x1 == 4)
```

However, the RISC-V instruction set provides a more convenient way to perform the operations with a constant. The `addi` instruction can be used to add a constant to a register directly.
```asm
addi x2, x3, 4 // x2 = x3 + 4
```

The constant `0` plays a special role in RISC-V. We can negate the value of a register by using the `x0` register and the `sub` instruction.
```asm
sub x2, x0, x3 // x2 = 0 - x3 = -x3
```
We could use the `x0` register as the constant `0` because the `x0` is hardwired to the value `0` in the RISC-V instruction set.

# Representing Instructions in the Computer

## Machine Language

Although assembly language is already the lowest level of the programming language, but computer still can not understand the assembly language directly. The computer can only understand the binary code. So the assembly language has to be translated to the binary code before the computer can execute it.

These numeric version of instruction is called the *machine language*. And the sequence of the machine language is called the *machine code*.

Infact the assembly language is close to a human readable form of the binary code, so it is easy to translate the assembly language to the binary code. The following is the translation of the `add x1, x2, x3` instruction to the binary code:

- Assembly language:
    ```asm
    add x1, x2, x3
    ```

- Binary code:
    ```bin
    0000000 00011 00010 000 00001 0110011
    ```

- Writing the binary code in the decimal form:
    ```dec
    0 3 2 0 1 51
    ```

Each of the segments of the binary code has a specific meaning. We call those segments of the binary code the *fields* of the instruction. The fields of the instruction are used to specify the operation to be performed, the operands of the operation, and the result of the operation.

Here a table shows the fields layout of `add` instruction:
| funct7 |  rs2  |  rs1  | funct3 |  rd   | opcode |
| :----: | :---: | :---: | :----: | :---: | :----: |
|  7bit  | 5bit  | 5bit  |  3bit  | 5bit  |  7bit  |

The `opcode` field is used to specify the operation to be performed. The `rd`, `rs1`, `rs2` fields are used to specify the operands of the operation. The `funct3` and `funct7` fields are used to specify the result of the operation.

And the layout of the fields of the instruction is called the *instruction format*. The RISC-V instruction set have three instruction formats: *R-type*, *I-type*, and *S-type*. The `add` instruction is a R-type instruction.

## Instruction Formats

`addi` instruction is a I-type instruction example, and `ld` instruction is a S-type instruction example. The following table shows the fields layout of I-type and S-typeinstruction:
- I-type instruction format:
    | imm[11:0] |  rs1  | funct3 |  rd   | opcode |
    | :-------: | :---: | :----: | :---: | :----: |
    |   12bit   | 5bit  |  3bit  | 5bit  |  7bit  |

- S-type instruction format:
    | imm[11:5] |  rs2  |  rs1  | funct3 | imm[4:0] | opcode |
    | :-------: | :---: | :---: | :----: | :------: | :----: |
    |   7bit    | 5bit  | 5bit  |  3bit  |   5bit   |  7bit  |

And then we repeat the layout of R-type for a clear view:
- R-type instruction format:
    | funct7 |  rs2  |  rs1  | funct3 |  rd   | opcode |
    | :----: | :---: | :---: | :----: | :---: | :----: |
    |  7bit  | 5bit  | 5bit  |  3bit  | 5bit  |  7bit  |

There are different instruction formats because different instructions have different number of operands. And the operands of the different instructions have different size. We have to choose whether to use longer and more uniform length or shorter and more varied length of instructions. The RISC-V instruction set choose the latter.

Two thing to notice:
1. **The `opcode` field is the first field** of the instruction format, we read the fields from right to left. And in the field, we read the bits from left to right.

2. In the S-type instruction, the immediate value is divided, with the higher order bits (imm[11:5]) placed at the left of the instruction and the lower order bits (imm[4:0]) positioned at the right. i.e. **we read the immediate parts from left to right.**

## The Big Picture
Today's computers are built on two key principles:
1. Instructions are represented as numbers.
2. Programs are stored in memory to be read or written, just like data.

These principles lead to the stored-program concept; its invention let the computing genie out of its bottle. Figure 2.7 shows the power of the concept; specifically, memory can contain the source code for an editor program, the corresponding compiled machine code, the text that the compiled program is using, and even the compiler that generated the machine code.

One consequence of instructions as numbers is that programs are often shipped as files of binary numbers. The commercial implication is that computers can inherit ready-made software provided they are compatible with an existing instruction set. Such “binary compatibility” often leads industry to align around a small number of instruction set architectures.


# Logical Operations

## Bitwise Operations

During the development of the computer, it soon became clear that it was useful to perform operations on the individual bits of the data instead of a bundle of bits at once. The operations that perform on the individual bits of the data are called the *bitwise operations*.

Here's a table shows the bitwise operations of the RISC-V instruction set:
|       Operation        |  Instruction  | Example          | Description     |
| :--------------------: | :-----------: | ---------------- | --------------- |
|   Shift left logical   | `sll`, `srli` | `slli x1, x2, 3` | `x1 = x2 << 3`  |
|  Shift right logical   | `srl`, `srli` | `srli x1, x2, 3` | `x1 = x2 >> 3`  |
| Shift right arithmetic | `sra`, `srai` | `srai x1, x2, 3` | `x1 = x2 >> 3`  |
|      Bitwise AND       | `and`, `andi` | `and x1, x2, x3` | `x1 = x2 & x3`  |
|       Bitwise OR       |  `or`, `ori`  | `or x1, x2, x3`  | `x1 = x2 \| x3` |
|      Bitwise XOR       | `xor`, `xori` | `xor x1, x2, x3` | `x1 = x2 ^ x3`  |
|      Bitwise NOT       |    `xori`     | `not x1, x2`     | `x1 = ~x2`      |

## Shift Operations

The shift operations are used to move the bits of the data to the left or to the right. The `sll` instruction is used to shift the bits of the data to the left, and the `srl` instruction is used to shift the bits of the data to the right. The `sra` instruction is used to shift the bits of the data to the right, and the most sign bit of the old data will be filled with the original value of the sign bit.

If we use constant to determine the number of bits to be shifted, we can use the `slli`, `srli`, and `srai` instructions, where the `i` in the instruction name stands for *immediate*.

Here's some examples of the shift operations:
- Shift left 3 bits:

    `00000101` to `00101000`

- Shift right 3 bits:

    `10100000` to `00010100`

- Shift right arithmetic 3 bits:

    `10100000` to `11110100`

    `01100000` to `00001100`

## AND, OR, NOT, XOR

The previous operations are more like a whole operation on all the bits of the data. The AND, OR, NOT, XOR operations are more like the operations on the individual bits of the data. Because each result bit is determined by the corresponding bits of the operands, won't affect the bits on the other positions. 

Here's some examples of the bitwise operations:
- Bitwise AND:

    `10101010` and `11001100` to `10001000`

- Bitwise OR:

    `10101010` and `11001100` to `11101110`

- Bitwise XOR:

    `10101010` and `11001100` to `01100110`

- Bitwise NOT:
    
    `10101010` to `01010101`


Here's the truth table of the bitwise operations:
|   a   |   b   | a AND b | a OR b | a XOR b | NOT a |
| :---: | :---: | :-----: | :----: | :-----: | :---: |
|   0   |   0   |    0    |   0    |    0    |   1   |
|   0   |   1   |    0    |   1    |    1    |   1   |
|   1   |   0   |    0    |   1    |    1    |   0   |
|   1   |   1   |    1    |   1    |    0    |   0   |


# Instructions of Making Decisions

## Conditional

What distinguishes a computer from a simple calculator is its ability to make decisions according to the result of the computation.

RISC-V instruction set provides two instructions to make decisions: `beq` and `bne`, which represent the *branch if equal* and *branch if not equal* respectively. These two instructions are traditionally called the *conditional branches*. 

The first instruction `beq` used like this:
```asm
beq x1, x2, L1
```
This instruction means that if the value of `x1` is equal to the value of `x2`, then the program will jump to the statement labeled `L1`.

`bne` instruction simply means jump when not equal.

Here's a compiling example of the C code:
```c
if (i == j)
    f = g + h;
else
    f = g - h;
```
```asm
beq x1, x2, Else       // If i == j, go to Else
add x3, x4, x5         // f = g + h
beq x0, x0, Exit       // Go to Exit
Else: sub x3, x4, x5   // f = g - h
Exit:
```

## Loops

RISC-V instruction set don't have a specific instruction to perform the loops. But we can use the conditional branches to perform the loops, since loop is just keep jumping back to the same statement.

Here's two compiling example of the C code:
- While loop:
    ```c
    while (i < j)
        f = f + g;
    ```
    ```asm
    Loop: blt x1, x2, Exit   // If i < j, go to Exit
    add x3, x3, x4           // f = f + g
    beq x0, x0, Loop         // Go to Loop
    Exit:
    ```

- For loop:
    ```c
    for (i = 0; i < 10; i++)
        f = f + g;
    ```
    ```asm
    li x1, 0                  // i = 0
    Loop: bge x1, 10, Exit    // If i >= 10, go to Exit
    add x3, x3, x4            // f = f + g
    addi x1, x1, 1            // i = i + 1
    beq x0, x0, Loop          // Go to Loop
    Exit:
    ```


Such sequences of instructions that end in a conditional branch are called *basic blocks*. The basic blocks are the building blocks of the control flow of the program. One of the first early phases of compilation is to identify the basic blocks of the program.

## Boundary Checking Shortcut

The RISC-V instruction set also provides more conditional branches to make decisions. The `blt`, `bge`, `bltu`, `bgeu` instructions represent the *"branch if less than"*, *"branch if greater than or equal"*, *"branch if less than unsigned"*, and *"branch if greater than or equal unsigned"* respectively.

Since we already know the number representaion of the signed and unsigned numbers, we can image tha the way to compare the signed and unsigned numbers are different. However, this dosen't means that we can not use unsigned comparison to compare the signed numbers. And this actually offers a shortcut for boundary checking.

For a unsigned comparison, it's clear that we just need to compare the two numbers' binary code directly, like whose leftmost `1` is more left. As the signed number use the leftmost bit to represent the sign, which means negative numbers will always lager than any non-negative numbers if we use the unsigned comparison. And for a Boundary checking, we happens to need to check if a number is non-negative and less than a specific positive number. So we can use the unsigned comparison to perform the boundary checking.

```asm
bgeu x20, x11, IndexOutOfBounds // If x20 >= x11 or x20 < 0, go to IndexOutOfBounds
```

## Case/Switch Statement

Most programming languages provide a `case` or `switch` statement that allows the programmer to select one of many branches based on the value of an expression. A simple way to implement this feature in is to turn the `case` statement into a series of `if` statements. 

But the alternative way may be more efficient. The RISC-V instruction set provides the `jalr` instruction (*jump and link register*), which is used to jump to a specific statement with no condition. By encoding the selectable branches' addresses into a table and load the address we want into a register, we can use the `jalr` instruction to jump to that specific address. The table is called a *branch address table* or *branch table*.


# Supporting Procedures in Computer Hardware 

A *procedure* is a sequence of instructions that can solve a specific problem, which is suitable for reuse. In programming languages, a procedure has a more familiar name: *function*.

## What will a Procedure Do?

RISC-V don't have specific instructions to support the procedures, but the reason it doesn't have is that the procedures are supported by the combination of the basic operations and the conditional branches. **We can implement the concept of procedure abstractly**.

All of us are very skilled at high-level programming using functions. Since procedure are just an alias of the function, we can directly describe some features of the procedure:
- Taking parameters as input.

- Returning a result as output.

- The procedure can be called in the main program.

- The procedure can call other procedures.

Now, as we've known the basics of assembly language, how we exactly do to implement the procedures using the assembly language? Well, in the execution of a procedure, the assembly program must follow these six steps:
1. Put parameters in a place where the procedure can access them.
    - Using `add`, `mv` ...

2. Transfer control to the procedure.
    - Using `jalr`, `jal` ...

3. Acquire the storage resources needed for the procedure.
    - Using `sd`, `sw` ...

4. Perform the desired task.

5. Put the result value in a place where the calling program can access it.
    - Using `add`, `mv` ...

6. Return control to the point of origin, since a procedure can be called from several points in a program.
    - Using `ret`/`jal`, `ld`, `lw` ...

## How procedure abstractly implemented?

### Sharing the Registers
Recall that, we implement procedures in an abstract way. Why we call it abstract? 

In the high-level programming, we consider the procedure has its own memory space. we call functions in high-level programming, except we passed pointers as the parameters, we won't worry about the function changes the things "outside" the function. This is because the main memory is big enough to sperate the memory spaces. 

But in RISC-V, we are persuing using registers as much as possible, and the registers are very limited, just exactly 32 registers in total and 64bits large per register.

Thus, when the *caller* calls a procedure, the caller will lose the control of the registers. After the calling, all the registers turn to serve the *callee*. 

### Communicating between the Caller and the Callee

Then how caller and callee communicate? In the high-level programming, the caller and callee communicate through the parameters and the return value. 

In RISC-V, we follow a convention, or could be called a protocol, to let the caller and callee communicate. Just as the six steps we mentioned above, before the caller calls the callee, the caller will put the parameters in the specific registers, and after the callee returns, the callee will put the return value in the specific registers.

By convention, we use:
-  `x10` to `x17` as the parameter registers.
-  `x1` to store the address of the next instruction after the jump instruction (for calling procedure) made by the caller. 

### Saving and Restoring the States of the Registers

Communicating issue solved, but not only caller need the return value, after the control back, we need to restore states of the registers for the continuation of the caller's execution. So the callee should also save the states of the registers before it uses the registers, and restore the states of the registers before return.

But now we have a new problem, to store the states of the registers, we need to transfer them into main memory and tansferring data between the memory and the registers is very slow. If this transfer happens frequently, the performance of the program will be very low. How to solve this problem? We can add some new rules to the protocol, like the callee can use some specific registers without saving the states, while some other registers must be saved before use and restored before return.

By convention, we use:
- `x5` to `x7` and `x28` to `x31` as temporary registers that are not preserved by the callee on a procedure call.
- `x8` to `x9` and `x18` to `x27` as saved registers that must be preserved on a procedure call.

### Return

In the previous content, we only know we need to go back to the *call site* (the position where the caller calls), and focus on how caller and callee communicating with eachother. Yet we don't know how to make sure that we could go back to the call site.

Recall when we use jump instruction to call callees, like `jal` or `jalr`, they have an additional step before jumping: linking.

The linking means to put `PC + 4` into the `x1` (we store the return address here). `PC` stands for *Program Counter*, which is special register not involved in the 32 normal register. Its value is the address of current executing instruction in the program. 

Since each instruction is 4-byte and each address mapped to one byte of momery, the address of the instruction next the jump instruction is the jump instruction's address plus $4$, i.e. `PC + 4` when executing the jump instruction.

Thus when callee finished his job and ready to go back, callee could directly jump to the address stored in `x1` and start executing immediately.

## Datas in the main memory

As said above, we need to store some data in the main memory. If all the procedures won't call another procedure, which we call a *leaf procedure*, life will be much easier.

But it turns out that even the simplest program that print out `"Hello, World!"` coded in C will call tons of procedures. This leads to so many data transfer between the memory and the registers, and the data we stored in main memory contains the varibles of many different precedures. 

Now the question is, how we determine the address of the data stored in or loaded from the main memory? Besides, we also need a way to distinguish the data of different procedures in the main memory. 

Recall that in C, one function could only access the variables in the function itself except the global variables. These variables are called the *local variables*. The local variables were wrapped up by a concept called *frame*. One frame contains the local variables of a called function. As one function calls another function, new frames are generated. We put these frames in the main memory together, and call these bunch of frames the *stack*.

To effectively manage data in main memory when using assembly language, **especially when dealing with nested or recursive procedure calls**, the concepts of the stack and frame are crucial. These concepts not only help in distinguishing the data of different procedures but also streamline the process of storing and retrieving procedure-specific data.

### The Stack

The stack is a structured portion of the computer's main memory used for storing temporary data such as procedure parameters, return addresses, and local variables. It operates on a Last In, First Out (LIFO) principle, meaning that the last piece of data pushed onto the stack will be the first one to be popped off. This structure is particularly useful for managing data in nested procedure calls, as it ensures that each procedure call has access to its own set of data without interfering with others.

When a procedure is called, a new block of memory on the stack, **known as a stack frame or activation record**, is allocated for that procedure. This stack frame contains all the necessary information for the procedure, including its parameters, local variables, and the return address. Once the procedure completes its execution and returns, its stack frame is deallocated, and control is handed back to the calling procedure, which resumes execution right where it left off.

### The Frame

A frame, or an activation record, is a specific section of the stack allocated for a single execution of a procedure. The frame includes several key components:

- **Parameters**: The values passed to the procedure upon calling.
- **Return Address**: The point in the program to return to once the procedure execution is completed.
- **Saved Registers**: Registers that need to be preserved across procedure calls, as per the calling convention, are saved here.
- **Local Variables**: Variables that are declared within the scope of the procedure.
- **Temporary Data**: Space for data that is temporarily needed during the execution of the procedure.

The layout of a frame is determined by the calling convention used by the system, which dictates how parameters are passed, where the return address is stored, and which registers are saved.

### Managing the Stack and Frame

To manage the stack and frames, the RISC-V architecture (like many other architectures) uses a pair of registers:

- **Stack Pointer (SP)**: This pointer points to the top of the stack. It is automatically updated as data is pushed to or popped from the stack. In RISC-V, `x2` is often used as the stack pointer.
- **Frame Pointer (FP)**: This **optional** pointer **always** points to the base of the current stack frame, making it easier to access the frame's components. While not always necessary, using a frame pointer can simplify the code for accessing local variables and parameters, especially in deeply nested calls or when frames have variable sizes. Typically, `x8` is used as the frame pointer.

When a procedure is called, the stack pointer is adjusted to allocate space for the new frame. The procedure's parameters, return address, and any necessary saved registers are then stored in this new frame. Upon completion of the procedure, the saved registers are restored, the return address is used to jump back to the calling procedure, and the stack pointer is adjusted to deallocate the frame.

This structured approach allows for efficient, organized management of procedure calls and returns, especially in complex programs with many nested procedure calls. It ensures that each procedure has access to its own data and resources, while maintaining the integrity and continuity of the program's execution flow.

Here's are two example of precudure and nested procedure's assembly code:
```asm
add_numbers:
    // Prologue
    addi sp, sp, -16   // Allocate space on the stack for 2 words (16 bytes)
    sd ra, 8(sp)       // Save the return address on the stack
    sd s0, 0(sp)       // Save the frame pointer (s0) on the stack
    addi s0, sp, 16    // Set up the new frame pointer

    // Function body
    add a0, a0, a1     // Perform the addition; result is in a0

    // Epilogue
    ld ra, 8(sp)       // Restore the return address from the stack
    ld s0, 0(sp)       // Restore the frame pointer (s0) from the stack
    addi sp, sp, 16    // Deallocate space on the stack
    ret                // Return to caller
```

```asm
factorial:
    addi sp, sp, -16   // Allocate space on the stack for 2 words (16 bytes)
    sd ra, 8(sp)       // Save the return address on the stack
    sd a0, 0(sp)       // Save the argument a0 on the stack since we'll call this function recursively

    // Base case: if n <= 1, return 1
    li t0, 1           // Load immediate value 1 into temporary register t0
    ble a0, t0, end_recursion // If a0 <= 1, jump to end_recursion

    // Recursive case: n * factorial(n-1)
    addi a0, a0, -1    // Decrement n by 1
    jal ra, factorial  // Recursive call to factorial with n-1

    // After returning from recursion, multiply n by the result
    ld a0, 0(sp)       // Restore the original value of n
    mul a0, a0, a1     // Multiply n by factorial(n-1), result is in a0

end_recursion:
    li a0, 1           // If we hit the base case, set result to 1
    ld ra, 8(sp)       // Restore the return address from the stack
    addi sp, sp, 16    // Deallocate space on the stack
    ret                // Return to caller
```

### Heap

Corresponding to the stack, the concept of *heap* may also be familiar to you. Heap is also a data structure, but the concept here is not about the data structure, but the memory management.

Up to now, all we stored in the main memory are static data, which means the size of the data is determined when we write the program. But in the real world, we often need to store the data whose size is determined when the program is running. This kind of data is called the *dynamic data*. The dynamic data is stored in the heap.

When using stack pointer, the original stack pointer stores the address of the top address table of the main memory. When we want to allocate space for a new frame, we need to move down the stack pointer, i.e. subtract the stack pointer.

In the contrary, the address allocated for the dynamic data in the heap is always increasing. 


## Global

A C variable is generally a location in storage, and its interpretation depends both on its type and storage class. C has two storage classes: automatic and static. Automatic variables are local to a procedure and are discarded when the procedure exits. Static variables exist across exits from and entries to procedures. C variables declared outside all procedures are considered static, as are any variables declared using the keyword static. The rest are automatic. To simplify access to static data, some RISC-V compilers reserve a register `x3` for use as the* global pointer*, or `gp`.


## Full Calling Convention

Finally, we've introduced all the key concepts when we implement the procedures in the assembly language. Now we can summarize the full calling convention of the RISC-V instruction set:

1. **Parameter Passing**: Parameters are passed in registers `x10` to `x17`. If there are more than 8 parameters, the remaining parameters are passed on the stack.

2. **Return Value**: The return value is passed in register `x10`.

3. **Saved Registers**: Registers `x8` to `x9` and `x18` to `x27` must be saved by the callee if they are used.

4. **Temporary Registers**: Registers `x5` to `x7` and `x28` to `x31` can be used by the callee without saving their states.

5. **Stack Pointer**: Register `x2` is used as the stack pointer.

6. **Frame Pointer**: Register `x8` is used as the frame pointer, but it is optional.

7. **Return Address**: The return address is stored in register `x1`.

8. **Zero**: Register `x0` is hardwired to the value `0`.

But if we always need to remember these rules, it will be very hard to write the assembly code. Thus, like the pseudo-instructions (compressed-instruction) that assembler can translate to the real instructions, **we can also use the *nick name* (formally *ABI Name*) of those registers to represent the registers.** Here's a table shows the nick names of the registers:

| Register | ABI Name | Description                      | Saver  |
| -------- | -------- | -------------------------------- | ------ |
| x0       | zero     | Hard-wired zero                  |        |
| x1       | ra       | Return address                   | Caller |
| x2       | sp       | Stack pointer                    | Callee |
| x3       | gp       | Global pointer                   |        |
| x4       | tp       | Thread pointer                   |        |
| x5-x7    | t0-t2    | Temporaries                      | Caller |
| x8       | s0/fp    | Saved register/frame pointer     | Callee |
| x9       | s1       | Saved register                   | Callee |
| x10-x17  | a0-a7    | Function arguments/return values | Caller |
| x18-x27  | s2-s11   | Saved registers                  | Callee |
| x28-x31  | t3-t6    | Temporaries                      | Caller |


# RISC-V Addressing for Wide Immediates and Addresses

## Addressing

The concept of *Addressing* in computer science means the actual process when we use a number to express that we want to access a certain part of the memory.

Formally, the various *addressing modes* that are defined in a given instruction set architecture define:
- How the machine language instructions in that architecture identify the operand(s) of each instruction. 

- How to calculate the effective memory address of an operand by using information held in registers and/or constants contained within a machine instruction or elsewhere.

For example, base addressing is a common way to access the data in the memory. We use the pattern `imme(reg)` to access the data in the memory. The `imme` is the offset, and the `reg` is the base address of the array. The `imme` is added to the `reg` to get the address of the data in the memory. Thus the actual address we are accessing is `imme + reg`.

We're going to mainly talk about the addressing of wide/large immediate operands and branches' addresses 

## Wide Immediate Operands

As we know, in the RISC-V32, the size of instructions are 32-bit. And if we use those instructions contain immediate, like `addi`, it will be encoded into the instruction itself when assembling. If you don't know that, just check out the format of I type instruction.

Most of the instructions only have 12-bit field for storing the immediate. But the size of register is 64-bit, which means we could store 32-bit number in the register. And indeed, there's a instruction `li` allowing us to load a 32-bit immediate into a register directly. 

However, so long as you understand the instruction size are only 32-bit, you can image `li` must be not a normal instruction, since some of bits must used for storing `opcode`, `func3` ... 

Yep, it's a pseudo instruction. What `li` does, is utilizing `lui` to load the first 20 bits into register, and use `addi` to load the remaining 12 bits.

For an conrete example:
```
li x19, 3998976
```
where 
$$
    3998976_{10} =  00000000\;00111101\;00000101\;00000000_2
$$
, will be translated into:

```
lui  x19, 976
addi x19, x19, 1280
```

where 
$$
    976_{10} = 0000\;0000\;0011\;1101\;0000_2
$$
and 
$$
    1280_{10} = 00000101\;00000000_2
$$
.

As you can see, $976$ is the integer representation of the first 20 bits of $3998976$. And $1280$ is the integer representation of the last 12 bits of $3998976$.

What `lui` exactly will do is shifting right the `imme` ($976$ here) by 12 bits and store the shifted number in the `reg` (`x19` here). Just as it's name: **Load the number into the register as if it is the upper 20-bit part of an integer**.

> In the previous example, bit 11 of the constant was `0`. If bit 11 had been set, there would have been an additional complication: the 12-bit immediate is sign-extended, so the addend would have been negative. This means that in addition to adding in the rightmost 11 bits of the constant, we would have also subtracted $2^{12}$. To compensate for this error, it suffices to add `1` to the constant loaded with `lui`, since the `lui` constant is scaled by $2^{12}$.

## Addressing in Branches

The branch instructions in the RISC-V instruction use the instruction format called *SB-type*.

For example, `bne x10, x11, 2000` will be represented in machine code as:
| imm[12] | imm[10:5] |  rs2  |  rs1  | funct3 | imm[4:1] | imm[11] | opcode  |
| :-----: | :-------: | :---: | :---: | :----: | :------: | :-----: | :-----: |
|    0    |  111110   | 01011 | 01010 |  001   |   1000   |    0    | 1100111 |

> The reason why this format don't cover `imm[0]` is because each instruction is at least 16-bit or 2-byte (RVC set). So the address distance between the instructions is at least 2 (recall each address corresponds to one byte of memory). Thus the `imm` is always a multiple of 2, and the last bit of `imm` is always `0`, so we leave it implicit, like the implicit `1` in IEEE 754.

As you can see, the immediate field could only cover the range of $[−4096,\;4094]$, and if we use the direct addressing mode, we can only have 2192 instruction in one program, each instruction takes one address. If this be true, then we're done, at least you won't see any modern program today.

And a similar format *UJ-type*, which is only used by `jal`, also only have 20 bits for the immediate.

For example, `jal x0, 2000` will be represented in machine code as:
| imm[20] | imm[10:1]  | imm[11] | imm[19:12] |  rd   | opcode  |
| :-----: | :--------: | :-----: | :--------: | :---: | :-----: |
|    0    | 1111101000 |    0    |  00000000  | 00000 | 1101111 |

Even if we only use `jal` to jump, if addresses of the program still had to fit in this 20-bit field, no program could be bigger than $2^{20}$.

So RISC-V have to seek a way solving this, and the way is: *PC-relative addressing*

It help us jump by setting the Program Counter in this way:
```
PC = PC + Branch offset
```

The key idea here is: **For the most of time, the distance we want to jump is not that large.**

This still limits us that we can only branch within $\pm2^{10}$ words of the current instruction, or jump only within $\pm2^{18}$ words of the current 
instruction, But all loops and if statements are smaller than $2^{10}$ words.

## Addressing in Storing and Loading

Notice the syntax of the Load and Store function:
```
sd x1, 100(x2)
ld x1, 100(x2)
```

The pattern `imme(reg)` is used to access the data in the memory. The `imme` is the offset, and the `reg` is the base address of the array. The `imme` is added to the `reg` to get the address of the data in the memory. Thus the actual address we are accessing is `imme + reg`.

This is called the *base addressing mode*. And it actually hint us, the amount of the memory corresponds the bits of the memory address, which corresponds to the bits of the register.


# Parallelism and Instructions: Synchronization

In the world of computing, particularly within the RISC-V architecture, achieving parallelism often requires tasks to work in tandem. However, this collaboration introduces the need for synchronization to ensure that tasks don't interfere with each other, potentially leading to *data races*. Data races occur when multiple threads access the same memory location without proper synchronization, and at least one of them is writing to it.

## Data Race and Synchronization

Consider the analogy of reporters working on different sections of a story; if one reporter needs to read all sections before concluding, they must know when others have finished. **If the latter reporter make the conclusion earlier than the completion of the former reporter, then the conclusion might missing the evaluation of the unfinished part.** This is called a data race.

This scenario emphasizes the need for synchronization to prevent any changes that could affect the final outcome.

In computing, synchronization is typically managed through software routines that utilize hardware synchronization instructions. These instructions are crucial for implementing lock and unlock operations, which ensure that only one processor can access a specific region at a time—a concept known as mutual exclusion.

## Atomic Operations: The Foundation of Synchronization

Atomic operations means: **Some operations are strongly bonded and can't be split (like an atom).**

The cornerstone of synchronization in a multiprocessor environment is the ability to atomically read and modify a memory location. Without this, the complexity and cost of building synchronization mechanisms would skyrocket with an increasing number of processors.

A basic example of such an operation is the atomic exchange or swap, which allows a value in a register to be exchanged with a value in memory atomically. This operation can be used to implement a simple locking mechanism, ensuring that only one processor can acquire the lock at a time.

## RISC-V's Approach to Synchronization

RISC-V introduces a pair of instructions, load-reserved (`lr.d`) and store-conditional (`sc.d`), to facilitate atomic operations. 

These instructions work together to ensure that if the memory location used by the load-reserved instruction is altered before the store-conditional instruction is executed, if some instruction tries to modify the location before `sc.d`, it fails. 

This mechanism allows for the implementation of atomic exchanges and other synchronization primitives.

## Practical Applications and Considerations

While atomic exchange is a powerful tool for multiprocessor synchronization, it's also beneficial for single-processor systems in managing multiple processes. Furthermore, the load-reserved/store-conditional mechanism can be extended to build more complex synchronization primitives, such as atomic compare-and-swap or atomic fetch-and-increment.

However, developers must be cautious about the instructions placed between the load-reserved and store-conditional instructions to avoid deadlocks and ensure the atomicity of the operation.



# Translating and Starting a Program

So far we've learned so much about RISC-V's instructions and how to organize them in assembly language. And we've tried to translate many code snippets from C to RISC-V assembly language. **But seriously, how we really translate a C program into a RISC-V assembly program?**

Well, as the beginning, here's a big picture of the process of translating a C program into a RISC-V assembly program:

{% mermaid graph TD %}
A["C Program"] -->|Compiler| B["Assembly Language Program"]
B -->|Assembler| C["Object: Machine Language Module"]
D["Object: Library Routine<br>(Machine Language)"] -->|Linker| E["Executable: Machine Language Program"]
C -->|Linker| E
E -->|Loader| F["Memory"]
{% endmermaid %}

Let's then talk about these with more details.

## Compiler

The compiler transforms the C program into an assembly language program, a symbolic form of what the machine understands. High-level language programs take many fewer lines of code than assembly language, so programmer productivity is much higher.

In 1975, many operating systems and assemblers were written in assembly language because memories were small and compilers were inefficient. Themillion-fold increase in memory capacity per single DRAM chip has reduced program size concerns, and **optimizing compilers today can produce assembly language programs nearly as well as an assembly language expert, and sometimes even better for large programs.**


## Assembler

Assembly language is a symbolic representation of machine language. Thus the major task of the assembler is to translate each instruction into the corresponding machine language instruction. That is actually pretty straightforward, we know the format of each instruction, and **we just need to replace the symbolic representation with the binary code according to the format.**

However people don't satisfy with this major goal, they also let the assembler to support some instruction are not supported by the hardware, but easy to implement by some other original instructions. This is called the *pseudo-instruction*, and we've already introduced some of them. 

### General Process of Assembling

1. **Preprocessing**: The assembler reads the assembly language program and processes any preprocessor directives, such as `#include` or `#define`. Comments and whitespace are also removed during this phase.

2. **Lexical Analysis**: The assembler reads the assembly language program and breaks it into *tokens*, such as instructions, labels, and operands. During this process, some invalid tokens may be detected and reported as errors.

3. **Parsing**: The assembler use the tokens to build a *abstract syntax tree (AST)* of the program. AST are very useful for checking the syntax and translating the program. Assemblers also keep track of labels used in branches and data transfer instructions in a *symbol table*.
    > The AST is a tree representation of the syntactic structure of the program. for example, the `add x1, x2, x3` will be represented as:
    > ```
    >      add
    >    /  |  \
    >   x1  x2 x3
    > ```

4. **Symbol Resolution**: This phase resolves the addresses corresponding to all labels using the symbol table built in the parsing phase. The actual address should be written in to the ASTs.

5. **Encoding**: The assembler translates the ASTs into machine language instructions. The assembler also generates the relocation information for the linker to resolve the addresses of external symbols.

### The final output of the Assembler

The object file for UNIX systems typically contains six distinct pieces:

- The *object file header* describes the size and position of the other pieces of the object file.

- The *text segment* contains the machine language code.

- The *static data segment* contains data allocated for the life of the program. (UNIX allows programs to use both static data, which is allocated throughout the program, and dynamic data, which can grow or shrink as needed by the program.)

- The *relocation information* identifies instructions and data words that depend on absolute addresses when the program is loaded into memory.

- The *symbol table* contains the remaining labels that are not defined, such as external references.

- The *debugging information* contains a concise description of how the modules were compiled so that a debugger can associate machine instructions with C source files and make data structures readable.

These routines help link the object file with the library routines and other object files to create an executable file.

## Linker

Completely recompile the whole program every time is definitely a horrible idea. Thus people would like to separate the program into several modules, and compile them separately, then *link* them together.

Each module is compiled into an object file, and the linker combines these object files into an executable file.

### General Process of Linking

There are three steps for the linker:
1. Place code and data modules symbolically in memory.
2. Determine the addresses of data and instruction labels.
3. Patch both the internal and external references.

The linker uses the relocation information and symbol table in each object module to resolve all undefined labels. Such references occur in branch instructions and data addresses, so the job of this program is much like that of an editor: it finds the old addresses and replaces them with the new addresses. Editing is the origin of the name “link editor,” or linker for short.

If all external references are resolved, the linker next determines the memory locations each module will occupy. When the linker places a module in memory, all *absolute* references, that is, memory addresses that are not relative to a register, must be relocated to reflect its true location.

### The final output of the Assembler

The linker produces an *executable file* that can be run on a computer. **Typically, this file has the same format as an object file, except that it contains no unresolvedreferences.** 

It is possible to have partially linked files, such as library routines, thatstill have unresolved addresses and hence result in object files.

### A Linking Example
This example Links the two object files below and then Shows updated addresses of the first few instructions of the completed executable file. 

We show the instructions in assembly language just to make the example understandable; **in reality, the instructions would be numbers.** 

Note that in the object files we have "italic and bold" the addresses and symbols that must be updated in the link process: the instructions that refer to the addresses of procedures `A` and `B` and the instructions that refer to the addresses of data doublewords `X` and `Y`.

|   Object file header   |           |                  |            |
| :--------------------: | :-------: | :--------------: | :--------: |
|                        |   Name    |   Procedure A    |            |
|                        | Text size |     $100_8$      |            |
|                        | Data size |      $20_8$      |            |
|      Text segment      |  Address  |   Instruction    |            |
|                        |     0     | `ld x10, 0(x3)`  |            |
|                        |     4     |   `jal x1, 0`    |            |
|                        | $\vdots$  |     $\vdots$     |            |
|      Data segment      |    *0*    |     *( X )*      |            |
|                        | $\vdots$  |     $\vdots$     |            |
| Relocation information |  Address  | Instruction type | Dependency |
|                        |     0     |       `ld`       |  ***X***   |
|                        |     4     |      `jal`       |  ***B***   |
|      Symbol table      |  L abel   |     Address      |            |
|                        |  ***X***  |        -         |            |
|                        |  ***B***  |        -         |            |
|                        |   Name    |   Procedure B    |            |
|                        | Text size |     $200_8$      |            |
|                        | Data size |      $30_8$      |            |
|      Text segment      |  Address  |   Instruction    |            |
|                        |     0     | `sd x11, 0(x3)`  |            |
|                        |     4     |   `jal x1, 0`    |            |
|                        | $\vdots$  |     $\vdots$     |            |
|      Data segment      |  ***0***  |   ***( Y )***    |            |
|                        | $\vdots$  |     $\vdots$     |            |
| Relocation information |  Address  | Instruction type | Dependency |
|                        |     0     |       `sd`       |  ***Y***   |
|                        |     4     |      `jal`       |  ***A***   |
|      Symbol table      |  L abel   |     Address      |            |
|                        |  ***Y***  |        -         |            |
|                        |  ***A***  |        -         |            |

Procedure `A` needs to find the address for the variable labeled `X` to put in the load instruction and to find the address of procedure `B` to place in the `jal`  Procedure `B` needs the address of the variable labeled `Y` for the store instruction and the address of procedure A for its `jal` instruction.
  
| Executable file header |                            |                  |
| :--------------------: | :------------------------: | :--------------: |
|                        |         Text size          |     $300_8$      |
|                        |         Data size          |      $50_8$      |
|      Text segment      |          Address           |   Instruction    |
|                        | $0000\;0000\;0040\;0000_8$ | `ld x10, 0(x3)`  |
|                        | $0000\;0000\;0040\;0004_8$ |  `jal x1, 252`   |
|                        |          $\vdots$          |     $\vdots$     |
|                        | $0000\;0000\;0040\;0100_8$ | `sd x11, 32(x3)` |
|                        | $0000\;0000\;0040\;0104_8$ |  `jal x1, -260`  |
|                        |          $\vdots$          |     $\vdots$     |
|      Data segment      |          Address           |                  |
|                        | $0000\;0000\;1000\;0000_8$ |   ***( X )***    |
|                        |          $\vdots$          |     $\vdots$     |
|                        | $0000\;0000\;1000\;0020_8$ |   ***( Y )***    |
|                        |          $\vdots$          |     $\vdots$     |

Now the linker updates the address fields of the instructions. **It uses the instruction type field to know the format of the address to be edited.**

## Loader

Now that the executable file is on disk, the operating system reads it to memory and starts it. The loader follows these steps in UNIX systems:
1. Reads the executable file header to determine size of the text and data segments.

2. Creates an address space large enough for the text and data.

3. Copies the instructions and data from the executable file into memory.

4. Copies the parameters (if any) to the main program onto the stack.

5. Initializes the processor registers and sets the stack pointer to the first free location.

6. Branches to a start-up routine that copies the parameters into the argument registers and calls the main routine of the program. When the main routine returns, the start-up routine terminates the program with an exit system call.


## Dynamically Linked Libraries

The previous section describes the traditional approach to linking libraries before the program is run. Although this static approach is the fastest way to call library routines, it has a few disadvantages:

- The library routines become part of the executable code. **If a new version of the library is released that fixes bugs or supports new hardware devices, the statically linked program keeps using the old version.**

- It loads all routines in the library that are called anywhere in the executable, even if those calls are not executed. **The library can be large relative to the program**; for example, the standard C library on a RISC-V system running the Linux operating system is 1.5 MiB.

These disadvantages lead to *dynamically linked libraries (DLLs)*, where the library routines are not linked and loaded until the program is run. Both the program and library routines keep extra information on the location of nonlocal procedures and their names. 

In the original version of DLLs, the loader ran a dynamic linker, using the extra information in the file to find the appropriate libraries and to update all external references.

The downside of the initial version of DLLs was that it still linked all routines of the library that might be called, versus just those that are called during the running of the program. This observation led to the lazy procedure linkage version of DLLs, **where each routine is linked only after it is called**. The dynamic linker start the linking process when the routine is first called during the execution of the program.
