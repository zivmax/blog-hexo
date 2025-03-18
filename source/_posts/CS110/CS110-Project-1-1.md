---
title: CS110 Project 1.1
mathjax: false
description: A RISC-V Assembler (Individual Project)
date: 2024-03-26 11:26:20
tags:
- CS110
- 计算机体系架构
- 上科大
- 计算机科学
category:
- 学习
- 项目
---



# Project 1.1: A RISC-V Assembler (Individual Project)

[Computer Architecture I](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/index.html) [ShanghaiTech University](https://www.shanghaitech.edu.cn/)

  


## IMPORTANT INFO - PLEASE READ
The projects are part of your design project worth 2 credit points. As such they run in parallel to the actual course. So be aware that the due date for project and homework might be very close to each other! Start early and do not procrastinate.

---

## Introduction to Project 1.1

In Project 1.1, you are going to make a simple one-pass RISC-V assembler. The assembler takes RISC-V codes which contain no labels and symbols as input and outputs corresponding machine codes. You also need to implement basic error handling to detect invaild instructions. You can fetch the framework for Project 1.1 [here](https://classroom.github.com/a/ga24pVYj) on Github classroom, try to use git and Github for version control.

## Background of The Instruction Set

### Registers

Please consult the [RISC-V Green Sheet (PDF)](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2023/lecture_notes/riscvcard.pdf) for register numbers, instruction opcodes, and bitwise formats. Our asembler will support all 32 registers: **zero**, **ra**, **sp**, **gp**, **tp**, **t0-t6**, **s0 - s11**, **a0 - a7**. Other register numbers (eg. x0, x1, x2 etc.) shall be also supported. Note that floating point registers are not included in this project.

### Instructions

We will have 42 instructions and 6 pseudo-instructions to assemble. The instructions are:

<table>
  <thead>
      <tr>
          <td>Instruction</td>
          <td>Type</td>
          <td>Opcode</td>
          <td>Funct3</td>
          <td>Funct7/IMM</td>
          <td>Operation</td>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td><span class="inst">add</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td rowspan="14">R</td>
          <td rowspan="14"><span>0x33</span></td>
          <td>0x0</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">mul</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x0</td>
          <td>0x01</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="vars">R</span>[<span class="rgtr">rs1</span>] * <span class="vars">R</span>[<span class="rgtr">rs2</span>])[31:0]</td>
      </tr>
      <tr>
          <td><span class="inst">sub</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x0</td>
          <td>0x20</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] - <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">sll</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x1</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &lt;&lt; <span class="vars">R</span>[<span>rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">mulh</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x1</td>
          <td>0x01</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="vars">R</span>[<span class="rgtr">rs1</span>] * <span class="vars">R</span>[<span class="rgtr">rs2</span>])[63:32]</td>
      </tr>
      <tr>
          <td><span class="inst">slt</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x2</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="vars">R</span>[<span class="rgtr">rs1</span>] &lt; <span class="vars">R</span>[<span class="rgtr">rs2</span>]) ? 1 : 0</td>
      </tr>
      <tr>
          <td><span class="inst">sltu</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x3</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>]) &lt; <span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs2</span>])) ? 1 : 0</td>
      </tr>
      <tr>
          <td><span class="inst">xor</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x4</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] ^ <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">div</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x4</td>
          <td>0x01</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] / <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">srl</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x5</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &gt;&gt; <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">sra</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x5</td>
          <td>0x20</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &gt;&gt; <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">or</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x6</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] | <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">rem</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x6</td>
          <td>0x01</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="vars">R</span>[<span class="rgtr">rs1</span>] % <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">and</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span></td>
          <td>0x7</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &amp; <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">lb</span> <span class="rgtr">rd</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td rowspan="16">I</td>
          <td rowspan="5"><span>0x03</span></td>
          <td>0x0</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="func">SignExt</span>(<span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>, <span class="lite">byte</span>))</td>
      </tr>
      <tr>
          <td><span class="inst">lh</span> <span class="rgtr">rd</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x1</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="func">SignExt</span>(<span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>, <span class="lite">half</span>))</td>
      </tr>
      <tr>
          <td><span class="inst">lw</span> <span class="rgtr">rd</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x2</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>, <span class="lite">word</span>)</td>
      </tr>
      <tr>
          <td><span class="inst">lbu</span> <span class="rgtr">rd</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x4</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="func">U</span>(<span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>, <span class="lite">byte</span>))</td>
      </tr>
      <tr>
          <td><span class="inst">lhu</span> <span class="rgtr">rd</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x5</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="func">U</span>(<span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>, <span class="lite">half</span>))</td>
      </tr>
      <tr>
          <td><span class="inst">addi</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td rowspan="9">0x13</td>
          <td>0x0</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">slli</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x1</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &lt;&lt; <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">slti</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x2</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="vars">R</span>[<span class="rgtr">rs1</span>] &lt; <span class="immd">imm</span>) ? 1 : 0</td>
      </tr>
      <tr>
          <td><span class="inst">sltiu</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x3</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← (<span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>]) &lt; <span class="func">U</span>(<span class="immd">imm</span>)) ? 1 : 0 </td>
      </tr>
      <tr>
          <td><span class="inst">xori</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x4</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] ^ <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">srli</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x5</td>
          <td>0x00</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &gt;&gt; <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">srai</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x5</td>
          <td>0x20</td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &gt;&gt; <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">ori</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x6</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] | <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">andi</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x7</td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] &amp; <span class="immd">imm</span></td>
      </tr>
      <tr>
          <td><span class="inst">jalr</span> <span class="rgtr">rd</span>, <span class="rgtr">rs1</span>, <span class="immd">imm</span></td>
          <td>0x67</td>
          <td>0x0</td>
          <td></td>
          <td class="c8">
              <span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">PC</span>  +  <span class="lite">4</span>
              <br>
              <span class="vars">PC</span> ← <span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">imm</span>
          </td>
      </tr>
      <tr>
          <td><span class="inst">ecall</span>
          </td><td>0x73</td>
          <td>0x0</td>
          <td>0x000</td>
          <td class="c8">
              <span>(Transfers control to operating system)</span>
              <br>
              <span class="rgtr">a0</span> = <span class="immd">1</span> is print value of <span class="rgtr">a1</span> as an integer.
              <br>
              <span class="rgtr">a0</span> = <span class="immd">4</span> is print the string at address <span class="rgtr">a1</span>.
              <br>
              <span class="rgtr">a0</span> = <span class="immd">10</span> is exit or end of code indicator.
              <br>
              <span class="rgtr">a0</span> = <span class="immd">11</span> is print value of <span class="rgtr">a1</span> as a character.
          </td>
      </tr>
      <tr>
          <td><span class="inst">sb</span> <span class="rgtr">rs2</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td rowspan="3">S</td>
          <td rowspan="3">0x23</td>
          <td>0x0</td>
          <td></td>
          <td><span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>) ← <span class="vars">R</span>[<span class="rgtr">rs2</span>][7:0]</td>
      </tr>
      <tr>
          <td><span class="inst">sh</span> <span class="rgtr">rs2</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x1</td>
          <td></td>
          <td><span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>) ← <span class="vars">R</span>[<span class="rgtr">rs2</span>][15:0]</td>
      </tr>
      <tr>
          <td><span class="inst">sw</span> <span class="rgtr">rs2</span>, <span class="immd">offset</span>(<span class="rgtr">rs1</span>)</td>
          <td>0x2</td>
          <td></td>
          <td><span class="func">Mem</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>] + <span class="immd">offset</span>) ← <span class="vars">R</span>[<span class="rgtr">rs2</span>]</td>
      </tr>
      <tr>
          <td><span class="inst">beq</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td rowspan="6">SB</td>
          <td rowspan="6">0x63</td>
          <td>0x0</td>
          <td></td>
          <td class="c8">
              if(<span class="vars">R</span>[<span class="rgtr">rs1</span>] == <span class="vars">R</span>[<span class="rgtr">rs2</span>]) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">bne</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td>0x1</td>
          <td></td>
          <td class="c8">
              if(<span class="vars">R</span>[<span class="rgtr">rs1</span>] != <span class="vars">R</span>[<span class="rgtr">rs2</span>]) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">blt</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td>0x4</td>
          <td></td>
          <td class="c8">
              if(<span class="vars">R</span>[<span class="rgtr">rs1</span>] &lt; <span class="vars">R</span>[<span class="rgtr">rs2</span>]) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">bge</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td>0x5</td>
          <td></td>
          <td class="c8">
              if(<span class="vars">R</span>[<span class="rgtr">rs1</span>] &gt;= <span class="vars">R</span>[<span class="rgtr">rs2</span>]) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">bltu</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td>0x6</td>
          <td></td>
          <td class="c8">
              if(<span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>]) &lt; <span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs2</span>])) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">bgeu</span> <span class="rgtr">rs1</span>, <span class="rgtr">rs2</span>, <span class="immd">offset</span></td>
          <td>0x7</td>
          <td></td>
          <td class="c8">
              if(<span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs1</span>]) &gt;= <span class="func">U</span>(<span class="vars">R</span>[<span class="rgtr">rs2</span>])) 
              <br>
              &nbsp;<span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">offset</span>, 1b'0}
          </td>
      </tr>
      <tr>
          <td><span class="inst">auipc</span> <span class="rgtr">rd</span>, <span class="immd">offset</span></td>
          <td rowspan="2">U</td>
          <td>0x17</td>
          <td></td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← PC + {<span class="immd">offset</span>, 12b'0}</td>
      </tr>
      <tr>
          <td><span class="inst">lui</span> <span class="rgtr">rd</span>, <span class="immd">offset</span></td>
          <td>0x37</td>
          <td></td>
          <td></td>
          <td><span class="vars">R</span>[<span class="rgtr">rd</span>] ← {<span class="immd">offset</span>, 12b'0}</td>
      </tr>
      <tr>
          <td><span class="inst">jal</span> <span class="rgtr">rd</span>, <span class="immd">offset</span></td>
          <td>UJ</td>
          <td>0x6f</td>
          <td></td>
          <td></td>
          <td class="c8">
              <span class="vars">R</span>[<span class="rgtr">rd</span>] ← <span class="vars">PC</span>  +  <span class="lite">4</span>
              <br>
              <span class="vars">PC</span> ← <span class="vars">PC</span> + {<span class="immd">imm</span>, 1b'0}
          </td>
      </tr>
  </tbody>
</table>

**NOTE:** Since our assembler is a one-pass assembler, the **offset** in **SB** and **U** type and **imm** in **UJ** type will be integers.

The pseudo-instructions are:

|PSEUDO-INSTRUCTION|FORMAT|USES|
|---|---|---|
|Branch on Equal to Zero|beqz rs1, label|beq|
|Branch on not Equal to Zero|bnez rs1, label|bne|
|Jump|j label|jal|
|Jump Register|jr rs1|jalr|
|Load Immediate|li rd, immediate|lui, addi|
|Move|mv rd, rs1|addi|

For further reference, here are the bit lengths of the instruction components.

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|R-TYPE|funct7|rs2|rs1|funct3|rd|opcode|
|Bits|7|5|5|3|5|7|

  

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|I-TYPE|imm[11:0]|rs1|funct3|rd|opcode|
|Bits|12|5|3|5|7|

  

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|S-TYPE|imm[11:5]|rs2|rs1|funct3|imm[4:0]|opcode|
|Bits|7|5|5|3|5|7|

  

|   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|
|SB-TYPE|imm[12]|imm[10:5]|rs2|rs1|funct3|imm[4:1]|imm[11]|opcode|
|Bits|1|6|5|5|3|4|1|7|

  

|   |   |   |   |
|---|---|---|---|
|U-TYPE|imm[31:12]|rd|opcode|
|Bits|20|5|7|

  

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|UJ-TYPE|imm[20]|imm[10:1]|imm[11]|imm[19:12]|rd|opcode|
|Bits|1|10|1|8|5|7|

## Getting Started

### File Structure and Usage

The directory tree of the framework should like the following:

```

    .
    ├── inc
    │   ├── assembler.h
    │   └── util.h
    ├── Makefile
    ├── main.c
    ├── src
    │   ├── assembler.c
    │   └── util.c
    └── test
        ├── test.ref
        └── test.S
  
```

main.c is the entry of the whole assembler. You should not modify this file.

assembler.c and assembler.h are where you implement the assembler function.

util.c and util.h contain some helper functions. You can also add useful functions there.

test directory contains a basic test and the correspoding result.

### Build & Execute

1. Run make to compile the code and assembler executable file will be main
2. Or you can build the code with CMake. First make a directory build. Then run cmake .. && make under build. The executable file will be build/main
3. To run the assembler, type main input_file output_file . input_file contains RISC-V instructions (see below for detailed description). output_file is where you output your results to.
4. Run make test to test your codes with test/test.S and your output file will be test/test.out

### Input & Output

#### Input

Input will be a file containing RISC-V instructins. You can assume there are no empty rows and comments and each line ends with a `\n`. We will use **space** as delimiter instead of comma, e.g. **add x1 x2 x3**.

#### Output

Output shoud be RISC-V machine codes. You should use function dump_code in src/util.c when outputing machine codes. This function will requrie a file handler and a uint32_t variable as parameters, which should be the output file and code to be dumped. Do not use your own output function, otherwise, there may be format problems. Also, do no change the output format in dump_code since we will use your util.c when grading.

### Error Handling

If the input file contains some illegal instructions, you should find it and output error information to the output file. You should use function dump_error_information in src/util.c for outputing error information. Once an error occurs, you should continue to assemble the rest instructions and keep outputing results and errors. Also, you should not directly finish the whole program using exit. Quiting unexpectedly will be viewed as run time error.

To simplify the error handling part, we promise that there will only be one space between each string. Also, you do not need to handle cases where there are more or less parameters in an instruction, like addi a0 a1 or addi a0 a0 a0 1. Load/Store instructions will always be the correct format, e.g. lw a0 0(a1). But the correctness of registers and offset is not guaranteed.

Here are situations you need to consider in this project:

1. Non-existent instruction: All supported instructions are listed above and any other instrcutions should be viewed as illegal.
2. Bad registers: Wrong names of register or registers which are out of scope should be detected, e.g. rp, x32. You don't need to handle situations like x01 and a-1
3. Bad immediate or offset: The imm or offset in instructions may not be a number, e.g. addi a0 a1 a0.
4. Immediate out of range: The immediate in some instructions should be limited into some scope, since the number of bits to represent imm is limited. For example, imm in addi should be between -2048 and 2047. You can refer to [Venus](https://venus.cs61c.org/) and the [RISC-V manual](https://toast-lab.sist.shanghaitech.edu.cn/courses/CS110@ShanghaiTech/Spring-2024/project/p1.1/Project%201.1%20-%20Computer%20Architecture%20I%20-%20ShanghaiTech%20University_files/riscv-spec-20191213.pdf) for more information about the limitation.

## Testing

### Diff

Use diff file1 file2 to compare your output with the reference answer. Note that we will use diff to check your answer. To see how to interpret diff results, [click here](https://en.wikipedia.org/wiki/Diff#Usage)

### Valgrind

To check memory leak, you can use Valgrind by running valgrind --tool=memcheck --leak-check=full --track-origin=yes main input_file output_file

### Venus

Venus is a powerful assembler and you can use Venus to test the correctness of your code.

First type RISC-V instructions at the editor page. Then at the simulator page, you can see the machine code of each instruction. You can also use Dump button to collect all machine codes as a reference.

## Tips

1. Immediate in auipc and lui should be between 0 and 1048575. Venus views this immediate as an unsigned integer by defulat, while the official manual does not mention this. We choose to follow Venus. For auipc, since the starting address of text is smaller than that of data, PC-relative addresses are always larger than current PC, causing non-negative offset. For lui, it will load upper part of the immediate into the register, which does not care about the sign.
2. Immediate in jal should be between -1048576 and 1048575. Venus does not limit this immediate for some reasons, even if immediates out of this range can not be represented. However, we are going to follow the hardware limitation.
3. This project needs a lot of spliting operations. You may find strtok useful.
4. You need to check whether the immediate in li instruction is between -2048 and 2047. If so, li should be translated into only one addi instruction. Otherwise, it will be translated into lui and addi
5. Try to generate your own test cases. Codes need testing.
6. Don't forget writing comments frequently.

## Submission

You should submit your code via Github. Please follow the guidance in Gradescope to submit your codes on Github. Note that we will not use your main.c or Makefile for grading. The compilation flag will be `-Wpedantic -Wall -Wextra -Wvla -Werror -std=c11`.

---

In Project 1 are,

Chundong Wang <`wangchd` AT `shanghaitech.edu.cn`>  
Siting Liu <`liust` AT `shanghaitech.edu.cn`>  

and,

Linjie Ma <`malj` AT `shanghaitech.edu.cn`>  
Suting Chen <`chenst` AT `shanghaitech.edu.cn`>  
Luojia Hu <`hulj` AT `shanghaitech.edu.cn`>  

  
Last modified: 2024-03-19