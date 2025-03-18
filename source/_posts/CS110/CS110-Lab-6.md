---
title: CS110 Lab [6]
mathjax: false
date: 2024-04-25 9:05:51
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

- Learning advanced techniques to help you create more concise circuits within Logisim.

- Feel free to do each part as separate sub-circuits in the same Logisim file.

- Strengthening experience in designing circuits using Logisim.

<!--more-->

# Lab 6

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) @ [ShanghaiTech University](https://www.shanghaitech.edu.cn/)


## Advanced Logisim

Download the [files](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/lab6_starter.zip) for Lab 6 first.

Here are three Logisim features that should both save you a lot of time and make your circuits look much cleaner.

#### 1. Splitters

Splitters allow you to take a multi-bit value and split it up into smaller parts, or (despite the name) combine multiple values that are one or more bits into a single value.  
Here, we split the 4-bit binary number `0111` into `01` and `11`, then recombine it with `10` into the final 6-bit number `100111`:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/splitters.png)

Click on a splitter to get its menu in the sidebar. This menu determine the number of arms on your splitter and how many bits should go on each arm. For the circuit above, the left splitter's menu looks like this:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/splitters_left.png)

While the right splitter's menu looks like this:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/splitters_right.png)

**Notice that there's an option called** `facing`. You can use this to rotate your splitter. Above, see that the splitter on the right is facing West while the splitter on the left is facing East.

If you see an error wire that is orange, this means that your bit width in does not match your bit width out. Make sure that if you're connecting two components with a wire, you correctly set the bit width in that component's menu.

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/splitters_error.png)

#### 2. Tunnels

A tunnel allows you draw an "invisible wire" to bind two points together. Tunnels are grouped by case-sensitive labels give to a wire. They are used to connect wires like so:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Tunnels.png)

Some care should be taken as to which wires are connected with tunnels to which other wires, such as in this case:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Tunnels_error.png)

We _strongly_ recommend you use tunnels with Logisim, because they make your circuits much cleaner looking, and therefore easier to debug.

#### 3. Extenders

When changing the width of a wire, you should use a bit extender for clarity. For example, consider the following implementation of extending an 8-bit wire into a 16-bit wire:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Extenders.png)

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Extenders_0.PNG)

Compared to the splitter, the extender is easier to understand at a glance. This becomes especially helpful when working with complex circuits.

Additionally, consider the case of throwing out bits. Despite its name, an extender can also perform this operation:

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Extenders_1.PNG)

![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/Extenders_2.PNG)

## Exercises

### Exercise 1: Practice with Advanced features

In this part of the lab, we will construct a circuit that manipulates an 8-bit number.

#### ACTION ITEM:

Complete the following steps to create the splitter circuit, and show this to your TA (remember to save). When you've completed the circuit, answer the question in the checkoff session.

1. Open up the Exercise 1 schematic (`File->Open->ex1.circ`) and go to the empty Split circuit.
2. Go to the `Wiring` folder and select the `Splitter` circuit. This circuit will take a wire and split it into a set of wires of smaller width. Conversely, it can also take many sets of wires and combine them into one.
    
3. Change the `Bit Width In` property (bus width) to `8`, and `Fan Out` property to `8`. Connect the inputs to tunnels
    
    ![](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab6/practice1.PNG)
    
4. Now, judge the number of '1s' in the input whether odd. You can use `XOR` gates to eath bit of input and route the output to `OUT1`.
    
5. Then, judge the number of '1s' in the input whether greater than '0s'. You can use `Bit Adder and Comparator` from `Arithmetic` to implement this circuit and route the output to `OUT2` .
    
6. We need to add a parity bit to the input to ensure that output has odd number of '1s', you are not allowed to use parity circuits from logisim `Gates`. Place another splitter with the proper properties to combine the parity bit and the input to `OUT3`.
    
7. We consider the `OUT2` is the sign bit of input, changing input into 2's complement format. Place another splitter with the proper properties to combine the sign bit and the 2's complement to `OUT4`.
    

**Hint 1**: An unsigned comparator is required in `OUT2`.

**Hint 2**: The parity bit can be obtained from `OUT1` because when input has odd bit '1s', the parity bit will be 0, otherwise, the parity bit will be 1.

**Hint 3**: 2's complement transform is different according to sign bit.

