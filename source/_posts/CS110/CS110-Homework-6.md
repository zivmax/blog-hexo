---
title: CS110 Homework [6]
mathjax: false
date: 2024-05-18 11:21:15
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 作业
---

***HW 4, HW 5 are handwritten.***

You can get the template code [here](https://classroom.github.com/a/OlfGnHG5)

<!--more-->

# Homework 6: Multi-level Cache Simulator

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/) [ShanghaiTech University](http://www.shanghaitech.edu.cn/)

  

## Introduction

Exploiting locality in the memory access with data cache is one of the key idea in CS110 Computer Architecture. In this homework, you are going to implement a two-level cache simulator. 

## File structure

You should see files below in your starter code.

- main.c includes an exemplary dot-product computation program that employs the cache simulator. The implementation of environmental interfaces is also implemented in this file.
- cache.c includes the implementation of the cache access functions, which should be currently empty. Feel free to add your own functions here.
- cache.h includes declaratons of:
    - the simulated cache structure
    - functions used to interact with cache, like read, write, etc.
    - some other helper functions

## Specification

### Structure Specification

We describe the meaning of some selected variables inside main structs here.

- struct cache_config is used for initialize cache.
    - address_bits indicates the number of bits in the address
    - line_size indicates the cache line size in byte, guaranteed to be power of 2
    - lines indicates the number of cache lines in the cache, guaranteed to be power of 2
    - ways indicates how many lines within a set, i.e. associativity, guaranteed to be power of 2
    - write_back indicates whether the cache will follow write back policy. Otherwise, the cache will follow write through policy.
- struct cache_line is the simulated cache line.
    - last_access is used for LRU replacement.
    - data is the array that stores data in each cache line. You should initialize it.
- struct cache is the simulated cache, which is the main struct.
    - xx_bits represents the number of bits in this field
    - xx_mask is used to extract corresponding field. For example, you can get index of an address by (addr & index_mask) >> offset_bits
    - lines is the array of cache lines. You should initialize it.
    - lower_cache is the pointer to L2 cache. It should be NULL in L2 cache.

### Functions Specification

We describe the usage, parameters and returns for some functions here.

- cache_create : create a cache simulator(a struct cache variable)
    - parameter:
        - struct cache_config config sets of parameters this cache should follow
        - struct cache * lower_cache pointer to the lower level cache. If the cache is the last level cache, this parameter will be NULL
    - returns: pointer to a struct cache variable. You should return NULL if memory allocation failed.
- cache_destroy : destroy a cache simulator and free all dynamic allocated memroy
    - parameter:
        - struct cache * cache pointer to the cache simulator you are going to destroyed.
    - note: You should write the modified cache line back to memory. See comments in the initial code for the required eviction order.
- cache_read_byte : read one byte of data at a specific address
    - parameter:
        - struct cache* cache cache simulator to read from
        - uint32_t addr address to read
        - uint8_t * byte pointer to space to store the result
    - returns: true on cache hit, false on cache miss.
    - note: Whether hit or miss is determined by L1 cache only.
- cache_write_byte : write one byte of data at a specific address
    - parameter:
        - struct cache* cache cache simulator to write from
        - uint32_t addr address to write
        - uint8_t byte the data to be written
    - returns: true on cache hit, false on cache miss.
    - note: Whether hit or miss is determined by L1 cache only.
- mem_store is implemented for you and is used to store data into memory.
    - parameter:
    
    - uint8_t * src pointer to source data to be stored
    - uint32_t addr address to store
    - uint32_t count size of data
    
- mem_load is implemented for you and is used to load data from memory.
    - parameter:
    
    - uint8_t * dst pointer to space to store the loaded data
    - uint32_t addr address to load
    - uint32_t count size of data
    
- get_timestamp is implemented for you and will return current timestamp.

## Task Description

You are required to implement a 2-level cache in this homework. To simplify the task, we divide it into 2 parts

### Part 1: single-level cache

In this part, you are going to implement a cache simultor with only a L1 cache. Your cache simulator should:

- has configurable associative, cache line size, total cache data capacity
- works correctly with both write through and write back policy
- employs LRU replacement policy

You are required to follow some rules:

- last_access should be updated at every cache read/write, no matter hit or miss. You should call get_timestamp to get the system time.
- For write-through policy, any modification should reach memory after the access is triggered. While for write-back policy, modicication will only reach memory when a cache line is evicted from the cache.
- When finding a place for a new cache line, first try to find an invalid cache line. You should find the first invalid slot in a set. For example, if slot0 and slot3 are all empty, you should use slot0 first. If the set is full, apply LRU to evict one cache line
- If there occurs a cache miss while writing, you should first read the corresponding cache line from memory and then finish the writting. NOTE THAT you should return a miss for a writing miss, not just writing down the data and return a hit.
- The granularity between cache and memory should be one cache line, i.e. you should write/read a whole cache line to/from the memory

To simplify your task and the grading, we provide a flow chart here to show the working process of write-back and write-through cache. Please strictly follow this process while implementing your simultor.

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/homework/hw6-%E7%BD%91%E9%A1%B5/write-back.png) ![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/homework/hw6-%E7%BD%91%E9%A1%B5/write-through.png)

### Part 2: two-level cache

In part2, you are going to extend your simulator to a 2-level cache simulator. Each cache in a 2-level cache system should behave individually and follows the rules mentioned in part1.

Except those rules in part1, there are some additional rules for this part:

- Whether hit or miss for function cache_read_byte or cache_write_btye should be determined by the status of L1 cache in this part. For example, if L1 cache miss and L2 cache hit, you should return false.
- The granularity between L1 and L2 cache should be one cache line. Thus, L1 cache should evict/load one cache line to/from L2 cache at one time. Please do not call multiply times of cache_read_byte/cache_write_byte. Instead, implementing new functions cache_read_cachelin/cache_write_cacheline for L2 cache may be better.
- L1 cache should not access the memory directly. Only L2 cache can access the memory
- Since L1 and L2 cache can differ in the cache configuration, you should take care of the tag field while transfer cache lines bewteen the two caches.
- When a replacement in L1 cache occurs, make sure you first evcit the victim cache line to L2 cache, then read the needed cache line from L2 cache. THIS IS IMPORTANT FOR GRADING

## Test

We provide a simple test dot_test in main.c. You can refer to it for how we test your simulator. The final test cases will be in the similar form. You are also highly recommend to write your own test cases by converting conventional applications into those using cache simulator interfaces. we will test whether you implement write through policy correctly by directly checking the memory

## Submission

You should submit your code via Github. Please follow the guidance in Gradescope to submit your codes on Github.

---

In Homework 6 are,

Chundong Wang <`wangchd` AT `shanghaitech.edu.cn`>  
Siting Liu <`liust` AT `shanghaitech.edu.cn`>  

and,

Linjie Ma <`malj` AT `shanghaitech.edu.cn`>  
Qing Xu <`xuqing2` AT `shanghaitech.edu.cn`>  

  
Last modified: 2024-05-12