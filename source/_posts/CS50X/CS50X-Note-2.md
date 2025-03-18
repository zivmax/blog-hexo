---
title: CS50X Note [2]
mathjax: false
date: 2023-03-14 10:45:34
tags:
- CS50X
- C 语言
- 计算机科学
- 精校翻译
category: 
- 翻译
- 笔记
---

[本篇在CS50X官网的英文原版](https://cs50.harvard.edu/x/2023/notes/2/)

<!--more-->

# Lecture 2
*点击列表中任意的项目可跳转到原版对应部分*

-   [Welcome!](https://cs50.harvard.edu/x/2023/notes/2/#welcome)
-   [Compiling](https://cs50.harvard.edu/x/2023/notes/2/#compiling) [编译]
-   [Debugging](https://cs50.harvard.edu/x/2023/notes/2/#debugging) [调试]
-   [Arrays](https://cs50.harvard.edu/x/2023/notes/2/#arrays) [数组]
-   [Strings](https://cs50.harvard.edu/x/2023/notes/2/#strings) [字符串]
-   [Command-Line Arguments](https://cs50.harvard.edu/x/2023/notes/2/#command-line-arguments) [命令行参数]
-   [Exit Status](https://cs50.harvard.edu/x/2023/notes/2/#exit-status) [退出状态]
-   [Cryptography](https://cs50.harvard.edu/x/2023/notes/2/#cryptography) [密码学]
-   [Summing Up](https://cs50.harvard.edu/x/2023/notes/2/#summing-up)

## Welcome!

-   在上一节课中，我们学习了一种名为 C 的文本编程语言。
-   本周，我们将更深入地探讨一些额外的代码构建块，以支持我们从底层向上学习编程的目标。
-   本质上来说，除了编程的基础知识外，本课程还涉及到如何解决问题。因此，我们还将进一步专注于如何处理计算机科学问题。

## Compiling [编译]

-   *加密* 是将明文隐藏起来，防止被他人窥探。*解密* 是将一段加密的文本还原为可读的人类语言。
-   一段加密的文本可能长成这样：
    
    ![](https://s2.loli.net/2023/03/15/KyVS7fFWALMa1JE.png)
    
-   回顾一下上周你学习的有关编译器的知识，编译器是一种专门的计算机程序，可以将源代码转换为机器代码，以便计算机能够理解。
-   例如，你可能有一个计算机程序看起来像这样：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("hello, world\n");
    }
    ```
    
-   编译器将读取上面的代码，并将其转换为机器代码,  如下：
    
    ![](https://s2.loli.net/2023/03/15/R8fpdXKDk3GnN5F.png)
    
-   提供给你的编程环境 *VS Code*，使用了一个名为 `clang` 或 *c language* 的编译器。
-   如果你输入 `make hello` ，它会执行一个命令，运行 clang 创建一个输出文件.  你可以作为用户运行这个文件。
-   VS Code 已经被预先配置，使得 `make` 将把 clang 与众多的命令行参数一起运行，以方便作为用户的你使用。
-   考虑以下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string name = get_string("What's your name? ");
        printf("hello, %s\n", name);
    }
    ```
    
-   你可以尝试在终端[^1]中输入：`clang -o hello hello.c` 。你会遇到一个错误，指出clang不知道在哪里找到 `cs50.h` 库。
-   再次尝试编译此代码，请在终端中运行以下命令：`clang -o hello hello.c -lcs50` [^2] 。这将使编译器能够访问 `cs50.h` 库。
-   在终端中运行 `./hello` ，你的程序将正常运行。
-   上面的内容是为了说明编译代码的过程和概念，在CS50中直接使用 `make` 来进行编译是完全可以的，并且是我们所期望你做的事情！
-   编译涉及主要步骤，包括以下内容：
    
    -   首先，*预处理*（preprocessing）是指将你的代码中由 `#`（例如 `#include <cs50.h>` ）指定的头文件有效地复制粘贴到你的文件中。在这个步骤中，`cs50.h` 中的代码会被复制到你的程序中。同样地，正如你的代码包含 `#include <stdio.h>` 一样，存储在你的电脑上的 `stdio.h` 中的代码也会被复制到你的程序中。这个步骤的结果可以被视为下面的形式：
    
        ```c
        ...
        string get_string(string prompt);
        int printf(string format, ...);
        ...
        
        int main(void)
        {
            string name = get_string("What's your name? ");
            printf("hello, %s\n", name);
        }
        ```
    
    -   第二， *编译（compiling）* 是将你的程序转换为汇编代码的过程。这个步骤的结果可以被视为下面的形式：
        
        ![](https://s2.loli.net/2023/03/15/re8nbQawDMUJIZc.png)
        
    -   第三， *汇编（assembling）* 是编译器将你的汇编代码转换为机器代码的过程。这个步骤的结果可以被视为下面的形式：
        
        ![](https://s2.loli.net/2023/03/15/WKMn5R9c3fJGj4P.png)
        
    -   最后，在 *链接（linking）* 步骤中，来自包含库（included libraries）的代码也将被转换为机器代码并与你的代码组合；然后输出最终的可执行文件。
        
        ![](https://s2.loli.net/2023/03/15/HK2O4tMRZypBfgo.png)
        


[^1]: 即终端窗口，在接下来的所有 Note 中我们都简称终端
[^2]: `-lcs50` 中的 `l` 代表 link

## Debugging [调试]

-   每个人在编程时都会犯错误。
-   回过头来看一张上次课上的图片：
    
    ![[cs50Week2Slide061.png]](https://s2.loli.net/2023/03/07/rq5L87ntHfwObvg.png)
    
-   接着，请注意以下代码，其中有一个故意留下的 bug：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i <= 3; i++)
        {
            printf("#\n");
        }
    }
    ```
    
-   在终端中输入 `code buggy0.c` ，并编写上述代码。
-   运行此代码，将出现四个砖块，而不是预期的三个。
-   `printf` 是调试代码时非常有用的一个函数。您可以修改代码如下：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i <= 3; i++)
        {
            printf("i is %i\n", i);
            printf("#\n");
        }
    }
    ```
    
-   在运行这段代码时，你会看到很多语句，包括 `i is 0` ， `i is 1` ，`i is 2` 和 `i is 3` 。当看到这些，你可能就会意识到代码需要进行修正，具体如下：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i < 3; i++)
        {
            printf("#\n");
        }
    }
    ```
    
    注意 `<=` 被 `<` 替换了。
    
-   第二个调试工具被称为 *调试器（debugger）* ，是程序员们创建的软件工具，用来跟踪代码中的错误。
-   在 VS Code 中，已经为您提供了一个预配置的调试器。
-   要使用此调试器，首先，我们要通过单击代码行左侧（行号的左侧）设置一个 *断点（breakpoint）* 。当您单击此处时，将看到一个红点出现。将其想象为一个停止标志，要求编译器在这里暂停，以便您可以考虑在代码的这部分中发生了什么。
    
    ![[cs50Week2Debugging.png]](https://s2.loli.net/2023/03/17/13SBFrdKEo5L9Qs.png)
    
-   其次，运行 `debug50 ./buggy0` 。您会注意到，在调试器启动后，您的代码中的一行将以接近金色的颜色高亮显示。字面意思上，代码已经在这一行代码处 *暂停* 了。注意在窗口左上角，显示有所有的局部变量，包括 `i` ，它当前的值为 `0` 。在窗口顶部，您可以点击击 `step over` 按钮，它将让您的代码继续向下执行。注意 `i` 的值增加随着代码的执行不断增加。
-   虽然这个工具不会告诉您错误在哪里，但它会帮助您放慢速度，并逐步查看代码运行情况。
    
-   为了说明第三种调试方法，请在终端中运行 `code buggy1.c` 来创建一个新文件。按照以下方式编写代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int get_negative_int(void);
    
    int main(void)
    {
        int i = get_negative_int();
        printf("%i\n", i);
    }
    
    // Prompt user for positive integer
    int get_negative_int(void)
    {
        int n;
        do
        {
            n = get_int("Negative Integer: ");
        }
        while (n < 0);
        return n;
    }
    ```
    
    注意 `get_negative_int` 的目的是为了从用户那里获取负整数。
    
-   在运行 `make buggy1` 和 `./buggy1` 后，您会发现它并没有按预期执行。它似乎只接受正整数，而忽略了负整数。
-   与之前一样，在代码的某一行设置断点。最好在 `int i = get_negative_int` 处设置断点。现在，运行 `debug50 buggy1` 。
-   不同于之前在窗口顶部使用 `step over` 按钮，这次我们使用 `step into` 按钮。这将把调试器带入您的 `get_negative_int` 函数。注意这样做是如何帮助你发现代码被困在了 `do while` 循环中。
-   在了解这个死循环的事实后，您可能会考虑为什么会被困在这个循环中，从而导致您按如下方式编辑代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int get_negative_int(void);
    
    int main(void)
    {
        int i = get_negative_int();
        printf("%i\n", i);
    }
    
    // Prompt user for positive integer
    int get_negative_int(void)
    {
        int n;
        do
        {
            n = get_int("Negative Integer: ");
        }
        while (n >= 0);
        return n;
    }
    ```
    
    注意到 `n < 0` 已经被修改了。
    
-   最后一种调试的方式被称为 *橡皮鸭调试* 。当您在代码中遇到困难时，可以考虑大声对一个橡皮鸭（字面意义上）喊出代码问题。如果您不想和一个小塑料鸭子说话，欢迎您告诉身边的人！他们不需要了解如何编程：与他们交谈是您谈论代码的机会。 
	
	*（这段可能看起来有点奇怪，但是原文的确如此。简单地解释一下就是：你可以对别人或小鸭子逐步讲述你这个代码是如何解决问题的，你为什么要这么写，而这其实就相当于在逐步复盘代码的运行逻辑是什么，在你复盘的过程中，你很可能就发现了逻辑上的谬误或者漏洞，进而成功修复 bug）*
	
	
    ![[cs50Week2Slide070.png]](https://s2.loli.net/2023/03/17/Cqco1aErQx8Znpe.png)
    

## Arrays [数组]

- 在第0周，我们讨论了`bool`、`int`、`char`、`string`等 _数据类型_ 。
- 每种数据类型需要一定数量的系统资源：
    - `bool` ：1 byte[^3]
    - `int` ：4 bytes
    - `long` ：8 bytes
    - `float` ：4 bytes
    - `double` ：8 bytes
    - `char` ：1 byte
    - `string` ：? bytes
- 在计算机内部，你的内存是有限的。
    
    ![[cs50Week2Slide084.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide084.png)
    
-   从物理意义上来看，在计算机内存中，你可以想象特定类型的数据是如何存储在计算机中的。你可以想象一个只需要 1 byte 内存的 `char` 类型变量，可能如下所示：
    
    ![[cs50Week2Slide087.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide087.png)
    
- 同样地，一个需要 4 bytes 的 `int` 可能如下所示：
    
    ![[cs50Week2Slide088.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide088.png)
    
- 我们可以创建一个程序来探索这些概念。在您的终端内输入 `code scores.c` 并编写以下代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        // Scores
        int score1 = 72;
        int score2 = 73;
        int score3 = 33;
    
        // Print average
        printf("Average: %f\n", (score1 + score2 + score3) / 3.0);
    }
    ```
    
    请注意右边的数字是一个浮点值 `3.0`，这样计算的结果最终会被呈现为浮点值。
    
- 运行 `make scores`，执行程序。
- 你可以想象这些变量是如何存储在内存中的：
    
    ![[cs50Week2Slide098.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide098.png)
    
-   _数组（Arrays）_ 是一种将数据依次存储在内存中的方式，以便该数据可以轻松访问。

-   `int scores[3]` 是告诉编译器为你提供三个连续的大小为 `int` 的内存空间，用于存储三个 `scores`。顺着我们之前的程序，你可以按照以下方式修改你的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Scores
        int scores[3];
        scores[0] = 72;
        scores[1] = 73;
        scores[2] = 33;
    
        // Print average
        printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
    }
    ```
    
    请注意，`score[0]` 通过在数组 `scores` 中索引到位置 `0`，来查看该内存位置上存储的值。
    
- 你可以从上面的代码可以看出，它确实可以工作，但仍有改进的空间。请按照以下方式修改您的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Get scores
        int scores[3];
        for (int i = 0; i < 3; i++)
        {
            scores[i] = get_int("Score: ");
        }
    
        // Print average
        printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
    }
    ```
    
    请注意，我们通过使用 `for` 循环中提供的 `i`，来索引访问数组 `scores` 中的元素，即 `scores[i]` 。
    
-   我们可以简化或“抽象掉”对平均值的计算。将你的代码修改如下：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    // Constant
    const int N = 3;
    
    // Prototype
    float average(int length, int array[]);
    
    int main(void)
    {
        // Get scores
        int scores[N];
        for (int i = 0; i < N; i++)
        {
            scores[i] = get_int("Score: ");
        }
    
        // Print average
        printf("Average: %f\n", average(N, scores));
    }
    
    float average(int length, int array[])
    {
        // Calculate average
        int sum = 0;
        for (int i = 0; i < length; i++)
        {
            sum += array[i];
        }
        return sum / (float) length;
    }
    ```
    
	请注意，这里声明了一个名为 `average` 的新函数。此外，还声明了一个常量值 `N` 。最重要的是，要注意 `average` 函数接受的参数是 `int array[]` ，这意味着编译器会将一个数组传递给该函数。

- 数组不仅可以作为容器，还可以在函数之间传递。

[^3]: byte 的中文为字节，1 byte = 8 bits

## Strings [字符串]

- 一个 `string` 简单来说就是一个 `char` 类型的数组：也就是一个字符数组。
- 从下面的图片中可以看出，一个字符串就是一个由字符组成的数组，以一个特殊的字符作为结尾，这个特殊字符称为 `NUL character`（空字符）：
    
    ![[cs50Week2Slide116.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide116.png)
    
-   如果用十进制来想象这个数组，它将如下所示：
    
    ![[cs50Week2Slide117.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide117.png)
    
-   在你自己的代码中实现这一点，可以在终端窗口中键入 `code hi.c` ，然后编写以下代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        char c1 = 'H';
        char c2 = 'I';
        char c3 = '!';
    
        printf("%i %i %i\n", c1, c2, c3);
    }
    ```
    
    请注意，这将输出每个字符对应的十进制值。
    
-   为了进一步理解 `string` 的工作原理，请按如下方式修改你的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string s = "HI!";
        printf("%i %i %i\n", s[0], s[1], s[2]);
    }
    ```
    
    请注意 `printf` 语句如何从名为 `s` 的数组中提取并输出三个值。
    
-   假设我们想要同时输出 `HI!` 和 `BYE!` 。请按以下方式修改您的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string s = "HI!";
        string t = "BYE!";
    
        printf("%s\n", s);
        printf("%s\n", t);
    }
    ```
    
	请注意，在这个例子中声明并使用了两个字符串。
-   你可以将其可视化如下：
    
    ![[cs50Week2Slide126.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide126.png)
    
-   我们可以进一步改进这段代码。请按照以下方式修改您的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string words[2];
    
        words[0] = "HI!";
        words[1] = "BYE!";
    
        printf("%s\n", words[0]);
        printf("%s\n", words[1]);
    }
    ```
    
	请注意，这两个字符串都存储在类型为 `string` 的单个数组中。
	
- 编程中常见的一个问题，尤其在 C 语言中更为常见，就是要确定数组的长度。我们可以如何用代码来实现这一点呢？在终端窗口中键入 `code length.c` 并编写如下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Prompt for user's name
        string name = get_string("Name: ");
    
        // Count number of characters up until '\0' (aka NUL)
        int n = 0;
        while (name[n] != '\0')
        {
            n++;
        }
        printf("%i\n", n);
    }
    ```
    
    请注意，此代码循环直到找到 `NUL` （空字符）为止。
    
-   因为这在编程中是一个如此常见的问题，其他程序员已经在 `string.h` 库中创建了代码来找到字符串的长度。你可以通过修改你的代码如下来方便地找到字符串的长度：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    #include <string.h>
    
    int main(void)
    {
        // Prompt for user's name
        string name = get_string("Name: ");
        int length = strlen(name);
        printf("%i\n", length);
    }
    ```
    
    请注意，这段代码使用了在文件顶部声明的 `string.h` 库。此外，它使用了该库中的一个名为 `strlen` 的函数，该函数用于计算传递给它的字符串的长度。
    
- `ctype.h` 是另一个非常有用的库。想象一下，我们想要创建一个程序，将所有小写字符转换为大写字符。在终端窗口中键入 `code uppercase.c` 并编写以下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    #include <string.h>
    
    int main(void)
    {
        string s = get_string("Before: ");
        printf("After:  ");
        for (int i = 0, n = strlen(s); i < n; i++)
        {
            if (s[i] >= 'a' && s[i] <= 'z')
            {
                printf("%c", s[i] - 32);
            }
            else
            {
                printf("%c", s[i]);
            }
        }
        printf("\n");
    }
    ```
    
    请注意，这段代码通过字符串中的每个值进行迭代[^4]。程序检查每个字符。如果字符是小写的，它会从字符的值中减去 32 [^5]，以将其转换为大写。
    
- 回顾我们上周的学习内容，你可能还记得这个 ASCII 值表：
    
    ![[cs50Week2Slide120.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide120.png)
    
- 当从小写字符中减去`32`时，会得到相应大写版本的字符。
- 虽然之前的程序实现了我们的目的，但使用`ctype.h`库有一种更简单的方法。按以下方式修改你的程序：
    
    ```c
    #include <cs50.h>
    #include <ctype.h>
    #include <stdio.h>
    #include <string.h>
    
    int main(void)
    {
        string s = get_string("Before: ");
        printf("After:  ");
        for (int i = 0, n = strlen(s); i < n; i++)
        {
            if (islower(s[i]))
            {
                printf("%c", toupper(s[i]));
            }
            else
            {
                printf("%c", s[i]);
            }
        }
        printf("\n");
    }
    ```
    
    请注意，该程序使用 `islower` 函数来检测每个字符是大写还是小写。然后，`toupper` 函数将 `s[i]` 作为参数传递。每个字符（如果是小写）将被转换为大写。
    
-   又和上次一样，虽然这个程序实现了预期的功能，但仍有改进的空间。正如 `ctype.h` 的文档所述，`toupper` 非常智能，如果传递的字符已经是大写字母，它会直接忽略。因此，我们不再需要 `if` 语句。你可以将代码简化如下：
    
    ```c
    #include <cs50.h>
    #include <ctype.h>
    #include <stdio.h>
    #include <string.h>
    
    int main(void)
    {
        string s = get_string("Before: ");
        printf("After:  ");
        for (int i = 0, n = strlen(s); i < n; i++)
        {
            printf("%c", toupper(s[i]));
        }
        printf("\n");
    }
    ```
    
	请注意，这段代码相当简化，去掉了不必要的`if`语句。

-   您可以在[手册页面](https://manual.cs50.io/#ctype.h)上阅读有关 `ctype` 库的所有功能。

[^4]: 迭代地意思通俗来讲就是挨个处理
[^5]: 所有英文字母的大写和小写形式的 ASCII 码的差值都是 32

## Command-Line Arguments [命令行参数]

- `Command-line arguments`（命令行参数）是指在命令行中传递给你的程序的参数。例如，你在`clang` 后面输入的所有语句都被视为命令行参数。你可以在自己的程序中使用这些参数！
- 在你的终端窗口中，输入 `code greet.c` ，并编写如下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string name = get_string("What's your name? ");
        printf("hello, %s\n", name);
    }
    ```
    
    请注意，这里我们向用户输出了 `hello` 。
    
-   不过，在程序运行之前就提前接受参数不是更好吗？请按照以下方式修改你的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(int argc, string argv[])
    {
        if (argc == 2)
        {
            printf("hello, %s\n", argv[1]);
        }
        else
        {
            printf("hello, world\n");
        }
    }  
    ```
    
    请注意，该程序同时使用了 `argc` （命令行参数的数量）和 `argv` （作为命令行参数传递的字符数组）这两个变量。
    
-   因此，使用该程序的语法，执行 `./greet David` 将导致程序输出 `hello, David`。

## Exit Status [退出状态]
-   当程序结束时，会向计算机提供一个特殊的退出代码。
-   当程序无错误退出时，计算机会返回状态码 `0` 。通常情况下，当程序因发生错误而结束时，计算机会返回状态码 `1` 。
-   你可以编写下面的程序来说明这一点，输入 `code status.c` 并编写如下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(int argc, string argv[])
    {
        if (argc != 2)
        {
            printf("Missing command-line argument\n");
            return 1;
        }
        printf("hello, %s\n", argv[1]);
        return 0;
    }
    ```
    
   请注意，如果你未能提供 `./status David` ，你将会得到一个退出状态码为`1`。然而，如果你提供了 `./status David` ，你将会得到一个退出状态码 `0` 。
    
-  你可以想象一下，你可以如何使用上述程序的一部分来检查用户是否提供了正确数量的命令行参数。

## Cryptography [密码学]
-   密码学是加密和解密消息的艺术。
-   通过向 `cipher` （加密器）提供 `plaintext`（明文）和`key`（密钥），得到 ciphered text（加密文本）。
    
    ![[cs50Week2Slide153.png]](https://cs50.harvard.edu/x/2023/notes/2/cs50Week2Slide153.png)
    
-   **密钥（Key）** 是一个特殊的参数，与明文一起传递给 **密码（cipher）** 。密码使用密钥来做出关于如何实现其密码算法的决策。

## Summing Up 

在这节课中，你学到了有关编译的更多细节，以及数据是如何储存在计算机上的相关知识。具体而言，你学到了：

- 通常情况下，编译器的工作原理。
- 如何使用四种方法调试你的代码。
- 如何在你的代码中使用数组。
- 数组如何将数据存储在内存中相邻的位置。
- `string` 其实就是 `char` 的数组。
- 如何在你的代码中与数组进行交互。
- 如何将命令行参数传递给你的程序。
- 密码学的基本构建块。

***下次见！***