- Show your `ex1.circ` to your TA.
- When the output from `OUT4` is negative and we want to compute its two's complement once more, what would the outcome be? **Hint:** In fact, this is a question about finding the two's complement of a two's complement. Just share your findings with TA.

### Exercise 2: Rotate Right

With your knowledge and experience of splitters and multiplexers, you are ready to implement a non-trivial combinational logic block: `rotr`, which stands for "Rotate Right". The idea is that `rotr A,B` will "rotate" the bit pattern of input `A` to the right by `B` bits. So, if `A` were `0b1011010101110011` and `B` were `0b0101` (`5` in decimal), the output of the block would be `0b1001110110101011`. Notice that the rightmost `5` bits were rotated off the right end of the value and back onto the left end. In RTL, the operation would be something like `R = A >> B | A << (16 - B)`.

#### ACTION ITEM:

Implement a subcircuit named `rotr` with the following inputs. Show the final circuit to your TA (remember to save!).

- `A` (16-bit), the 16-bit input to be rotated
- `B` (4-bit), the rotation amount (why 4 bits?) You can find the starter subcircuit in `ex2.circ`.

The output should be `A` rotated right by `B` bit positions, as outlined above. You are **NOT** allowed to use Logisim shifters in your solution, though all other combinational logic (MUXes, constants, gates, adders, etc.) is allowed. Logisim's built-in MUXes (find them under the `Plexers` menu) might be especialy helpful. Your solution shouldn't involve a clock or any clocked elements, like registers.

**Hint 1**: Before you start wiring, you should think very carefully about how you might decompose this problem into smaller ones and join them together. You should feel very free to use subcircuits when implementing `rot4` and `rot8` as well as `rotr`.

**Hint 2**: Just because we gave you an RTL representation doesn't mean it's the best way to look at this problem. Think about the input bits of `B` and think about how to effectively use splitters! Can you do something with the binary form? Remember why binary is good for use in computers: a `1` is easy to represent as an `ON` signal, and a `0` is easy to represent as an `OFF` signal. Let's say we want to rotate `11` times. `11` is `1011` in binary, or `1*8 + 0*4 + 1*2 + 1*1`. Can you use this to make a cleaner circuit? Making use of the `rot*`circuits we have provided is a good idea that will keep things clean!

**Hint 3**: Perhaps you also need to create other sub-circuits and call them in the `rotr` circuit. Please explore how to customize the appearance of your components! It's important to note that after designing their appearance, they also need to be saved immediately and their `Appearance` attribute should be changed to `Custom`.

- Show your TA your `rotr` circuit and verify that it works.

### Exercise 3: Runing LED

From Exercise 2, we obtained a combinational circuits which can rotate the input with different values. In this part of lab, we will implement a version for sequential circuits. For each clock, the input will rotate with step of 1 and the rotated value will be input next time. For example, if `input` is `0b10000000` and we set the rotate step of `1`. At clock 2, `output` is `0b01000000`, at clock 3, output is `0b00100000`...

#### ACTION ITEM:

Complete the following steps and show this completed circuit to your TA (remember to save!)

1. Open up the Exercise 3 schematic (`File->Open->ex3.circ`) and fill in the `rot1`, you can use the circuits in Exercise 2 and revise the width of input and output.

2. Using the `Register` to store the previous value, and then using a `rot1` to rotate the value, the output is `OUT_LED`.

3. Using a `Multiplexer` to set the initinal value `INITIAL_LED` of `Register` when reset signal `RST` is 1. When `RST` is 0, the input of Multiplexer will be `OUT_LED`.

- Show your TA your `Runing LED` circuit and verify that it works.
- If we want to control the LED to shift for a certain number of times, such that it stops after moving 8 times, remaining stationary until the next change in the `RST` signal occurs. What changes should we make to the circuit? Show your TA your revised circuit drawing or create a new Logisim file containing your revised circuit.

### Testing

Debugging circuits can be done in two ways. One method involves directly using the poke tool to alter component values and observe the output instantly. The second approach is to use the testing scripts provided by us via:

./test.sh

Since Logisim will be running in one terminal window already, make sure to open up a new window to run the testing script.

If it says you don't have permission to `test.sh`, run the following code:

chmod +x test.sh

If it says you don't have permission to `test.py`, run the following code **in** the `./testing` folder:

chmod +x test.py

---

The following TA(s) are responsible for this lab:

Zhaojun Ni <nizhj2022 AT shanghaitech.edu.cn>