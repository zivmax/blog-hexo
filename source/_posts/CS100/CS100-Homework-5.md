---
title: CS100 Homework [5]
date: 2023-04-16 09:16:11
mathjax: true
tags:
  - C 语言
  - CS100
  - 上科大
  - 计算机科学
category:
  - 学习
  - 作业
---

**Problems**:
- [Checklist for C++ Beginners](https://cs100.geekpie.club/p/P501?tid=6425c7c3e03cc2ed71cc32e0)
- [Playing with Strings and Vectors](https://cs100.geekpie.club/p/34?tid=6425c7c3e03cc2ed71cc32e0)
- [Dynamic Array](https://cs100.geekpie.club/p/33?tid=6425c7c3e03cc2ed71cc32e0)


<!--more-->

---

# CS100 Homework 5 (Spring 2023)

Download the materials for problem 3 from Piazza.

(April 6) 20:20 Problem 3 and bonus have been updated. All submissions have been rejudged. In case we made any mistakes during this procedure, please re-submit your code. We are sorry for causing any inconvenience.

Please note that we do not allow the use of standard library containers in any form. Direct use of standard library containers will result in compilation error. If you managed to escape our compile-time check, you will still get zero points. We will have our ways to check this.

(April 7) 9:57 Problem 2 has been released. Deadline has been postponed to April 13, 23:59.

(April 8) 14:25 Problem 3 and bonus: Accepted submissions have been rejudged. See Piazza for details.

(April 10) 0:59 Problem 2 bonus added. See Piazza for details.

(April 13) 23:27 In Problem 2, split(str, sep) should return a vector containing one empty string, instead of an empty vector, if str == "". Deadline is postponed to April 14, 23:59. Sorry for not making this clear.

---

## Checklist for C++ Beginners
This problem is to answer questions.

[👉 Go to GitHub for the HTML page file](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw5_attachments/prob1)


---

## Playing with Strings and Vectors

### Implement the following four functions.

```cpp
std::string strip(const std::string &str);
std::string join(const std::string &sep, const std::vector<std::string> &strings);
std::vector<std::string> split(const std::string &str, const std::string &sep);
std::string swapcase(std::string str);
```

- `strip(str)`

  Returns the string obtained from str by removing all the leading and trailing whitespaces. A character c is a whitespace if std::isspace(c) is true. For example,

  ```cpp
  assert(strip(" wefafwefw \n") == "wefafwefw");
  ```

  The assertion above should succeed.

- `join(sep, strings)`

  Concatenate the strings strings, during which the string sep is inserted in between each given string. Then return the result. For example:

  ```cpp
  std::vector<std::string> strings = {"hello", "world", "cxx23"};
  assert(join(", ", strings) == "hello, world, cxx23");
  ```

  The assertion above should succeed.

  If the vector `strings` is empty, return an empty string.

- `split(str, sep)`

  Using sep as the delimiter, split the string str into a vector of strings and return that vector. For example,

  ```cpp
  std::vector<std::string> ans{"", "aaa", "", "bbb", "cdefg"};
  assert(split("xaaaxxbbbxcdefg", "x") == ans);
  ans = {"", "x"};
  assert(split("xxx", "xx") == ans);
  ```

  The assertions above should succeed. It is guaranteed that `sep` is not an empty string.

- `swapcase(str)`

  Returns the string obtained from `str` with cases swapped, i.e. lowercase letters are changed to their uppercase forms, and uppercase letters are changed to their lowercase forms. Other characters remain unchanged. For example, `swapcase("123..abcDEF")` is equal to `"123..ABCdef"`.

### Submission

Submit your code containing these four functions to OJ.


---

## Dynamic Array

Implement your own version of the dynamic array `Dynarray` class. This is almost the same as the example in Lecture 17.

### Expected Usage

A `Dynarray` represents an "array" of ints, the length of which is determined at run-time and the elements are stored in dynamically allocated memory. A `Dynarray` should manage the memory it uses correctly: It should allocate adequate memory when initialized, and deallocate the memory when it is destroyed to avoid memory leaks.

#### Initialization

Let `n` be a non-negative integer and `x` be an integer. Let `begin` and `end` be of type `const int *` satisfying `begin <= end`, with `begin` pointing at the first element of some array and `end` pointing at **the element after** the last element of that array. A `Dynarray` can be constructed in the following five possible ways:

- ```cpp
  Dynarray a;
  ```
  Initializes the object `a` to be an empty "array" whose length is `0`. (Default-initialziation)
- ```cpp
  Dynarray a(n);
  ```
  Initializes the object `a` to be an "array" of length `n`, all elements **value-initialized**.

  **Note**: A constructor with one parameter also defines a **type conversion**. For example, the `std::string` class has a constructor that accepts one argument of type `const char *`, so the implicit conversion from C-style strings to C++ strings is supported. However, we definitely don't want to see the abuse of such constructors:
  ```cpp
  Dynarray a = 42;
  ```
  or
  ```cpp
  void fun(Dynarray a);
  fun(42); // constructs Dynarray(42) and passes it to `a`
  ```
  In order to forbid the use of this constructor as an implicit type conversion, you should add the `explicit` keyword:
  ```cpp
  class Dynarray {
    // ...
    explicit Dynarray(std::size_t) /* ... */
  };
  ```

- ```cpp
  Dynarray a(n, x);
  ```
  Initializes the object `a` to be an "array" of length `n`, all elements initialized to be `x`.
- ```cpp
  Dynarray a(begin, end);
  ```
  Initializes the object `a` to be an "array" of length `end - begin`. The elements are obtained from the range `[begin, end)`. For example,
  ```cpp
  const int arr[10] = {19, 64, 10, 16, 67, 6, 17, 86, 7, 29};
  Dynarray a(arr + 3, arr + 7); // a contains the elements {16, 67, 6, 17}
  ```
- ```cpp
  Dynarray b = a;
  ```
  Copy-initialization. See below.

#### Copy control

Let `a` be some existing `Dynarray` object. The following are equivalent ways of initializing a new `Dynarray` object:

```cpp
Dynarray b = a;
Dynarray b(a);
Dynarray b{a};
```

This is the copy-initialization from `a`. Our `Dynarray` adopts the **value semantics**: Such copy-initialization should allocate another block of memory for `b` and copy the elements from `a`. After that, the data (elements) owned by `a` and `b` should be independent: Modification to some element in `a` should not influence the elements in `b`.

If `b` is also an existing `Dynarray` object, the following assignment can be performed:

```cpp
b = a;
```

This is the copy-assignment from `a`. After that, `b` should be a copy of `a`. Any modification to an element of `a` should not influence the elements in `b`.

Your copy-assignment operator must be self-assignment safe.

#### Basic information

Let `a` be an object of type `const Dynarray`. The following operations should be supported:

- ```cpp
  a.size()
  ```
  Returns the length of the "array", that is, the number of elements in `a`. It should be of type `std::size_t`, which is defined in `<cstddef>`.
- ```cpp
  a.empty()
  ```
  Returns a `bool` value indicating whether the `Dynarray` is empty or not. A `Dynarray` is said to be *empty* if its length is zero.

#### Element access

Let `a` be an object of type `Dynarray` and `ca` be an object of type `const Dynarray`. Let `n` be a non-negative integer. The following operations should be supported:

- ```cpp
  a.at(n)
  ```
  Returns a **reference** to the element indexed `n` in `a`. It is both readable and modifiable since `a` is not `const`. For example:
  ```cpp
  a.at(n) = 42;
  std::cout << a.at(n) << std::endl;
  ```
- ```cpp
  ca.at(n)
  ```
  Returns a **reference-to-`const`** to the element indexed `n` in `ca`. It should be read-only, since `ca` is `const`. For example:
  ```cpp
  std::cout << ca.at(n) << std::endl; // OK
  ca.at(n) = 42; // This should lead to a compile-error.
  ```

Moreover, to keep in consistent with the behaviors of the standard library containers, `Dynarray::at` should do bounds-checking. If `n` is not in the range `[0, a.size())`, you need to **throw an exception `std::out_of_range`**. To throw this exception, write
```cpp
throw std::out_of_range{"Dynarray index out of range!"};
```
The exception class `std::out_of_range` is defined in the standard library file `<stdexcept>`.

### Examples

Write your code in `dynarray.hpp`. We have provided a template for you to begin with, although it contains only some preprocessor directives.

We have also provided a `compile_test.cpp` for you which contains the compile-time checks of your implementation. Members missing, `const` qualifier missing, or incorrect return types will be detected. **You can also try `concept_test.cpp`, which uses C++20 concepts and is more readable with less magic code.**

Here is a sample usage of the `Dynarray`:

```cpp
#include "dynarray.hpp"
#include <algorithm>
#include <iostream>

void reverse(Dynarray &a) {
  for (int i = 0, j = a.size() - 1; i < j; ++i, --j)
    std::swap(a.at(i), a.at(j));
}

void print(const Dynarray &a) {
  std::cout << '[';
  if (!a.empty()) {
    for (std::size_t i = 0; i + 1 < a.size(); ++i)
      std::cout << a.at(i) << ", ";
    std::cout << a.at(a.size() - 1);
  }
  std::cout << ']' << std::endl;
}

int main() {
  int n;
  std::cin >> n;
  Dynarray arr(n);
  for (int i = 0; i != n; ++i)
    std::cin >> arr.at(i);
  reverse(arr);
  print(arr);
  Dynarray copy = arr;
  copy.at(0) = 42;
  std::cout << arr.at(0) << '\n'
            << copy.at(0) << std::endl;
  return 0;
}
```

Input:

```
5
1 2 3 4 5
```

Output:

```
[5, 4, 3, 2, 1]
5
42
```

### Submission

Submit the contents of your `dynarray.hpp` to the OJ.

### Self-test on OJ

Input an integer $i\in\{0,1,2,3,4,5\}$ to run the $i$-th testcase.

### Bonus

If your copy-assignment operator is **exception-safe**, or more specifically, offers [**strong exception safety guarantee**](https://en.cppreference.com/w/cpp/language/exceptions#Exception_safety), you can get the extra 10 points. Submit it to Problem 504 on OJ.

Related materials: *C++ Primer* (5th edition) section 13.2.1, section 13.3; *Effective C++* (3rd edition) Item 29.
