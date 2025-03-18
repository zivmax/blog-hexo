---
title: CS100 Homework [7]
date: 2023-05-09 10:04:31
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
- [Unit Test: Inheritance and Operator Overloading](https://cs100.geekpie.club/p/P701?tid=644c0429e03cc2ed71dab92b)
- [Dynamic Array 3.0](https://cs100.geekpie.club/p/42?tid=644c0429e03cc2ed71dab92b)
- [Polynomial](https://cs100.geekpie.club/p/43?tid=644c0429e03cc2ed71dab92b)
- [Expression](https://cs100.geekpie.club/p/44?tid=644c0429e03cc2ed71dab92b)


<!--more-->

---

# CS100 Homework 6 (Spring 2023)
There will be four problems in total: one set of objective questions and three programming problems.

(May 4 22:22) Problem 2 has been added.

(May 9 02:38) Problem 3 has been added.

(May 9 07:12) Problem 4 has been added.

---


## Unit Test: Inheritance and Operator Overloading

This problem is to answer questions.

[👉 Go to GitHub for the HTML page file](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw7_attachments/prob1)

---


## Dynamic Array 3.0

This problem is based on the `Dynarray` you wrote in Homework 6 Problem 2. Before adding anything new to it, your `Dynarray` should meet all the requirements in that problem first.

In this task, your `Dynarray` should support the following new things.

### Subscript operator

The `Dynarray` should support the subscript operator, so that we can use `a[i]` instead of `a.at(i)` to access the i-th element.

Let `a` be an object of type `Dynarray` or `const Dynarray`. The behavior of `a[i]` should be exactly the same as `a.at(i)` , except that the subscript operator does not perform bounds checking. That is, no exception should be thrown if `i >= a.size()`.

### Relational operators

The `Dynarray` should support the six relational operators: `<`, `<=`, `>`, `>=`, `==` and `!=`. These operators perform *lexicographical comparison* of two `Dynarray`.

Lexicographical comparison is an operation with the following properties:

- Two ranges are compared element by element.
- The first mismatching element defines which range is lexicographically *less* or *greater* than the other.
- If one range is a prefix of another, the shorter range is lexicographically *less* than the other.
- If two ranges have equivalent elements and are of the same length, then the ranges are lexicographically *equal*.
- An empty range is lexicographically *less* than any non-empty range.
- Two empty ranges are lexicographically *equal*.

Since we use C++17, you still have to define all six of them. It is often good practice to implement `operator<` and `operator==` first, and define the rest in terms of them.

Note that in homework 8, we will make this `Dynarray` a class template `Dynarray<T>`, and we should always minimize the requirements on unknown types when we do generic programming. Since C++17 does not have compiler-generated comparison operators, we suggest that your implementation **depend only upon the `operator<` and `operator==` of the element type**.

You are free to choose to define them as either members or non-members.

### Output operator

We want to print a `Dynarray` directly using `operator<<`. For example,

```cpp
int arr[] = {1, 2, 3, 5};
Dynarray a(arr, arr + 4);
Dynarray b;
std::cout << a << '\n' << b << std::endl;
```

The output is as follows.

```
[1, 2, 3, 5]
[]
```

In details:
- Elements are separated by a comma ( `,` ) followed by a space.
- The printed content starts with `[` and ends with `']'` . If the dynamic array is empty, just print an empty pair of brackets `[]` .

### OJ tests

There are three subtasks on OJ. Subtask $i$ will be run only if all the testcases of subtask $i-1$ are passed.

Subtask 1 is a compile-time check. Subtask 2 contains all the testcases from Homework 5 Problem 3 and Homework 6 Problem 2. Subtask 3 contains the new testcases specific for this problem.


---

## Polynomial

In this task, you will design and implement a class `Polynomial` representing a real polynomial of the following form:

$$
P(x)=\sum_{i=0}^na_ix^i,\quad a_i,x\in\mathbb R
$$

where $a_0,\cdots,a_n$ are the coefficients and $a_n\neq 0$. The **degree** of a polynomial is the highest degree of the terms with non-zero coefficients, denoted by

$$
\deg(P)=n.
$$

In particular, the following corner cases are defined.

- $a_0x^0=a_0$ even if $x=0$.
- **A polynomial has at least one coefficient.** When $n=0$, the polynomial becomes a constant function $P(x)=a_0$ whose degree is $0$, even if $a_0=0$.

There are a number of operations that can be performed on polynomials, including addition, subtraction, multiplication, etc. To ensure that the coefficient of the highest-order term is always non-zero, we may need a function `adjust` to remove the unnecessary trailing zeros.

```cpp
class Polynomial {
 private:
  // `m_coeffs` stores the coefficients.
  // Note: This is not the unique correct implementation.
  // For example, you may separate the constant term from others,
  // and store the constant term using another variable.
  std::vector<double> m_coeffs;

  static auto isZero(double x) {
    static constexpr auto eps = 1e-8;
    return x < eps && -x < eps;
  }

  // Remove trailing zeros, to ensure that the coefficient of the term with
  // the highest order is non-zero.
  // Note that a polynomial should have at least one term, which is the
  // constant. It should not be removed even if it is zero.
  // If m_coeffs is empty, adjust() should also insert a zero into m_coeffs.
  void adjust() {
    // YOUR CODE HERE
  }

  // Other members ...
};
```

Considering potential floating-point errors, it is suggested to use the function `isZero(x)` to determine whether `x` is zero, which is effectively $|x|<\epsilon$ for some very small $\epsilon>0$.

The coefficients, of course, should be stored in a sequential container, and `std::vector` is a perfect choice.

### Constructors and copy control

The class `Polynomial` should meet the following requirements.

- Default-constructible. A `Polynomial` can be default-initialized to the constant function $P(x)=0$.
- Copyable, movable and destructible. A `Polynomial` should have a copy constructor, a move constructor, a copy assignment operator, a move assignment operator and a destructor. These functions should perform the corresponding operations directly on the member `m_coeffs`. Before starting to define them, think about the following questions: 
  - Will the compiler generate these functions for you if you do not define them?
  - What are the behaviors of the compiler-generated functions? Do they meet the requirements?

  Note: Direct moving of the member `m_coeffs` will possibly make the moved-from polynomial have no coefficients (its `m_coeffs` will possibly be empty), which is not in a valid state if we require all `Polynomial`s to have at least one coefficient. But considering that this is not the concentration of this problem, we only require that the moved-from object can be safely assigned to or destroyed, which means that the compiler-generated move operations suffice. The move operations of `Polynomial` are not required to be `noexcept` here. You are free to choose any reasonable design.

- Constructible from a pair of *InputIterators*. This function has been implemented for you, as long as your `adjust()` is correct.
  
  ```cpp
  template <typename InputIterator>
  Polynomial(InputIterator begin, InputIterator end)
      : m_coeffs(begin, end) { adjust(); }
  ```
  
  This allows the following ways of constructing a `Polynomial`:

  ```cpp
  double a[] = {1, 2, -1, 3.5};
  std::vector b{2.5, 3.3, 0.0};
  std::list c{2.7, 1.828, 3.2}; // not a vector, but have iterators
  std::deque<double> d; // empty
  Polynomial p1(a, a + 4); // 1 + 2x - x^2 + 3.5*x^3
  Polynomial p2(b.begin(), b.end()); // 2.5 + 3.3x, the trailing zero removed
  Polynomial p3(c.begin(), c.end()); // 2.7 + 1.828x + 3.2*x^2
  Polynomial p4(d.begin(), d.end()); // 0
  ```

  If the iterator range `[begin, end)` is empty, i.e. `begin == end`, this constructor initializes the `Polynomial` to be `P(x)=0`.

- Constructible from a `const std::vector<double>`, which contains the coefficients.
  
  ```cpp
  std::vector a{2.5, 3.3, 4.2, 0.0, 0.0};
  std::vector<double> b; // empty
  Polynomial p1(a); // 2.5 + 3.3x + 4.2*x^2.
  Polynomial p2(b); // 0
  Polynomial p3(std::move(a)); // Move it, instead of copying it.
  ```

  Note that if the argument passed in is a non-`const` rvalue, it should be moved instead of being copied. Moreover, implicit conversion from a `std::vector<double>` to `Polynomial` through this constructor should be disabled.

### Basic operations

Let `p` be of type `Polynomial` and `cp` be of type `const Polynomial`. The following operations should be supported.

- Both `p.deg()` and `cp.deg()` return the degree of the polynomial. Any reasonable integral return type is allowed, but it should not be too small.
- For an integer `i`, both `p[i]` and `cp[i]` return the coefficient of the term $x^i$ as a read-only number. Any reasonable return type is allowed. The behavior is undefined if `i` is not in the valid range `[0, deg()]`.
- For an integer `i` and a real number `c`, `p.setCoeff(i, c)` sets the coefficient of the term $x^i$ to `c`. The behavior is undefined if `i < 0`. If `i > deg()`, it has the same effect as adding a monomial $cx^i$ to it, which means that after this operation,
  - `p.deg()` becomes `i`.
  - `p[j]` is zero for every `j` in `(d, i)`, where `d` is the degree of `p` before this operation.
  
  Note that allowing the user to modify the coefficients may result in trailing zeros, which your code must handle correctly. This is why we don't allow `p[i]` to return the non-`const` reference to the `i`-th coefficient.

  `std::vector<T>::resize` may help you.
- `operator==` and `operator!=` should be defined. Two polynomials `a` and `b` are equal if and only if `a - b` is the zero constant function $P(x)=0$. Equivalently, `a == b` if and only if
  - `a.deg() == b.deg()`, and
  - `isZero(a[i] - b[i])` holds for every integer `i` in `[0, a.deg()]`.
- For a number `x0` (suppose it is a `double`), both `p(x0)` and `cp(x0)` return the value of the polynomial at $x=x_0$, i.e. $P\left(x_0\right)$.

### Arithmetic operations

The arithmetic operators (as well as the corresponding compound assignment operators) that need to be supported are as follows.

- `-a` (the unary negation operator), `a + b`, `a += b`, `a - b`, `a -= b`.
- `a * b`, `a *= b`.

### Derivatives and integrals

For a polynomial `p`,
- `p.derivative()` should return the derivative of `p`, i.e. $\displaystyle\frac{\mathrm d p(x)}{\mathrm dx}$. Note that this should still be a polynomial.
- `p.integral()` should return the integral of `p`, defined as
  
  $$
  \int_0^xp(t)\mathrm dt.
  $$
  
  Note that this should still be a polynomial.

### Notes

For each member function, should it be `const`, or non-`const`, or having the "`const` vs non-`const` overloads"? Think about these questions carefully before coding.

Regarding the floating-point errors: we will not test this on purpose, but such problems might show up accidentally and cause unexpected results. To avoid such risks, it is suggested to always use `isZero(x)` and `isZero(a - b)`, instead of directly comparing floating-point numbers.

### Compile-time requirements test

We have provided a compile-time test `compile_test.cpp` for you.

### Attachments

[👉 Go to GitHub](https://github.com/zivmax/CS100-homework-attachments/tree/main/hw7_attachments/prob3)



---

## Expression

In recitations, we showed an example of designing a class hierarchy for different kinds of nodes in an **abstract syntax tree** (AST). In this problem, we will support more functionalities:

- A new kind of node called `Variable`, which represents the varaible `x`. With this supported, our AST can be used to represent unary functions, instead of only arithmetic expressions with constants.
- Evaluation of the function at a certain point $x=x_0$.
- Evaluation of the derivative of the function at a certain point $x=x_0$.

The operations that need to be supported are declared in this base class:

```cpp
class NodeBase {
 public:
  // Make any of these functions virtual, or pure virtual, if necessary.
  NodeBase() = default;
  double eval(double x) const; // Evaluate f(x)
  double derivative(double x) const; // Evaluate df(x)/dx
  std::string rep() const; // Returns the parenthesized representation of the function.
  ~NodeBase() = default;
};
```

In recitations, we used only one class `BinaryOperator` to represent four different kinds of binary operators. Such design may not be convenient for this problem, because the ways of evaluating the function and its derivative differ to a greater extent between different operators. Therefore, we separate them into four classes:

```cpp
class BinaryOperator : public NodeBase {
 protected:
  Expr m_left;
  Expr m_right;
 public:
  BinaryOperator(const Expr &left, const Expr &right)
      : m_left{left}, m_right{right} {}
};

class PlusOp : public BinaryOperator {
  using BinaryOperator::BinaryOperator;
  // Add functions if necessary.
};

class MinusOp : public BinaryOperator {
  using BinaryOperator::BinaryOperator;
  // Add functions if necessary.
};

class MultiplyOp : public BinaryOperator {
  using BinaryOperator::BinaryOperator;
  // Add functions if necessary.
};

class DivideOp : public BinaryOperator {
  using BinaryOperator::BinaryOperator;
  // Add functions if necessary.
};
```

By declaring `using BinaryOperator::BinaryOperator;`, we are **inheriting** the constructor from `BinaryOperator`. For example, the class `PlusOp` now has a constructor like this:

```cpp
class PlusOp : public BinaryOperator {
 public:
  PlusOp(const Expr &left, const Expr &right) : BinaryOperator(left, right) {}
};
```

and the same for `MinusOp`, `MultiplyOp` and `DivideOp`.

To support the variable `x`, we define a new class `Variable`:

```cpp
class Variable : public NodeBase {
  // Add functions if necessary.
};
```

Then we define a `static` member of `Expr` as follows.

```cpp
class Expr {
  std::shared_ptr<NodeBase> m_ptr;
  Expr(std::shared_ptr<NodeBase> ptr) : m_ptr{ptr} {}

 public:
  Expr(double value);

  static const Expr x;
  // Other members ...
};

// After the definition of `Variable`:
const Expr Expr::x{std::make_shared<Variable>()};
```

Note that `Expr` also has a non-`explicit` constructor that receives a `double` and creates a `Constant` node:

```cpp
Expr::Expr(double value) : m_ptr{std::make_shared<Constant>(value)} {}
```

With all of these, and the overloaded arithmetic operators defined, we can create functions in a very convenient manner:

```cpp
auto &x = Expr::x;
auto f = x * x + 2 * x + 1; // x^2 + 2x + 1
std::cout << f.rep() << std::endl; // (((x) * (x)) + ((2.000000) * (x))) + (1.000000)
std::cout << f.eval(3) << std::endl; // 16
std::cout << f.derivative(3) << std::endl; // 8
auto g = f / (-x * x + x - 1); // (x^2 + 2x + 1)/(-x^2 + x - 1)
std::cout << g.eval(3) << std::endl; // -2.28571
std::cout << g.derivative(3) << std::endl; // 0.489796
```

### Requirements for `rep()`

Note: This is a very simple and naive way to convert an expression to a string, because we don't want to set barriers on this part. You can search for some algorithms to make the expression as simplified as possible, but it may not pass the OJ tests.

Any operand of the unary operators ( `+`, `-` ) or the binary operators ( `+`, `-`, `*`, `/` ) should be surrounded by a pair of parentheses. Correct examples:
- $2+3$: `(2.000000) + (3.000000)`
- $2x+3$: `((2.000000) * (x)) + (3.000000)`
- $2\cdot(-x)+3$: `((2.000000) * (-(x))) + (3.000000)`

There should be a space before and after each binary operator. For example, `(expr1) + (expr2)` instead of `(expr1)+(expr2)`.

To convert a floating-point number to `std::string`, just use `std::to_string` and you will be free of troubles caused by precisions and floating-point errors.

Note: If the floating-point value of a `Constant` node is negative, it is **not** treated as a unary minus sign applied to its absolute value. For example,

```cpp
Expr e(-2);
std::cout << e.rep() << std::endl;
```

The output should be `-2.000000` instead of `-(2.000000)`.

`example.cpp` contains these simple tests.

### Requirements for `Expr`

From the user's perspective, only `Expr` and its arithmetic operators are **interfaces**. Anything else in your code is **implementation details**, which user code will not rely on. In other words, you are free to implement these things in any way, as long as the interfaces of `Expr` meet our requirements.

- `Expr` is copy-constructible, copy-assignable, move-constructible, move-assignable and destructible. These operations should just perform the corresponding operations on the member `m_ptr`, and let the corresponding function of `std::shared_ptr` handle everything. The move operations should be `noexcept`.
- `Expr` is constructible from a `double` value, and this constructor is non-`explicit`.
- Let `e` be `const Expr` and `x0` be `double`, then the subtree rooted at `e` represents a function. `e.eval(x0)` returns the value of this function at $x=x_0$. `e.derivative(x0)` returns the value of the derivative of this function at $x=x_0$. `e.rep()` returns a `std::string` representing this function.
- Let `e1` and `e2` be two objects of type `const Expr`. The following expressions return an object of type `Expr`, which creates a new node corresponding to the operators.
  - `-e1`
  - `+e1`
  - `e1 + e2`
  - `e1 - e2`
  - `e1 * e2`
  - `e1 / e2`
- `Expr::x` is an object of type `const Expr`, which represents the variable `x`.

Use `compile_test.cpp` to check whether your code compiles.

### Submission

Submit `expr.hpp` or its contents to OJ.

### Thinking

Why do we use `std::shared_ptr` instead of `std::unique_ptr`?

Can you add support for more functionalities, e.g. the elementary functions $\sin(e)$, $\cos(e)$, exponential expressions $e_1^{e_2}$, ...? More variables? Print expressions to $\LaTeX$?


### Attachments

[👉 Go to GitHub](https://github.com/zivmax/CS100-homework-attachments/tree/main/hw7_attachments/prob4)

