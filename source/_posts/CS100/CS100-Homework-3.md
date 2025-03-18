---
title: CS100 Homework [3]
mathjax: true
date: 2023-03-09 09:44:53
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
1. [Chihiro Fujisaki and Caesar Cipher](https://cs100.geekpie.club/p/P301?tid=6408b595836fd4c1b898bd0a)
1. [Chihiro Fujisaki and Keyworded Caesar Cipher](https://cs100.geekpie.club/p/16?tid=6408b595836fd4c1b898bd0a)
1. [Nagito Komaeda and lucky days](https://cs100.geekpie.club/p/20?tid=6408b595836fd4c1b898bd0a)
1. [Chiaki Nanami Wants to Play Games](https://cs100.geekpie.club/p/19?tid=6408b595836fd4c1b898bd0a)
1. [Gundham Tanaka and the Dark Devas of Destruction](https://cs100.geekpie.club/p/21?tid=6408b595836fd4c1b898bd0a)

<!--more-->

---

# CS100 Homework 3 (Spring 2023)

Deadline: 2023-03-15 (Wed) 23:59:59

Late submission will open for 24 hours after the deadline, with 50% point deduction.

If you get full marks in this assignment by no more than 40 submission attempts and **also solve the challenge in problem 4**, you can earn special OJ displays and a "1-case protection" that can be used in further assignments to cancel one testcase failure. See Piazza or OJ dashboard for more information.

---

## Chihiro Fujisaki and Caesar Cipher 

*"If I don't do something, nothing's ever going to change."*

### Description: 

Chihiro Fujisaki, the Ultimate Programmer, is developing a new kind of artificial intelligence called Alter Ego. Now he is training the artificial intelligence secretly in order to prevent Monokuma spotting it. 

Fujisaki is training Alter Ego with [Caesar cipher](https://en.wikipedia.org/wiki/Caesar_cipher), an ancient encryption algorithm. With a fixed number as `key`, for example, $3$, Caesar cipher replaces each letter in the plaintext a letter $3$ positions down the alphabet. In this case, the message `box` becomes `era`, because going down $3$ letters turns `b` into `e` and turns `x`, going through the alphabet, into `a`.

Fujisaki's messages contain uppercase letters, lowercase letters, digits, and other characters like spaces and punctuations. He doesn't want to scramble his messages, so he <u>keeps the cases and encrypts uppercase letters, lowercase letters and digits respectively</u> (so that `Z` encodes to `C` and `9` to `2`). All other characters, including spaces and punctuations, are not encoded. 


Monokuma has discovered this secret, and has obtained the secret `key`. Please help Monokuma to decrypt Fujisaki's messages. 

### Input Format: 

The input contains two lines: 

The first line contains a number $n$, the `key` for Caesar cipher. $n>0$ shifts the message right (going down the alphabet), and $n<0$ shifts it left (going up the alphabet).

The second line contains a non-empty string `ciphertext`, the encoded text, ending with a line break character (`'\n'`). Only <u>uppercase letters, lowercase letters, and digits</u> have been encoded.

It is guaranteed that $|n|\leq 10^6, |ciphertext|\leq 10^6$.

### Output Format: 

You need to print the decoded message in one line. 

**Sample Input 1** 

```
8
Qv KA988, xtioqizqau qa bismv dmzg amzqwca! Mdmzg gmiz, uivg abclmvba omb kicopb ivl omb xcvqapml!
```

**Sample Input 2** 

```
-30
Zk JKP odwna ukqn ykza sepd kpdano, zk JKP yklu ykza bnki Ejpanjap kn kxpwej ykza bnki opqzajpo ej lwop uawno, wjz zk JKP qoa YdwpCLP ej dkiaskngo! Sa ydayg whh ukqn oqxieooekjo wjz sa SEHH gjks!
```

**Sample Outputs...?**

Whoops, ciphers are no more secret if we tell you the answers! Why not figure out yourself? Hints: the above samples all decode to proper English sentences. 


### Test cases
[👉 Go to GitHub](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob1/testcases)


---

## Chihiro Fujisaki and Keyworded Caesar Cipher 

*"I'm scared, but...I can handle it. I...don't really understand why, but...*

*When I think about everyone else, my courage starts to grow...!"*

### Description: 

Monokuma has cracked Chihiro Fujisaki's Caesar Cipher. The Ultimate Programmer Fujisaki is working on a more secure code called **"Keyworded Caesar Cipher"**. Let Fujisaki explain his genius ideas to you:

"My keyworded Caesar cipher no longer has a numerical `key`, but a `keyword`, for example, `"Wednesday"`. Just as the original Caesar cipher creates a shifted alphabet for encoding letters, so too does my keyworded Caesar cipher have one, and this encoding alphabet is created by the `keyword` `"Wednesday"`.

"I first turn all letters into lowercase, and remove duplicate letters in the keyword, keeping only their first occurences. Now `"Wednesday"` has become `"wednsay"`.

"Then I write the alphabet `a` through `z` in one line, and my new keyword `"wednsay"` beneath it.


    a b c d e f g h i j k l m n o p q r s t u v w x y z
    w e d n s a y


"And finally, I fill the rest of my encoding alphabet starting from the last letter `y`. However, **letters in the keyword are already used**. I should <u>skip these letters when filling the rest</u>, because I cannot let two letters encode to a same value.


    a b c d e f g h i j k l m n o p q r s t u v w x y z
    w e d n s a y z b c f g h i j k l m o p q r t u v x

"Alright. I can use it to encode `a` to `w`, `z` to `x`, and my name `Chihiro` to `Dzbzbmj`! For uppercase letters I can do the same, but not for digits any more. It's no big deal. I know in homework 1 you turned numbers into English words, right? (laughs)"

How nice would it be, if only you and he know the secret! Monokuma has heared all your conversation and learned his new method. More importantly, Monokuma has obtained the `keyword`! Please help Monokuma again to decrypt Fujisaki's messages. 

### Input Format: 

The input contains two lines: 

The first line contains a non-empty string `keyword`, containing only uppercase and lowercase letters.

The second line contains a non-empty string `ciphertext`, the encoded text, ending with a line break character (`'\n'`). Only <u>uppercase and lowercase letters</u> have been encoded.

It is guaranteed that $1 \leq |keyword|\leq 100,|ciphertext|\leq 10^6$.

### Output Format: 

You need to print the decoded message in one line. 

**Sample Input 1**

```
CPPistheBESTlanguage  
Ewws mcq wh btgxlve: sldijdd qwjz lstcd mlfb qwjz hzltvsd. 
```


**Sample Input 2**

```
CPPistheBESTlanguage  
Pcs mcq wh btgxlve: elkt qwjz iwst fw dwutwvt.
```

**Sample Input 3**

```
ProgrammingIsFun  
Qyhc oyga pxg qyhc pooyhxed pca qyhc daocae. Qyh npja ena cadzyxdsrsvseq ey zcyeaoe enaw. Gy XYE vae pxqyxa pooadd qyhc pooyhxe, hda qyhc oywzheac, yc vyyu pe qyhc oyga!
```

**Sample Outputs...?**

Secret, of course! Again, they all decode to proper English sentences. 

#### Hints

You may find some functions in `<string.h>` useful, for example, `strlen`, `strchr`, `strcmp`. Here is [a full list of standard library support for string and char manipulations](https://en.cppreference.com/w/c/string/byte).


### Test cases
[👉 Go to GitHub](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob2/testcases)


---

## Nagito Komaeda and lucky days 

*"I believe my actions will become the foundation of this world's hope. And...if that really happens... Praise me."*

### Description 

Some people believe numbers with certain properties, like palindromes, are special, so does Nagito Komaeda. Komaeda wants to choose a date for himself to do some important things. As the Ultimate Lucky Student, he finds the dates he chooses are all palindrome dates. He is very interested in this kind of special dates. 

We define a date `YYYYMMDD` (`Y`, `M`, `D` are digits for year, month, and date) to be a **palindrome date** if it is palindrome, that is, its digits remain the same after being reversed. For example, we can write Apr. 4th, 2022 as `20220404` and write Dec. 30th, 321 as `03211230` (We also zero-fill the year to make it exactly $8$ digits). The date `03211230` is a palindrome date.

However, Komaeda has a special calendar, in which there are a total of $m$ months in a year, and the $i$-th month will have $d_i$ days. Luckily, there is no leap year(闰年) in his calendar, so there is no need to calculate whether a year is a leap year. 

Nagito Komaeda wants to know the total number of palindrome dates in a certain time interval. He is so lucky that he can randomly guess out the correct answer, but you cannot. You want to write a program to calculate the number of palindrome dates. 

### Input Format: 
The input contains 4 lines.

In the first line is a number $m$, indicating the number of months each year in the special calendar. 

In the second line are $m$ numbers separated by spaces. The $i$-th number $d_i$ indicates that the $i$-th month has $d_i$ days. 

In the next two lines, you will get two dates $start$ and $end$ respectively, representing the start date and the end date, in the form of `YYYYMMDD`.

It is guaranteed that:
- $1\leq m\leq99,1\leq d_i\leq 99$ .
- $00010101 \leq start < end \leq 99999999$ .

### Output Format: 

You only need to output one integer, the number of palindrome dates in the range $[start, end]$ (both ends inclusive). 

### Sample Input: 

```
12  
31 28 31 30 31 30 31 31 30 31 30 31 
20000101  
20101231  
```

### Sample Output: 

```
2 
```

### Sample Explanation: 

There are two palindrome dates between the two dates: $20011002$ and $20100102$. 

(In this sample, the calendar is exactly "our" calendar!)



### Test cases
[👉 Go to GitHub](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob3/testcases)

---

## Chiaki Nanami Wants to Play Games 

*"If you want to believe in someone\...you need to overcome doubt first. Belief without doubt\...is simply a lie."*

### Description: 

Chiaki Nanami, the Ultimate Gamer, now wants to play games. She has a lot of games in her game console and wants to play some of them. Nanami has marked every game with its game type (like FPS or Rogue-like), represented by an integer. If two game types are marked by the same integer, the two games have the same type and vice versa. Nanami also records the price of each game. 

The Ultimate Game Player, Nanami Chiaki, also has her favorite type of games. However, she forgot which type of games is her favorite. She only knows that She loves those games so much that <u>the money she spent on her favorite type of games is over half of the total cost of all games</u>.

Please help Nanami to find her favorite game type. 

### Input Format: 

You will first recieve a number $n$, which indicates the number of games Nanami has. Then in each of the next $n$ lines you will recieve a tuple. The tuple$(s_i,p_i,t_i)$ in $i$-th line represents the **name**, **price** and **type** of the $i$-th game. 

It's guaranteed that there will be no comma in $s_i$, the game names. The game types may not be consecutive (i.e. the $t_i$s may not simply be $1, 2, 3...$). 

For $100\%$ input, 
 - $1\leq n\leq10^6$ 
 - $1 \leq p_i\leq 2\times10^6$
 - $1\leq t_i\leq 10^6$ 
 - $\sum_{i=1}^{n}|s_i|\leq 10^7$ 

### Output Format: 

You only need to print one integer, Nanami's favorite type of games. 

### Sample Input: 

```
3  
(Goose Goose Duck,2,1) 
(CSGO,4,2)  
(Among us,3,1)  
```

### Sample Output: 

```
1
```

### Sample Explanation: 

In the sample given, there are $3$ games in Nanami's game console. The cost of game type $1$ is $2+3=5$ and the cost of game type $2$ is $4$. The cost of game type $1$ is over the half of the total cost $4+5=9$, so her favorite type is $1$. 

### Challenge (Golden Strawberry): 

**This problem can be solved in a clever way! You don't need to record all games' types and costs into arrays, and in fact you only need very few variables! Can you solve the problem by declaring at most 10 variables (without using arrays or pointers)? Submit your solution to the additional problem 6 to win the golden strawberry award!** 

Hint: Use the property of the favorite games. 


### Test cases
[👉 Go to GitHub](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob4/testcases)


---

## Gundham Tanaka and the Dark Devas of Destruction

*\"Pitiful humans\...they refuse to lift their heads up for fear of doubting the authenticity of the blue sky\...\"*

### Description:

Gundham Tanaka, the Ultimate Breeder, kept a lot of animals, such as hamsters and eagles. He named all his animals respectively (like his four favourite hamsters, \"the Four Dark Devas of Destruction\"). One day, the \"Dark Devas of Destruction\" (his hamsters, actually) wanted to play a game with him.

These hamsters form a matrix of $r$ rows and $c$ columns, but facing to different directions `L` (left), `R` (right), `U` (up), or `D` (down). Among them are some special hamsters which Tanaka needs to find out.

At first, a leading hamster will hint where the first special hamster is. Whenever he finds a special hamster, it will also hint the position of the next special hamster to him. All hints are in forms like \"The next special hamster is the $p_i$-th one in front/back/left/right of me\".

The hamsters make mistake sometimes. When a hamster gives a position, if it is <u>out of bound </u>, or if <u>the hamster at that position has already been identified as a special hamster previously</u>, Tanaka will stop the game and tell his hamsters that they have made a mistake.

Now Tanaka is playing the game, please help him find all special hamsters.

### Input Format:

In the first line, you will receive $3$ numbers $r,c,q$, separated by spaces, indicating the number of rows and columns of the hamster matrix, and the number of hints given by hamsters.

In the next $r$ lines, you will receive a $r$-row $c$-column word matrix$\{d_{ij}\}(d_{ij} \in \{$ `L`, `R`, `U`, `D`$\})$. $d_{ij}$ represents the direction of the hamster in the $i$-th row and $j$-th column, facing `L` (left), `R` (right), `U` (up), or `D` (down).

In the next line, you will receive $2$ numbers $r_{leading},c_{leading}$, indicating the row and column of the leading hamster.

In the next $q$ lines, each line you will receive a word $w_i(w_{i} \in \{$ Left, Right, Front, Back $\})$ and an integer $p_i$, meaning that the hint is \"The next hamster is the $p_i$-th in $w_i$ of me\".

Another thing you need to know: Hamsters are not computers, they count from $1$ rather than $0$.

It is guaranteed that:

- $2 \leq r,c\leq500$
- $1 \leq q \leq 100000$

### Output Format:

In each line, you should print `(r, c)`, the row and column of a special hamster, in their order of being found, until the game ends or the last one is found. If the hamsters made mistake in one hint, you should print `Mistake!` in the corresponding line and end the game.

### Sample Input:

```
2 2 4
U L
D D
1 1
Back 1
Left 1
Left 10
Right 1
```

### Sample Output:

```
(2, 1)
(2, 2)
Mistake!
```

### Sample Explanation:

Their directions:

↑ ←
↓ ↓

The leading hamster is at $(1,1)$. It says that the first special hamster is the $1$st hamster in its back, at $(2,1)$.

Notice that the hamster at $(2,1)$ is facing down, and it says the next special hamster is in <u>**its**</u> left, which actually means to the right (back's left) of the matrix, which is $(2,2)$.

The next special hamster is obviously unable to find, so there is a mistake. The game ends, ignoring any more hints from hamsters.


### Test cases
[👉 Go to GitHub](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob5/testcases)

---


## Reference solutions
*Remember, you should always solve all the problems yourself and passed all the testcases first.*

1. [Happy Coding](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob1)
1. [Quadratic Equation](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob2)
1. [Hexadecimal Calculator](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob3)
1. [Bit Operation](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob4)
1. [No-Horse Sudoku](https://github.com/zivmax/CS100-2023-Spring-homework-resources/tree/main/hw3_refsol_and_testcases/prob5)