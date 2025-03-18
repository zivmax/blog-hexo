---
title: CS110 Project 3
mathjax: false
description: Flappy-bird game on Longan Nano. (Individual Project)
date: 2024-05-30 11:01:25
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 项目
---

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) | [ShanghaiTech University](https://www.shanghaitech.edu.cn/)

# CS110 Project 3 (24s): Flappy-bird game on Longan Nano

[Reference Implementation](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p3/index.html#)

As CS110 is coming to an end, it's time to put the skills we've learned into practice.

In this project, you will code in C language and RISC-V assembly to implement a Flappy-bird game. You will also use `platformIO` to cross-compile and generate a program for the Longan Nano development board.

While we do have a reference implementation, you are not required to fully replicate it. Once you have completed the basic requirements, you are free to add or modify more features according to your preferences.

We hope you enjoy playing with Longan Nano in this project.

**Important Note 1**: Project 3 this year is an individual project - one person, no teammate.

**Important Note 2**: We only score Flappy Bird game. You may implement your own game for fun, but they are not scored.

**Important Note 3**: Grading of Project 3 is manually done by TAs during Lab 15 check procedure, instead of using Autograder. See [Grading Policy](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p3/index.html#grading-policy) for details.

## Academic Integrity Policy

Project 3 this year is an individual project - one person, no teammate.

We will conduct code similarity check. If your code (except for Lab 12 template) is too similar to code from others, prepare to face the penalty from entire TA group and two professors. See slide from the 1st lecture for all possible consequences.

## Grading Policy

1. The overall score for project 3 is 17 points. Note that **if your code does not compile or assemble, you will get 0 points for project 3**. However, the good news is that in project 3, there will be **no** memory leak check and **no** additional compiler flags like `-Wall` and `-Werror`.
2. The grading rubrics sum up to 19 points, which is more than 17. You will get a final score of 17 even if your score exceeds 17. If you are more than confident with the stability and robustness of your implementation, it is explicitly allowed to ignore some of the tasks. However, it is still strongly recommended that you implement everything that is within your reach.
3. Since checking procedure of Project 3 is complicated, it might be impossible to check all the ≈ 25 students of one Lab within your Lab's time slot. Therefore, expect Lab 15 to last a long time ( ≈ 3 hours).
4. Grading of Project 3 is manually done by TAs during check procedure, instead of using Autograder.
5. Location and time slot for Project 3 Check: Same as your Lab location.
6. Project 3 check procedure after DDL: On student's computer, fetch project 3 source code from Gradescope, then compile the newly fetched source code, and download the firmware into device.
7. All requirements other than _RISCV Code_ has an all-or-nothing grading scheme, for example, you will only get 0 or 2 points for player part of _Moving Entities (Sprites)_.

## Implementation and Submission Policy

Project 3 has no fixed template, but no fixed template means less constraint. You will implement project 3 from the ground up. Starting from the Lab 12 template (`lab12-starter-20240515.tar.gz`), you are free to:

- Change any code in `lab12-starter-20240515.tar.gz`. In fact, our reference implementation modifies the LCD library.
- Add and remove `.c` or `.h` files.

The submission must at least include:

- Anything in Lab 12 template (`lab12-starter-20240515.tar.gz`), unless removed by yourself.
- Any new source code and assets added.

Do not modify compiler framework (located in `<pio root>`)!

## Detailed Implementation Requirements

Your implementation should look like the [Reference Implementation](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p3/index.html#), but we do not require them to be identical. For unspecified requirements, please refer to the reference firmware for detail. If there are ambiguous statements, first check Piazza post [@270](https://piazza.com/class/lsvl55gd4jz4h0/post/270), then post on Piazza.

### (3 Points) RISCV Code

- Grading Prerequisites: None.

Use RISCV to create the splash screen (the welcome screen before starting the game or selecting difficulty, depending on your implementation). Comments and labels are not counted. Pseudo-instructions are always counted as one instruction.

Let n be the number of _**meaningful**_ RISCV instructions, then your score for this section is min ( 3 , ⌊ n / 100 ⌋ ) . The term _meaningful_ here refers to: TAs think your splash screen have meaningful animations, **or** your RISCV code does not come from simple copy-and-paste operation.

For example, hand-writing `add t0, t0, t0` or doing useless `lw` and `sw` instructions for 300 times will not lead to 3 points for this section.

Note on Venus: Do not rely only on Venus. Your code may execute normally on Longan Nano even if Venus reports error. In fact, it is suggested not to use Venus in project 3.

Tips: If you want to display a series of images, please make sure that you are not exceeding the maximum memory and storage limit of the hardware. Those two limits will be reported by `pio run` or the build command in PlatformIO VSCode Plugin.

### (4 Points) Moving Entities (Sprites)

**(2 Point)** Display player on screen and player can react to keyboard input.

- Grading Prerequisites: None.

When key k is pushed, the player is given a upward speed. When k is released, the player will fall as if caught by gravity.

k , upward speed, player color, initial player location, and gravity constant are defined by you. An optional constraint on maximum falling speed could also be implemented.

**(2 Point)** There are static walls and moving walls on screen and remaining life will reduce by one if player hit the wall.

- Grading Prerequisites: Player part of _Moving Entities (Sprites)_.

When the player hits any wall, including floor, ceil, and moving wall, the player life should be reduced by one, then the player's location is set to its initial location. However, the game should continue even if the player life reaches 0 or negative, because some TAs are dying again and again when trying Songhui Cao's reference implementation. The initial player life is defined by you.

Player score is not a factor for grading here.

Moving walls refer to the walls that move left in the reference firmware. Wall color, static wall location, wall moving speed, and the vertical and horizonal space between walls are defined by you.

Tips: It is explicitly allowed to eliminate player's hitbox against walls for a while after the player respawns (including initial spawn and respawn after hitting a wall). However, eliminating player's hitbox is an optional feature. In the reference firmware, when the player appears green, its hitbox is eliminated against walls.

Note on collision handling: 1 or 2 pixels of error or overlap is explicitly allowed. You may have noticed that the [Reference Implementation](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p3/index.html#) allows overlap that is no more than 1 pixel.

### (2 Points) Life and Score

**(1 Point)** Player life is correctly displayed.

- Grading Prerequisites: Player part of _Moving Entities (Sprites)_.

When the player life reaches negative, you may do one of the two following: displaying player life as a negative number, **or** adding a constant (defined by you) to player life.

Player life location, font, text, and color is defined by you.

**(1 Point)** Player score is correctly displayed.

- Grading Prerequisites: All 2 parts of _Moving Entities (Sprites)_.

When the player passes through the space between a pair of walls (and did not die), the player score should be increased by one.

Though not scored, we recommend that you handle the case where player score exceeds the display length properly.

Player score location, font, text, and color is defined by you.

### (4 Points) Difficulty Selection

**(2 Points)** You have implemented difficulty selection screen. Different difficulty levels must have identifiable differences.

- Grading Prerequisites: All 2 parts of _Moving Entities (Sprites)_.

At least 2 different difficulties should be implemented so that it is possible to _select_ one. The difficulty selection screen must contain **all** of the following key points: all possible difficulties; highlighting of the chosen option.

The differences must contain **at least one** of the following: player uprising and falling speed; the vertical space between walls; the horizonal space between walls.

**(2 Points)** You have implemented input key de-bouncing during difficulty selection.

- Grading Prerequisites: A working interface (such as difficulty selection screen) as a showcase to your input key de-bouncing implementation.

Definition of input key de-bouncing: if a key (for example, joystick down) is triggered at t 0 , it cannot be triggered again before t 0 + 0.3 , while other keys can. Unit of time here is seconds.

Without input key de-bouncing, during difficulty selection, when you press the controlling key once, the target difficulty may stroll more than once, making accurate selection impossible. Therefore, your task is to implement input key de-bouncing and use it to address this issue.

Input key de-bouncing is an optional feature during actual gameplay.

### (2 Points) Guaranteed Stable FPS

- Grading Prerequisites: None, but TAs will manually review your code for this section.

Assume: 1. The CPU frequency of your Longan Nano is stable and fixed; 2. The compute time per one frame is always less than the length of one frame; 3. Power consumption is not an issue.

According to the assumption above, if you use a fixed delay to maintain FPS, your actual FPS is subject to the compute time within a frame. For example, if the compute time within a frame is 2ms, and you are delaying 20ms for 50 FPS, the actual FPS will be 45.5.

Your task for this section is to change your code, such that your frame rate have nothing to do with the compute time within a frame.

TAs will manually check your code for this section.

Tips: See implementation of `delay_1ms` for how to exploit the CPU cycle counter.

### (2 Points) Player Trace

- Grading Prerequisites: Player part of _Moving Entities (Sprites)_.

Sample player location once per frame, move the sampled points left, and connect adjacent point pairs, so that all points, as a whole, mimics a tail of the player.

Whether or not the tail will fade by time is not a factor for grading here.

Tips: You may modify the LCD library. See piazza post [@313](https://piazza.com/class/lsvl55gd4jz4h0/post/313) for details.

### (2 Points) Incremental Rendering

- Grading Prerequisites: All 2 parts of _Moving Entities (Sprites)_.

Use everything at your disposal, to prevent the screen from blinking.

[Lab 12 Starter](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/labs/Lab12/lab12-starter-20240515.tar.gz) is an example of blinking. It does not implement incremental rendering and is calling `LCD_Clear` periodically.

Criteria of _prevent the screen from blinking_: TA think the screen does not have visual blinking, **or** by reviewing your code, TA is convinced that you are not calling `LCD_Clear` or its equivalents periodically.

Tips: the reference implementation is to first identify difference between the current frame and the last frame, then perform update instead of call `LCD_Clear` and re-draw. You may come up with your own implementation, as long as your screen does not blink.

---

 Dark mode

Author: Songhui Cao (`caosh2022`).

Verified by: Luojia Hu (`hulj`)

Last modified: 2024-05-21
