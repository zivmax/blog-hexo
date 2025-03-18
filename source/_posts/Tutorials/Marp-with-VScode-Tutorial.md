---
title: Marp with VScode Tutorial for Anyone
mathjax: false
description: 基于 VScode 编写 Marp 幻灯片的工作流
date: 2024-06-24 15:51:14
tags:
- Marp
- Slides
category:
- 经验
---

# Marp with VScode Tutorial for Anyone

本文意在为任何人讲解如何使用 VScode 编写 Marp 幻灯片。

## Marp

Marp 是一个基于前端技术的 Slides 制作工具，它可以将 Markdown 文档渲染成幻灯片。

Marp 基于前端技术就意味着它渲染出来的幻灯片实际上是一个网页，而其渲染出来的 `pdf` 和 `pptx` 文件实际上都是通过 `html` 网页文件用浏览器内核渲染出来的。

这也是为什么 Marp 导出的 PDF 和 PPTX 文件中所有 Slide 都是纯图片，无法选中任何文本。

它最初是一个前端程序员圈子内的工具，因此有着强烈的前端技术气息。如果去看[官网的 Tutorial](https://marpit.marp.app/?id=how-to-use)，它会教你从 Node.js 的环境中安装 Marp CLI，写一份内嵌了 Markdown 语法的 JS 代码，然后使用 `marp` 命令来渲染 Markdown 文件。

但是对于大部分不接触前端技术的人来说，**使用 VScode 的 Marp 插件可能是一个更好的选择**。


## VScode

### 介绍
VScode 是一个文本编辑器（代码编辑器），它的本体就和 Windows 自带的记事本一样，只是一个文本编辑器。但是其基于前端技术开发，因此借助前端庞大的开发者群体，VScode 的插件生态非常丰富。因此完全可以通过安装和管理插件让 VScode 变成任何语言的 IDE。


### 安装
从[官网下载](https://code.visualstudio.com/#alt-downloads)并安装 VScode。

如果安装的电脑只会有一个人使用，个人推荐使用 System Installer 版本，便于管理 VScode 的软件本体，否则 Windows 会把 VScode 安装在用户目录下（如`C:\Users\USER\AppData\Local`）。

如果不清楚 x64 版本和 arm 版本的区别，那么简单来说，只要是 AMD 和 Intel 的 CPU 就用 x64。需要使用 arm 版本的人自会知道。

**总结就是：对于大部分人而言，去[官网下载](https://code.visualstudio.com/#alt-downloads) System Installer x64 版本。**


安装结束页面，对于`其他`选项的解释（红框内的选项）：

- 前两个选项：让右键菜单出现通过 VScode 打开的选项，第一个是右击文件出现，第二个是右击空白处出现
- 添加文件关联，注册默认应用
- 可以在终端中使用 code 命令行工具快速管理，启动 VScode

    ![VScode After Installation](https://s2.loli.net/2024/06/24/RvOaFKqHQJ2Vh1s.png)

第一次启动 VScode 界面大概如下：
![VScode GUI](https://s2.loli.net/2024/06/24/6O1hsSfuokzdHR5.png)


### 配置

打开在左边栏中的 Extension 板块

![VScode Extention](https://s2.loli.net/2024/06/24/qQb34dzTXVLMkZl.png)

搜索 `Marp for VScode`

![Marp for VScode](https://s2.loli.net/2024/06/24/z9wTXhIKblW2QFv.png)

图中第一个插件便是要安装的插件（`identifier: marp-team.marp-vscode`）

安装好之后，功能性上没有暂时没有更多需要配置的选项

> 如果需要中文支持，在 Extension 中搜索 `Chinese Simplified` 安装后
> 
> 使用快捷键 `Ctrl + Shift + P` 输入 `Configure Display Language`(可以模糊输入) 并回车后，切换为中文即可。
> 
> ![VScode Language Support](https://s2.loli.net/2024/06/24/wiOU7ohcL3VgRHA.png)
> 
> 或者似乎现在的 VScode 会自动添加语言支持。


## 正式编写

对于正式编写的工作流，便是建立一个项目（一份 Slides）专属的文件夹，VScode（包括绝大多数代码开发工具）都需要在一个独立的文件夹中进行使用。

作为演示：

1. 在桌面创建文件夹 `marp-example/`
2. 用 VScode 打开这个文件夹
   - 如果你添加了`通过 Code 打开` 在右键菜单，那么你可以直接右键文件夹图标，通过 Code 打开。
   - 如果没有的话，首先普通地打开 VScode，点击左上角的 File > Open Folder
    ![VScode Open Folder](https://s2.loli.net/2024/06/24/WuhTU8IaLNsjq2C.png)
   - 或者按照图中显示的快捷键，`Ctrl + K` （此时 VScode 进入一种激活状态）然后再 `Ctrl + O`
3. 打开文件后，理论上 File Explorer 会自动展开
    ![](https://s2.loli.net/2024/06/24/TLmpEBVeqNlO3ZC.png)
4. 可以看到 `MARP-EXAMPLE` 是被打开的文件夹名称，Explorer 中显示的文件都是这个文件夹中的文件
5. 现在创建一个新的文件，鼠标放在图中 New File 的位置即可
    ![](https://s2.loli.net/2024/06/24/qHGopjVtAwRuCMJ.png)
6. 输入完整的文件名，包括后缀，然后回车即可
    ![](https://s2.loli.net/2024/06/24/ToXGyQmV7d3UtuF.png)
7. 接着打开刚刚新建的文件，现在我们就完成正式编写前的所有操作了。

以上流程在每一次编写一份新的 Slides 都推荐进行：建立新的文件夹，用 VScode 打开，创建文件……

### Markdown

前文提到，Marp 使用 Markdown 作为编写语言进行解析和渲染，最后得到渲染好的 HTML 文件。

因此首先需要学习 Markdown 的基础语法。

Markdown 是一种用来进行简单（简洁）排版的文本，它起源于前端程序员手动编写 HTML 页面的痛苦。

后来随着网络发展，逐渐扩展到了很多普通人，用来编写在网络上被浏览的文档。

以下是最常用的 Markdown 语法，将它复制到我们刚刚创建的新文件中，并打开 VScode 自带的预览，即点击右上角有一个带有放大镜的图标（旁边的重叠三角形来自安装的 Marp VScode 扩展）：

```md
不同数量的 `#` 可以完成不同的标题，如下：

# 一级标题

## 二级标题

### 三级标题


`---` 是分割线

---

# 字体

粗体、斜体、粗体和斜体，删除线，需要在文字前后加不同的标记符号。如下：

**这个是粗体**

*这个是斜体*

***这个是粗体加斜体***

~~这里想用删除线~~

# 列表

无序列表的使用，在符号 `-` 后加空格使用。如下：

- 无序列表 1
- 无序列表 2
- 无序列表 3

如果要控制列表的层级，则需要在符号 `-` 前使用空格。如下：

- 无序列表 1
- 无序列表 2
  - 无序列表 2.1
  - 无序列表 2.2

有序列表的使用，在数字及符号 `.` 后加空格后输入内容，如下：

1. 有序列表 1
2. 有序列表 2
3. 有序列表 3

事实上此语法关键在于数字加上 `.`，最后渲染的结果的编号为自动决定的

3. 有序列表 1
2. 有序列表 2
1. 有序列表 3

# 链接

微信公众号仅支持公众号文章链接，即域名为`https://mp.weixin.qq.com/`的合法链接。使用方法如下所示：

对于该论述，欢迎读者查阅之前发过的文章，[你是《未来世界的幸存者》么？](https://mp.weixin.qq.com/s/s5IhxV2ooX3JN_X416nidA)

# 图片

插入图片，格式如下：

![这里写图片描述](https://s2.loli.net/2024/06/24/6O1hsSfuokzdHR5.png)

图片描述的意义是如果图片加载不出来，就用图片描述替代，不过其具体作用取决于具体 Markdown 渲染器的处理。

支持 jpg、png、gif、等图片格式

---

# 引用
引用的格式是在符号`>`后面书写文字。如下：

> 读一本好书，就是在和高尚的人谈话。 ——歌德

> 雇用制度对工人不利，但工人根本无力摆脱这个制度。 ——阮一峰



```

VScode 的 Markdown 预览为实时预览，可以随意修改内容并看见最终渲染结果。

同时图片也不一定要是图片的云端链接，可以写本地图片的路径，例如 example.md **同一个文件夹下**有一个 image 文件夹，里面有一张 test.jpg 图片
```md
![](image/test.jpg)
```


### Marpit

以上的 Markdown 是其用来编写文档的用法，对于使用这种语法来编写 Marp Slides，官方称其为 [Marpit Markdown](https://marpit.marp.app/markdown)。

将以下 Markdown 代码复制粘贴到 `example.md` 文件中，你会发现已经变成了 Slides。 

```md
---
marp: true
---

# Slide 1

foo

---

# Slide 2

bar
```

显然， Marpit Markdown 有两个核心要点：
1. 开头用 `---` 包裹的部分，激活了 Marp，这个部分也可以有更多进一步的总体设置。
2. 不同的 Slide 之间使用 `---` 分割。

除了上述提到的核心要点，其他的写法与普通的 Markdown 文档区别不大。

```md
---
marp: true
---

# 有序列表

1. 有序列表 1
2. 有序列表 2
3. 有序列表 3

---

# 引用
引用的格式是在符号`>`后面书写文字。如下：

> 读一本好书，就是在和高尚的人谈话。 ——歌德

> 雇用制度对工人不利，但工人根本无力摆脱这个制度。 ——阮一峰

---

# 一级标题

## 二级标题

### 三级标题

---

# 字体

粗体、斜体、粗体和斜体，删除线，需要在文字前后加不同的标记符号。如下：

**这个是粗体**

*这个是斜体*

***这个是粗体加斜体***

~~这里想用删除线~~

```

### Marp 全局配置

前文提到，我们可以在开头的被包裹部分提供更加进一步的全局设置，分别试试下面几种配置：

```mdx
---
marp: true
theme: default 
paginate: false
---
```

```mdx
---
marp: true
theme: default 
paginate: true
---
```

```mdx
---
marp: true
theme: uncover 
paginate: true
header: 'Header content'
footer: 'Footer content'
---
```

```mdx
---
marp: true
theme: uncover 
class: invert
paginate: true
header: 'Header content'
footer: 'Footer content'
---
```

### Marp 内嵌 HTML

然而，多加尝试就会发现，Marp 的便利性和排版需求，有的时候很难匹配具体的情况，举一个最简单的例子，Marp 中无法将多个列表并排放置。除此以外，对图片大小的精细调整也是一个难题，虽然对于图片 Marpit 已经做出了一些[拓展语法](https://marpit.marp.app/image-syntax)

前文不断提到，Markdown，Marp 均为前端技术，他们都会被编译成 HTML，再让浏览器渲染 HTML 得到画面。

因此我们实际上也可以直接内嵌 HTML 在 Markdown 或者 Marpit Markdown 中。而 HTML 作为一切网页的基础，可以实现的排版效果是近乎无穷的。

在开始尝试使用内嵌 HTML 之前，首先要去设置。
![VScode Open settings](https://s2.loli.net/2024/06/24/VbKMeiPE49l6JWI.png)

搜索，并打开 Marp VScode 插件的 HTML 开关（默认为关闭）
![Open Marp HTML](https://s2.loli.net/2024/06/24/L1Sw93fudvEZ4F8.png)

然后可以尝试以下的 Slide 代码

- 并列列表
    ```md
    ---

    # 4. Evaluating Histograms

    <div style="display: flex; justify-content: space-around; margin-top: 30px;">

    <div>

    -  Skewness and Tails
        - Skewed left vs skewed right
        - Left tail vs right tail

    </div>

    <div>

    -  Outliers
        - Using percentiles

    </div>

    <div>

    -  Modes
        - Most commonly occuring data

    </div>

    ---
    ```

- 有序列表并列分割
    ```md
    ---

    <style>
        .two-columns {
        display: flex;
        justify-content: center; /* Center the columns */
        margin-left: 100px;
    }
    .two-columns .column {
        flex: 1;
        padding: 10px 10px;
    }
    .two-columns ol {
        padding-left: 0;
        list-style: none;
    }

    </style>

    <div class="two-columns">
    <ol class="column">
        <li>引言</li>
        <li>概念辨析</li>
        <li>假设前提</li>
        <li>在传授知识方面的优势</li>
        <li>在给予学习反馈方面的优势</li>
    </ol>
    <ol class="column" start="6">
        <li>在安全性方面的优势</li>
        <li>在社会成本方面的优势</li>
        <li>面临的挑战</li>
        <li>结论</li>
    </ol>
    </div>

    ---
    ```

- 图文对应

    ```md
    ---

    # 4. Evaluating Histograms
    ## 4.1 Skewness and Tails
    <div style="display: flex; justify-content: space-around; margin-left: -150px;">

    <div style="margin-right:-300px;">

    <div style="text-align:center;"> 

    *Left Skew and Right Tail:*

    </div>

    ![w:500](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-19-output-2.png)

    </div>

    <div style="margin-right:-150px;">

    <div style="text-align:center;">

    *Right Skew and Left Tail:* 

    </div>

    ![w:500](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-20-output-2.png)

    </div>

    </div>

    ---
    ```

- 插入视频
    ```md
    ---
    
    <video controls= "controls" width="100%" src="https://www.shanghaitech.edu.cn/_upload/article/videos/b3/6a/50b2c11c4d188c4a7b392b74bb28/0428a130-e35a-4584-9684-f70000162e38-B.mp4"></video>

    ---
    ```

**本文的标题中写到，“for Anyone” 因此这里自然不会过多涉及前端技术，而之所以这些可以 “for Anyone”，是因为我们完全可以让生成式人工智能（如 ChatGPT）来完成我们具体的详细排版工作，关键在于至少知道一些关键词： `请使用内嵌 HTML 的方式为我完成这页 Marp Slide，要求是……`。**

不过其实单纯这里提供的 HTML 模板，加上[官方的一些图片排版语法](https://marpit.marp.app/image-syntax)也已经足够应对绝大多数的排版需求了。

### Marp for VScode 导出

上文介绍了大量 Marp 的编写，但是最终的目的是为了导出为 Slides 使用。

对于已经编写好并且确认要导出的 Marp Slides

- 可以使用快捷键 `Ctrl + Shift + P`，输入 `Marp: Export Slides`(可以模糊输入)，然后选择导出的格式。
- 或者点击右上角的重叠三角形图标，选择 `Export Slides Deck...` 选项，然后选择导出的格式。

对于导出的格式，最佳的格式自然是 HTML，尤其是 Slides 中包含一些互动，动态内容：gif 图，视频插入等。因为对于其他格式，如 PDF，PPTX，本质就是用浏览器内核渲染 HTML 文件得到图片，然后再组合成 PDF 或 PPTX 文件。

不过要注意，如果使用的是本地图片，那么导出的 HTML 文件中的图片链接是本地的，使用的时候，要保证图片文件和 HTML 文件的相对位置关系和导出时 Markdown 文档的相对位置关系一致。而 PDF 和 PPTX 文件则会将图片嵌入其中，因此不会有这个问题。


### 使用导出结果

对于 PDF 和 PPTX 文件，按照正常的文件使用方式即可。

而对于 HTML 文件，则用浏览器打开，并且如上文所说，如果使用了本地图片，要保证图片文件和 HTML 文件的相对位置关系和导出时 Markdown 文档的相对位置关系一致。

注意打开的页面是一个网页，中间的工具栏在鼠标不靠近一会后会自动隐藏。

全屏按钮旁边的按钮可以切换到 Presenter 模式，Presenter 模式和 PPT 的 Presenter 模式类似，重点在于 Presenter Note，也就是备注，下一节 Section 将讲解。

![](https://s2.loli.net/2024/06/24/vfVSAxzDWBdyO4H.png)

### 添加备注

要添加备注，只需要在 Markdown 文档中使用注释（源代码被执行，源代码中的注释不被执行，是用来给写代码的人看的）。

注释的格式为 `<!-- 注释 -->`，可以换行。

```md
---

# AI 替代初高中人类教学面临的挑战

## 学习动机和集体秩序的维持

  - **缺乏社会角色定位和地位**：
    - 作为一种非具身工具，其社会角色模糊，难以赢得学生的角色认同

  - **社会化过程的参与**：
    - 作为非社会性存在
    - 无法有效参与和影响学生的社会化过程

<!-- 

正如本文中的评价前提所述，教学的评价还取决于维持学习动机和维持集体秩序这两个维度。

这其中最核心的原因是， AI 缺乏明确的社会角色定位和地位。
人类社会是一个复杂的系统，个体在其中扮演特定的社会角色，并被赋予相应的权力、责任和义务。

教师作为知识和道德的传播者，在学生心目中具有崇高的地位。
而 AI 作为一种非具身工具，其社会角色模糊，难以赢得学生的角色认同，因此 AI 的言行很难对学生产生应有的影响力。

再次，学习动机和集体秩序的维持本质上是一个社会化的过程。
学生的学习动机不仅来自求知欲，更来自于荣誉感、责任感、集体认同感等社会情感的驱动。

AI 作为非社会性存在，无法参与和影响学生社会化过程，因此难以在维持学习动机和集体秩序方面发挥关键作用。

-->

---
```

给某页 Slide 添加注释后再导出，就可以在 Presenter View 的对应页面中看见备注。

而导出的 PPTX 中也同样自动添加了备注。


## 最后

Enjoy😎.
