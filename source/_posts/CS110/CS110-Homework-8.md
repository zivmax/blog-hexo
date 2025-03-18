---
title: CS110 Homework [8]
mathjax: false
date: 2024-05-28 11:21:15
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 作业
---

***HW 7 are handwritten.***

You can get the template code [here](https://classroom.github.com/a/NeKHp1pi)

<!--more-->

# Homework 8

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) @ [ShanghaiTech University](https://www.shanghaitech.edu.cn/)  

  
In this assignment, you will need to implement a toy simulator, where you can simulate the behavior of a paged virtual memory system and a fully-associative TLB.  


## 1. Introduction

Before you begin, make sure that you can answer the following questions:

1. What is address translation?
2. What is a multilevel page table?
3. What does TLB stores?
4. When do you need to flush TLB?

## 2. File structure

You shall see the following files in the starter repository:
```
root -+-- CMakeLists.txt
      |-- main.c
      |-- Makefile
      +-- inc
      |    |-- memory.h
      |    |-- process.h
      |    |-- PTE.h
      |    |-- simulator.h
      |    |-- TLB.h
      |    +-- utils.h
      |
      +-- src
           |-- memory.c
           |-- process.c
           |-- simulator.c
           +-- TLB.c
```
You will need one of `CMakeLists.txt` and `Makefile` to compile. You pick either of them. Gradescope will use the `Makefile`.  
Although we recommend you read every single line of code, it is possible to finish the assignment without even opening `memory.c` and `process.c`, where the resource allocation code is given.  
You will need to implement the following functions in the corresponding files:

- `simulator.h/c`: `status_t allocate_page(Process *process, addr_t address, addr_t physical_address)`
- `simulator.h/c`: `status_t deallocate_page(Process *process, addr_t address)`
- `simulator.h/c`: `status_t write_byte(Process *process, addr_t address, const byte_t *byte)`
- `simulator.h/c`: `status_t read_byte(Process *process, addr_t address, byte_t *byte)`
- `TLB.h/c`: `unsigned read_TLB(proc_id_t pid, unsigned vpn)`
- `TLB.h/c`: `void write_TLB(proc_id_t pid, unsigned vpn, unsigned ppn)`
- `TLB.h/c`: `void remove_TLB(proc_id_t pid, unsigned vpn)`

## 3. Function description

### 3.1. Simulator (in file `simulator.h/c`)

The online judge will test these four apis. So you should never modify the function signature of the four functions.  
The simulator is a two-level paging system, where the L1 page table takes `#define L1_BITS 12` bits as the index, and the L2 page table takes `#define L2_BITS 8` bits.  
You will need to manage memory properly and do page table walk to pass the testcases.

#### `status_t allocate_page(Process *process, addr_t address, addr_t physical_address)`:

- Allocate a page for the process.
- `process`: the process that we're working with.
- `address`: the virtual address of the page.
- `physical_address`: the physical address of the page.
- Return `SUCCESS` if the page is successfully allocated, otherwise return `ERROR`.

Note that `address` and `physical_address` are both addresses instead of page numbers, and the input is guaranteed to be page-aligned. It is possible (and should work perfectly) that two different virtual pages are mapped to the same physical page, no matter whether the two virtual pages are in the same process or not.  
To pass the testcases, you should return `ERROR` in these two scenarios: If the physical memory is not large enough to provide the required physical page. If the virtual page has already been allocated.

#### `status_t deallocate_page(Process *process, addr_t address)`:

- Deallocate a page for the process.
- `process`: the process that we're working with.
- `address`: the virtual address of the page.
- Return `SUCCESS` if the page is successfully deallocated, otherwise return `ERROR`.

Similar to the allocation process above, the input is guaranteed to be page-aligned. It is possible that the virtual page is not allocated, you should return `ERROR` in this case. In addition, if the last page in the L2 page table is deallocated, you should also deallocate the page table to save memory.

#### `status_t write_byte(Process *process, addr_t address, const byte_t *byte)` and `status_t read_byte(Process *process, addr_t address, byte_t *byte)`:

You will need to implement TLB before implementing these functions and pass the testcases 11~20. These two functions are used to read and write a byte given the process and the virtual address. You don't need to care about the CPU cache in this assignment.  
The behaviour of the two functions is trivial, write to the `address` with the value of `const byte_t *byte`, or read the value from the `address` and copy it to `byte_t *byte`.

- No matter reading or writing, return `ERROR` if the address is not allocated.
- If the address translation is a TLB hit, return `TLB_HIT`
- Otherwise, return `SUCCESS`
- We will also check whether you're reading the right value from memory.

For simplicity, you can safely assume that **every byte you read has been explicitly written before**. So you don't need to care about the uninitialized memory.

### 3.2. TLB rules (implement in file `TLB.h/c`)

You can freely modify the TLB structure as long as it compiles and works as expected. The structure we've provided is just a suggestion.  
The TLB is fully-associative with the replacement policy of LRU. We've provided a `clock` in `struct TLB` and `lut` in `TLB_entry` to help you implement the LRU policy. You can also have your own implementation.  
To get things right, you just need to consider the operations of TLB: read, write, remove, and flush.

#### read

The process is triggered by a memory visit. The TLB will be checked first. If it is a TLB hit, the physical page number will be returned.

#### write (or update)

However, if you experience TLB miss in the "read" process above, there is a need for a page table walk. After the page table walk, the TLB should be updated with the new entry.  
**Note that allocating page does not update any TLB entries!**  
Before you continue reading, make sure you're not messing TLB read/write with memory read/write.

#### remove

When a page is deallocated, the TLB should be removed immediately to prevent illegal visits to the physical page.

#### flush

Flush is trivial in this assignment, it just means invalidate all the TLB entries. You can implement it as a loop that sets all the valid bits to 0.  
Your code should trigger the flush operation **if the context of TLB will change**.  
We call it a context switch if you're trying to **update** the TLB entry from a process that has a different pid compared to the current one. The pid of a process can be obtained by `process->pid`, and the pid of the TLB can be obtained by `tlb->pid`.

## 4. Submission

1. Implement
2. Debug
3. `git commit`
4. `git push`
5. Submit on Gradescope
6. Congratulations! or `goto step_2;`

## 5. Code quality

- Your code with be compiled with `-Wpedantic -Wall -Wextra -Wvla -Werror -std=c11`
- No memory leak is allowed, `Valgrind` will be used to detect memory leaks on runtime.
- It is possible to pass multiple testcases by simply `return SUCCESS`. We will manually check this kind of (or similar) implementation and will give 0 points for it.

---

The following TAs are responsible for this homework:

Suting Chen <chenst AT shanghaitech.edu.cn>  
Lei Jia <jialei2022 AT shanghaitech.edu.cn>  

  

Last modified: 2024-05-24
