---
title: CS50X Note [1]
date: 2023-03-07 10:10:38
tags:
- CS50X
- C 语言
- 计算机科学
- 精校翻译
category: 
- 翻译
- 笔记
---

CS50X是我的计算机科学启蒙课，我向来不遗余力地向任何一位想打好基础的新人推荐这门课程

[本篇在CS50X官网的英文原版](https://cs50.harvard.edu/x/2023/notes/1/)

<!--more-->

# Lecture 1
*点击列表中任意的项目可跳转到原版对应部分*

-   [Welcome!](https://cs50.harvard.edu/x/2023/notes/1/#welcome)
-   [Hello World](https://cs50.harvard.edu/x/2023/notes/1/#hello-world)
-   [Functions](https://cs50.harvard.edu/x/2023/notes/1/#functions) [函数]
-   [Variables](https://cs50.harvard.edu/x/2023/notes/1/#variables) [变量]
-   [Conditionals](https://cs50.harvard.edu/x/2023/notes/1/#conditionals) [条件]
-   [Loops](https://cs50.harvard.edu/x/2023/notes/1/#loops) [循环]
-   [Linux and the Command Line](https://cs50.harvard.edu/x/2023/notes/1/#linux-and-the-command-line) [Linux系统和命令行]
-   [Mario](https://cs50.harvard.edu/x/2023/notes/1/#mario)
-   [Comments](https://cs50.harvard.edu/x/2023/notes/1/#comments) [注释]
-   [Abstraction](https://cs50.harvard.edu/x/2023/notes/1/#abstraction) [抽象 | 封装]
-   [Operators and Types](https://cs50.harvard.edu/x/2023/notes/1/#operators-and-types) [运算符和类型]
-   [Summing Up](https://cs50.harvard.edu/x/2023/notes/1/#summing-up)


## Welcome!


-   在我们之前的课程中，我们学习了Scratch，一种可视化编程语言。
-   实际上，当你学习任何编程语言时，Scratch中体现的所有基本编程概念都会被用上。
-   请记住，计算机只能理解 *二进制代码（binary code）*。在人类编写的 *源代码（source code）* 中，包含了计算机可以读懂的指令集，而计算机只能理解我们现在所称的 *机器代码（machine code）*。机器代码是一串以特定模式组成的1和0；计算机通过执行它，可以产生我们在编写源代码时所想要的效果。
-   事实证明，我们可以使用一种特殊的软件叫做 *编译器（compiler）* 来将源代码转换为机器代码。今天，我们将向你介绍一个编译器[^1]，它可以让你将用 C 语言写的源代码转换为机器代码。
-   今天，除了学习如何编程外，你还将学习如何写出好的代码。
-   代码可以从三个方面进行评估。首先，*正确（correctness）* 指的是 “代码是否按照预期运行？” 其次，*设计（design）* 指的是 “代码设计得有多好？” 最后， *风格（style）* 指的是“代码的美观程度和一致性如何？”


[^1]: 实际上是介绍了一个自带编译器的IDE


## Hello World


-   本课程使用的 IDE[^2] 是*Visual Studio Code* ，通常简称为*VS Code* 。
-   我们之所以使用*VS Code* ，其中一个最重要的原因是它已经预装了本课程所需的所有软件，包括之前提到的编译器。本课程和其中的命令行指令均是以*VS Code* 为基础设计的，因此最好始终使用*VS Code* 完成本课程的作业。
-   你可以通过 [code.cs50.io](https://code.cs50.io/) 打开*VS Code* 的在线版本。
-   *VS Code* 可以被分为多个区域：
    
    ![IDE](https://s2.loli.net/2023/03/07/S5DkmdClbgKNvET.png)
    
    请注意，左侧有一个*文件资源管理器* ，你可以在其中找到你的文件。此外，中间有一个称为*文本编辑器* 的区域，你可以在其中编辑你的程序。最后，有一个称为 `命令行界面` 的区域，也称为*CLI*，*命令行* 或*终端窗口*，我们可以在其中向云服务器上的计算机发送命令。
    
-   我们可以通过在终端窗口输入 `code hello.c` 来编写 C 语言的第一个程序。请注意，我们故意对文件只使用小写命名，包括 `.c` 扩展名。接下来，在出现的文本编辑器中编写以下代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("hello, world\n");
    }
    ```
    
    注意，上述每一个字符都有其特定的作用。如果你输错了，程序将无法运行。
    
-   点击终端窗口来回到它之后，你可以通过执行 `make hello` 来编译你的代码。请注意，我们省略了 `.c` 。 `make` 是一个自动编译执行器，它会查找我们的 `hello.c` 文件并将其转换为一个名为 `hello` 的程序。如果执行此命令后没有任何错误，你可以继续。如果有错误，请仔细检查你的代码，确保其与上面的相匹配。
-   现在，请输入 `./hello` 命令，你的程序将会被执行，并打印出 `hello, world` 。
-   现在，在左侧打开*文件资源管理器* 。你会注意到现在有一个名为 `hello.c` 的文件和另一个名为`hello`的文件。`hello.c` 可以被编译器读取：它是保存你代码的文件。 `hello` 是一个可执行文件，你可以运行它，但编译器无法读取它。
-   让我们仔细地看一下我们的代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("hello, world\n");
    }
    ```
    
    请注意，我们的代码被进行了语法高亮[^3]。
	


[^2]: Integrated Development Environment [集成开发环境]
[^3]: 不同意义的代码颜色不同


## Functions [函数]


-   在Scratch中，我们使用 `say` 积木块在屏幕上显示文本。而在 C 语言中，我们有一个被称为 `printf` 的函数，可以做到同样的事情。
-   请注意，我们的代码中已经调用了这个函数：
    
    ```c
    printf("hello, world\n");
    ```
    
    注意到 `printf` 函数被调用。传递给 `printf` 的参数（argument）是 `'hello, world\n'` 。代码语句以 `;` 结尾。
    
-   在 C 语言编程中，常见的错误是漏写分号。按照以下方式修改你的代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("hello, world\n")
    }
    ```
    
    请注意分号现在已经不见了。
    
-   在你的终端窗口中，运行 `make hello` 命令。这时你将会看到许多错误信息！把分号放回正确的位置，然后再次运行 `make hello` 命令，错误信息便消失了。
-   请注意代码中的特殊符号 `\n` 。尝试一下删除它，并通过执行 `make hello` 生成你的程序。在终端窗口中输入 `./hello` ，你的程序有何变化？
-   如下恢复你的程序：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("hello, world\n");
    }
    ```
    
    请注意，分号和 `\n` 已经恢复。
    
-   代码开头的语句 `#include <stdio.h>` 是一个非常特殊的命令，它告诉编译器你想要使用名为 `stdio.h` 的库的功能。这样，你就可以使用 `printf` 函数等许多其他功能。你可以在[手册页面](https://manual.cs50.io/)上阅读这个库的所有功能介绍。
-   事实上，CS50有一个自己的库，叫做 `cs50.h` 。让我们在你的程序中使用这个库。


## Variables [变量]


-   回想一下，在Scratch中，我们可以询问用户 `"What's your name? "` 并在其后说出带有名字的问候。
-   在 C 中，我们也可以这样做。按照以下方式修改你的代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        string answer = get_string("What's your name? ");
        printf("hello, %s\n", answer);
    }
    ```
    
    请注意，在你的代码顶部， `#include <cs50.h>` 被加了进来。代码中 `get_string` 函数用于从用户获取字符串（string）。然后，变量 `answer` 被传递给 `printf` 函数。`%s` 告诉 `printf` 函数准备接收一个字符串。
    
-   `answer` 是一个特殊的数据收容处，我们称之为 *变量* 。`answer` 的数据类型为 `string` ，可以存储任何字符串。还有许多其他的 *数据类型*，如 `int`、`bool`、`char` 等。
-   在终端窗口中再次运行 `make hello` ，你可以输入 `./hello` 来运行程序。程序现在会要求你输入姓名，然后附带你姓名地打印问候语。


## Conditionals [条件]


-   在 Scratch 中，你使用的另一个基本积木块是*条件语句* 。例如，如果 `x` 大于 `y` ，你可能想要执行某个操作。此外，如果条件不满足，你可能还想执行其他操作。
-   在终端窗口中，输入 `code compare.c` 并编写以下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        int x = get_int("What's x? ");
        int y = get_int("What's y? ");
    
        if (x < y)
        {
            printf("x is less than y\n");
        }
    }
    ```
    
    请注意，我们创建了两个变量，分别叫做 `x` 和 `y` ；它们都属于 `int` 类型（整数类型）的。这些变量的值是使用 `get_int` 函数填充的。
    
-   你可以通过在终端窗口执行 `make compare` ，然后执行 `./compare` 来运行你的代码。如果出现任何错误消息，请检查你的代码是否存在错误。
-   我们可以通过以下的代码形式来改进程序：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        int x = get_int("What's x? ");
        int y = get_int("What's y? ");
    
        if (x < y)
        {
            printf("x is less than y\n");
        }
        else if (x > y)
        {
            printf("x is greater than y\n");
        }
        else
        {
            printf("x is equal to y\n");
        }
    }
    ```
    
    请注意，现在，我们的程序已经考虑到了所有潜在的可能性。
    
-   你可以重新编译和运行程序，以测试它是否和我们预期的一样运作。
-   考虑到还存在另一种数据类型，叫做 `char` ，我们可以在终端窗口中输入并运行 `code agree.c` 来开始编写一个新的程序。在文本编辑器中，编写如下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Prompt user to agree
        char c = get_char("Do you agree? ");
    
        // Check whether agreed
        if (c == 'Y' || c == 'y')
        {
            printf("Agreed.\n");
        }
        else if (c == 'N' || c == 'n')
        {
            printf("Not agreed.\n");
        }
    }
    ```
    
    请注意，单引号用于表示单个字符。此外，`==` 确保某物*等于* 另一物，而单个等号在C中有完全不同的功能。最后，`||` 实际上意味着*或* 。
    
-   你可以通过在终端窗口中执行 `make agree` ，然后执行 `./agree` 来测试你的代码。


## Loops [循环]


-   我们还可以在 C 程序中使用 Scratch 中的循环积木块。
-   在你的终端窗口中，输入 `code meow.c` 并编写以下代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        printf("meow\n");
        printf("meow\n");
        printf("meow\n");
    }
    ```
    
    请注意，这样做虽然能够达到预期效果，但是其设计还有进步空间。
    
-   我们可以通过以下的代码形式来改进程序：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        int i = 0;
        while (i < 3)
        {
            printf("meow\n");
            i++;
        }
    }
    ```
    
    请注意，我们创建了一个名为 `i` 的 `int` 变量，并将其赋值为 `0` 。然后，我们创建了一个 `while` 循环，只要 `i` 满足 `i < 3` 的条件，该循环就会一直运行。接着，循环开始执行。每次循环中，我们使用 `i++` 语句将 `i` 加 `1` 。
    
-   同样地，我们可以通过修改代码如下，来实现倒数的功能：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        int i = 3;
        while (i > 0)
        {
            printf("meow\n");
            i--;
        }
    }
    ```
    
    注意我们的计数器（counter） `i` 是从 `3` 开始的。每次循环时， `i` 都会减 `1` 。一旦 `i` 小于零，循环就会停止。
    
-   我们可以进一步改进设计，使用一个 `for` 循环。请修改代码如下：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i < 3; i++)
        {
            printf("meow\n");
        }
    }
    ```
    
    注意 `for` 循环包含三个参数。第一个参数 `int i = 0` 将我们的计数器从零开始计数。第二个参数 `i < 3` 是要检查的条件。最后一个参数 `i++` 告诉循环，每次循环结束后，将计数器加一。
    
-   我们甚至可以使用以下代码来进行无限循环：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        while (true)
        {
            printf("meow\n");
        }
    }
    ```
    
    请注意 `true` 永远不会改变它的真值[^4]。因此，代码将始终运行。运行此代码后，你将失去对终端窗口的控制。你可以通过在键盘上按下 `control-C` 来退出无限循环。
    

[^4]: 这里的true是一个值，而不是变量，它是不可变的


## Linux and the Command Line [Linux系统和命令行]


-   _Linux_ 是一个操作系统，可以通过 VS Code 中的终端窗口，以命令行方式进行访问。
-   一些常见的命令行参数包括：
    -   `cd` ，用于更改当前目录[^5]
    -   `cp` ，用于复制文件和目录
    -   `ls` ，用于列出目录中的文件
    -   `mkdir` ，用于创建一个新的目录
    -   `mv` ，用于移动（重命名）文件和目录
    -   `rm` ，用于删除文件
    -   `rmdir` ，用于删除目录
-   最常用的是 `ls` ，它将列出当前目录或目录中的所有文件。请在终端窗口中输入 `ls` 并按 `enter` ，你将看到当前文件夹中的所有文件。
-   另一个有用的命令是 `mv` ，你可以将一个文件从一个文件移动到另一个文件。例如，你可以使用此命令将 `Hello.c`（注意大写的 `H` ）重命名为 `hello.c` ，方法是输入 `mv Hello.c hello.c` 。
-   你还可以创建文件夹。你可以输入 `mkdir pset1` 来创建名为 `pset1` 的目录。
-   然后，你可以使用 `cd pset1` 将当前目录更改为`pset1` 。

 [^5]: 目录就相当于我们常说的文件夹


## Mario


-   今天我们讨论的一切都集中在你作为程序员的各种基本代码构建块上。
-   接下来的内容将帮助你提前适应这门课程的问题集[^6]：如何解决与计算机科学相关的问题？
-   想象我们想要模拟超级马里奥兄弟游戏的视觉效果。考虑到图示的四个问号方块，我们如何编写代码来大致表示这四个水平方块呢？
	
    ![mario ?](https://s2.loli.net/2023/03/07/BaGsej8MzE9tOxN.png)
    
-   在终端窗口中，输入 `code mario.c` 并编写如下代码：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i < 4; i++)
        {
            printf("?");
        }
        printf("\n");
    }
    ```
    
    请注意这里我们是如何用一个循环来打印出四个问号。
    
-   同样地，我们可以运用相同的逻辑来创建三个垂直砖块。
    
    ![mario v](https://s2.loli.net/2023/03/07/rq5L87ntHfwObvg.png)
    
-   为了实现这个，你可以修改代码如下：
    
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
    
    请注意这里我们是如何用一个循环来打印出三个垂直砖块。
    
-   那么如果我们想要结合这些思想来创建一个三行三列的砖块组合怎么办？
    
    ![mario 3 3](https://s2.loli.net/2023/03/07/38NgDewUqnIrY1X.png)
    
-   我们可以按照上面的逻辑，结合相同的思想来实现。修改代码如下：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        for (int i = 0; i < 3; i++)
        {
            for (int j = 0; j < 3; j++)
            {
                printf("#");
            }
            printf("\n");
        }
    }
    ```
    
    注意，代码中的一个循环嵌套了另一个循环。第一个循环定义了正在打印的垂直行。对于每一行，都会打印出三列。在每一行之后，都会打印一个新的换行符用以换行。
    
-   如果我们想要确保块的数量是*常量（constant）*，即，不可改变的，我们可以将代码修改如下：
    
    ```c
    #include <stdio.h>
    
    int main(void)
    {
        const int n = 3;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                printf("#");
            }
            printf("\n");
        }
	}
    ```
    
    请注意，现在的 `n` 是一个常量。它永远不会被改变。
    
-   就像这节课前面所演示的那样，我们可以让代码提示（prompt）用户输入网格的大小。现在将代码修改如下：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        int n = get_int("Size: ");
    
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                printf("#");
            }
            printf("\n");
        }
    }
    ```
    
    注意 `get_int` 被用于提示用户输入。
    
-   一个关于编程的通用建议是，你永远不应该完全相信你的用户。他们可能会行为不当，在不应该的时候，输入不正确的值。我们可以通过检查用户的输入是否满足我们的需求来使我们的程序免受不正确行为的影响。按照以下方式修改代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        int n;
        do
        {
            n = get_int("Size: ");
        }
        while (n < 1);
    
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                printf("#");
            }
            printf("\n");
        }
    }
    ```
    
    请注意用户是如何被不断提示输入大小，直到用户的输入为 `1` 或更大。
    


[^6]: problem set，就是指这门课程的作业


## Comments [注释]


-   注释是计算机代码的基本部分，你可以在注释中留下解释性的备注，以便自己和其他可能与你合作的人理解你的代码。
-   你为本课程编写的所有代码都必须包含清晰明确的注释。
-   通常每个注释都是几个单词或更多单词，以帮助读者理解特定的代码块正在发生什么。此外，这些注释还可以在你需要修改代码时为你提供提醒。
-   添加注释的一种方法是在代码中放置 `//`，然后加上注释。请按照以下方式修改你的代码以集成注释：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Get size of grid
        int n;
        do
        {
            n = get_int("Size: ");
        }
        while (n < 1);
    
        // Print grid of bricks
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                printf("#");
            }
            printf("\n");
        }
    }
    ```
    
    注意，每个注释都以 `//` 开头。
    


## Abstraction [抽象 | 封装]


-   *抽象化* 是简化代码的艺术；通过抽象化，我们可以把一个问题分解为更小的问题，并逐个解决，最终解决整个问题。
-   现在回过头来看我们的代码，可以看出代码中有两个关键问题：*获取网格的大小* 和 *打印砖块的网格* 。
-   我们可以将这两个问题抽象成单独的函数。将你的代码修改如下：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int get_size(void);
    void print_grid(int n);
    
    int main(void)
    {
        int n = get_size();
        print_grid(n);
    }
    
    int get_size(void)
    {
        int n;
        do
        {
            n = get_int("Size: ");
        }
        while (n < 1);
        return n;
    }
    
    void print_grid(int n)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                printf("#");
            }
            printf("\n");
        }
    }
    ```
    
    注意到现在我们有三个函数。首先，我们有一个名为 `main` 的函数，它调用了另外两个名为 `get_size` 和 `print_grid` 的函数。其次，我们有第二个函数 `get_size`，其中包含了我们之前用来完成获取 `size` 输入的完整代码。第三，我们还有另一个名为 `print_grid` 的函数，用于打印出网格。因为我们在程序中抽象出了基本问题，所以我们的 `main` 函数非常短。
    


## Operators and Types [运算符和类型]


-   *运算符* 指的是编译器支持的数学运算操作。在 C 语言中，这些数学运算符包括：
    
    -   `+` 表示加法
    -   `-` 表示减法
    -   `*` 表示乘法
    -   `/` 表示除法
    -   `%` 表示取余数
-   类型是指可以存储在变量中的数据类型。例如，`char` 类型用于存储单个字符，如 `a` 或 `2` 。
-   类型非常重要，因为每种类型都有特定的限制。例如，由于内存的限制，`int` 类型的最大值为 `4294967296`。
-   你在本课程中可能会与以下类型打交道：
    
    -   `bool`，一个只有 true（真）或 false（假）的布尔表达式
    -   `char`，一个单个字符，比如 a 或 2
    -   `double`，一个浮点数，比 float 类型有更多位数
    -   `float`，一个浮点数，或者是带有小数点的实数
    -   `int`，整数类型，能够表示一定范围内的整数，具体范围由所占用的二进制位数决定
    -   `long`，长整数类型，比 int 类型能够表示更大的整数
    -   `string`，一个由字符组成的字符串
-   你可以在 C 语言中实现一个简单的计算器。在终端中输入 `code calculator.c` ，并编写以下代码：
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(void)
    {
        // Prompt user for x
        int x = get_int("x: ");
    
        // Prompt user for y
        int y = get_int("y: ");
    
        // Perform addition
        printf("%i\n", x + y);
    }
    ```
    
    注意观察如何利用 `get_int` 函数两次从用户获取整数。一个整数存储在名为 `x` 的 `int` 变量中，另一个整数存储在名为 `y` 的 `int` 变量中。然后， `printf` 函数打印了 `x + y` 的值，该值由符号 `%i` 指定。
    
-   在你编写代码时，要特别注意你使用的变量类型，以避免代码中出现未料及的问题。

## Summing Up

在本课中，你学习了如何将Scratch中学到的构建块应用于C编程语言。你学到了：

-   如何在 C 中创建第一个程序。
-    C 本地附带的预定义函数以及如何实现自己的函数。
-   如何使用变量、条件和循环。
-   如何使用 Linux 命令行。
-   如何不断接近一个计算机科学问题的解决方法。
-   如何将注释添加到你的代码中。
-   如何使用抽象来简化和改进你的代码。
-   如何利用类型和运算符。

***下次见！***
