---
title: DATA100 Note [3]
mathjax: true
description: Introduction to EDA and Data Wrangling.
date: 2024-03-26 18:31:28
tags:
- DATA100
- 计算机科学
- 数据科学
category:
- 学习
- 笔记
---

# **EDA and Data Wrangling**


Unboxing of New Data Set



# 1. Unboxing of New Phone

想象刚刚到货了一个新的手机，一种可能的流程是：

*开机 $\to$ 随便试试 $\to$ 发现某个设置不顺手 $\to$ 调整设置 $\to$ 重新试试 $\to$ 发现某个设置不顺手 $\to$ 调整设置 $\to$ ......* 

![bg left:33%](https://qnam.smzdm.com/202010/23/5f92537c5b6b1461.jpg_e1080.jpg)


# 2. Unboxing of New Data Set

刚刚得到了一个新的数据集:

![bg left:33%](https://img1.baidu.com/it/u=501391342,1436836699&fm=253&fmt=auto&app=138&f=JPEG)

*Load Data $\to$ EDA $\to$ 发现不对劲 $\to$ Wrangling $\to$ EDA $\to$ 发现不对劲 $\to$ Wrangling $\to$ ......*


# 3. EDA (Exploratory Data Analysis) and Wrangling

It is an open-ended, informal analysis paradigm.

It majorly focuses on:

<div style="margin-left:100px">

1. ***Structure***

1. ***Granularity, Scope, and Temporality***

1. ***Faithfulness***

</div>


# 3.1 Data's Structure
<br>

<div style="display: flex; justify-content: space-around; margin-left: -200px;">

<div style="margin-right: -300px;">

- **Format:**

  - *CSV* 
  - *TSV* 
  - *Json* 
  - $\vdots$

</div>

<div>

- **Variable Type:** 

  - *Quantitative*
    - *Continuous*
    - *Discrete*
  - *Qualitative* 
    - *Ordinal* 
    - *Nominal* 

</div>

</div>


# 4. Data's Structure
## 4.1 File Format: csv, tsv, json ...
```csv
'Year,Candidate,Party,Popular vote,Result,%\n'
'1824,Andrew Jackson,Democratic-Republican,151271,loss,57.21012204\n'
```

```tsv
'\ufeffYear\tCandidate\tParty\tPopular vote\tResult\t%\n'
'1824\tAndrew Jackson\tDemocratic-Republican\t151271\tloss\t57.21012204\n'
```

```json
[
 {
   "Candidate": "Andrew Jackson",
   "Party": "Democratic-Republican",
   "Popular vote": 151271,
 },
]
```



## 4.2 Metadata
```json
[
  {
    "Meta": {
      "Size": "1.2MB",
      "Date": "2021-10-01",
    },
    "Data": [
      {
        "Candidate": "Andrew Jackson",
        "Popular vote": 151271,
      }, // ... more data section
    ]
  }
]
```




## 4.3 Variable Type

![](https://ds100.org/course-notes/eda/images/variable.png)


# 5. Data's Granularity, Scope, and Temporality

- **Granularity:** How detialed is the data about an indivisual?

- **Scope:** How well the samples cover the target population? 

- **Temporality:** How timely is the data?


# 6. Data's Faithfulness
## 6.1 Signs that data may not be faithful

- **Unrealistic or “incorrect” values**
- **Violations of obvious dependencies**
- **Clear signs that data was entered by hand**
- **Signs of data falsification**
- **Duplicated**
- **Truncated data**



## 6.2 Missing Values (Abnormal Values)


- Many abnormal values are actually just missing values.


<div style="margin-left:200px">

![h:400 ](https://ds100.org/course-notes/eda/eda_files/figure-html/cell-62-output-1.png)

</div>




## 6.2 Missing Values (Abnormal Values)

Three typical ways to deal with missing values: ***Drop***, ***NaN***, and ***Impute***.

![](https://ds100.org/course-notes/eda/eda_files/figure-html/cell-75-output-1.png)


# 7. Summary

- **Data Overview**: Assess data's date, size, organization, and structure.
- **Individual Analysis**: Investigate each field/attribute/dimension.
- **Pairwise Analysis**: Explore relationships between dimensions.


- **Along the way, we can:**
    - **Visualize**
    - **Validate assumptions**
    - **Address anomalies**
    - **Document everything** (*Ideally using Jupyter Notebook*)

