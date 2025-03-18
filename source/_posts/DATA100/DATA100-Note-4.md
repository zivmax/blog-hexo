---
title: DATA100 Note [4]
mathjax: true
description: Introduction to Visualization.
date: 2024-04-3 18:31:28
tags:
- Unfinished
- DATA100
- 计算机科学
- 数据科学
category:
- 学习
- 笔记
---

# **Visualization**

Encoding Information into Intuition


# 1. Goals of Visualization

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-19-output-1.png)

- To broaden your understanding of the data. 

- To communicate results/conclusions to others.

Altogether, these goals emphasize the fact that ***visualizations aren’t a matter of making “pretty” pictures***.




# 2. An Overview of Distributions

![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-10-output-1.png)

-  The total frequency of all categories must sum to 100%

-  Total count should sum to the total number of datapoints if we’re using raw counts.

Most of the time, we're ploting the distribution of the data.



# 3. Variable Types Inform Plot Choice

Recall the types of the variable:

![](https://ds100.org/course-notes/visualization_1/images/variable_types_vis_1.png)


## 3.1 Qualitative Variables: Bar Plots
```python
import seaborn as sns # seaborn is typically given the alias sns
sns.countplot(data = wb, x = 'Continent')
```



![ ](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-5-output-1.png)




## 3.2 Quantitative Variables: Box, Violin and Hist

### Box
```python
sns.boxplot(data=wb, y='Gross domestic product: % growth : 2016');
```


![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-11-output-1.png)





### Violin

```python
sns.violinplot(data=wb, y='Gross domestic product: % growth : 2016');
```


![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-12-output-1.png)






### Histograms
```python
sns.histplot(data=wb, x="Gross n...", stat="density")
```

![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-15-output-1.png)




### Overlap

```python
sns.boxplot(data=wb, x="Continent", y='Gross n...')
sns.histplot(data=wb, x="Gross n...: 2016", hue="Hemisphere", stat="density")
```



![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-13-output-1.png)





![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-17-output-1.png)




# 4. Evaluating Histograms



-  Skewness and Tails
    - Skewed left vs skewed right
    - Left tail vs right tail



-  Outliers
    - Using percentiles



-  Modes
    - Most commonly occuring data





## 4.1 Skewness and Tails


### Left Skew and Right Tail


![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-19-output-2.png)





### Right Skew and Left Tail



![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-20-output-2.png)




## 4.2 Outliers

Loosely speaking, an outlier is defined as a data point that ***lies an abnormally large distance*** away from other values.




## 4.3 Modes

*We describe a “mode” of a histogram as a peak in the distribution.*

![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-22-output-1.png)




## 4.4 Challenge
*In this image, it's hard to observe. It is these ambiguities that motivate us to consider using **Kernel Density Estimation (KDE)***

![](https://ds100.org/course-notes/visualization_1/visualization_1_files/figure-html/cell-23-output-1.png)




# 5. KDE (Kernel Density Estimation)

- *A kernel density estimate (KDE) is a smooth, continuous function that **approximates a curve**.*

- *More formally, a KDE attempts to **approximate the underlying probability distribution** from which our dataset was drawn.*

![ ](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-3-output-2.png)



## 5.1 Constructing KDE
1. Place a kernel at each datapoint.

1. Normalize the kernels to have a total area of 1 (across all kernels).
1. Sum the normalized kernels.



Assume we want KDE this dataset: **[ 2.2, 2.8, 3.7, 5.3, 5.7 ]**


![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-4-output-1.png)






***Step 1: Place a kernel at each datapoint.***


![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-6-output-1.png)


![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-7-output-1.png)



***Step 2: Normalize the kernels to have a total area of 1.***

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-8-output-1.png)



***Step 3: Sum the normalized kernels.***

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-9-output-1.png)





## 5.2 Kernel Functions and Bandwidths

A general “KDE formula” function is given bello
$$
f_{\alpha}(x) = \frac{1}{n} \sum_{i=1}^{n} K_{\alpha}(x, x_i)
$$

, which is pretty much like the ***convolution***.



$$
f_{\alpha}(x) = \frac{1}{n} \sum_{i=1}^{n} K_{\alpha}(x, x_i)
$$

1. $K_{\alpha}(x - x_i)$ is the kernel centered on the observation `i`.

1. $n$ is the number of observed datapoints that we have.

1. Each $x_i \in \{x_1, x_2,\dots, x_n \}$ represents an observed datapoint.



*The most common kernel is the **Gaussian kernel**.*

$$
K_{\alpha}(x, x_i) = \frac{1}{\sqrt{2\pi\alpha^2}} e^{-\frac{(x-x_i)^2}{2}}
$$




$$
K_{\alpha}(x, x_i) = \frac{1}{\sqrt{2\pi\alpha^2}} e^{-\frac{(x-x_i)^2}{2}}
$$

1. $\alpha$ is the bandwidth of the kernel, which is the ***standard deviation***.

2. $x_i$ is the center of the kernel, which is the ***mean***.



***Gaussian kernel KDE with bandwiths: $0.1$; $1.0$; $2.0$; $10.0$:***

  ![](https://ds100.org/course-notes/visualization_2/images/gaussian_0.1.png)

  
  ![](https://ds100.org/course-notes/visualization_2/images/gaussian_1.png)
  
  
  ![](https://ds100.org/course-notes/visualization_2/images/gaussian_2.png)
  

  ![](https://ds100.org/course-notes/visualization_2/images/gaussian_10.png)




# 6. Multi Quantitative Variables

- Up until now, we’ve discussed how to visualize single-variable distributions. 

- Going beyond this, we want to understand **the relationship between pairs of numerical variables**.



## 6.1 Scatter Plots
```python
plt.scatter(wb["per c..."], wb['Adult l...'])
```

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-14-output-1.png)



## 6.1 Scatter Plots

*But this seems overplotting...*

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-14-output-1.png)



## 6.1 Scatter Plots

We can ***shrink the marks*** and ***add random shifting noise***.

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-16-output-1.png)




## 6.2 Linear Plots

```python
sns.lmplot(data = wb, x = "per c...", y = "Adult l...")
```

![ ](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-17-output-2.png)





## 6.3 Joint Plots

```python
sns.jointplot(data = wb, x = "per c...", y = "Adult l...")
```

![ ](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-18-output-1.png)




## 6.4 Hex Plots

```python
sns.jointplot(data = wb, x = "per c...", y = "Adult l...", kind = "hex")
```

![ ](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-19-output-1.png)





## 6.4 Hex Plots

Hex plots can be thought of as ***two-dimensional histograms*** !

![ ](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-19-output-1.png)





## 6.5 Contour Plots

Contour plots can be thought of as ***two-dimensional KDE*** !

```python
sns.kdeplot(data = wb, x = "per c...", y = "Adult l...", fill = True)
```

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-20-output-1.png)





# 7. Transformation

As said before, we want to ***reveal the relationships***. 

However, relying on plotting directly alone is limiting, ***not all plots show association***.




*Consider the following plot.*

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-21-output-1.png)




We can try applying transformation !

![](https://ds100.org/course-notes/visualization_2/images/linearize.png)



## 7.1 Making Transformation

*Step 1: Observe the plot*

![ ](https://ds100.org/course-notes/visualization_2/images/horizontal.png)




*Step 2: Transform on X axis*

```python
plt.scatter(np.log(df["inc"]), df["lit"])
```

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-22-output-1.png)





*Step 3: Transform on Y axis*

```python
plt.scatter(np.log(df["inc"]), df["lit"]**4)
```

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-23-output-1.png)





*Step 4: Linear regression*

```python
from sklearn.linear_model import LinearRegression # Discuss in the future
```

![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-24-output-2.png)




## 7.2 Inference reversely

$$
y^4 = m(\log{x}) + b \quad \to \quad y = [m(\log{x}) + b]^{\frac{1}{4}}
$$


![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-24-output-2.png)


![](https://ds100.org/course-notes/visualization_2/visualization_2_files/figure-html/cell-25-output-1.png)




## 7.3 Tukey-Mosteller Bulge Diagram

This diagram is a good guide when determining possible transformations.

![](https://ds100.org/course-notes/visualization_2/images/tukey_mosteller.png)



![](https://ds100.org/course-notes/visualization_2/images/bulge.png)



# 8. Visualization Theory

Remember, we had two goals for visualizing data. *Visualization Theory* is particularly important in:

- Helping us understand the data and results,
- Communicating our results and conclusions with others.




## 8.1 Information Channels

Visualizations are able to convey information through various encodings. 

*Except things in the image, **marks' relative position** is also a important channel.*

![bg left:40%](https://ds100.org/course-notes/visualization_2/images/markings_viz.png)

Each visulization should **at least contains one accurate channel**.

*Thus don't use pie chart any more!*

## 8.2 What is Good Encoding?

- *No wrong information encoded*
  - Example: Are abortion and cancer related?

![❌](https://ds100.org/course-notes/visualization_2/images/wrong_scale_viz.png)

![✔️](https://ds100.org/course-notes/visualization_2/images/good_viz_scale_1.png)




- *No redundent infomation encoded*
  - Example: How cases changed during Mar, 21,2020 to May, 6, 2020?

![❌](https://ds100.org/course-notes/visualization_2/images/unrevealed_viz.png)



![✔️](https://ds100.org/course-notes/visualization_2/images/revealed_viz.png)




- *Encode information linearly*
  - Example: Shows the numerical strength distribution in 2D.


![❌](https://ds100.org/course-notes/visualization_2/images/jet_colormap.png)



![✔️](https://ds100.org/course-notes/visualization_2/images/viridis_colormap.png)





- *Encode information linearly*
  - Larger number show be mapped to higher gray scale color.

![❌](https://ds100.org/course-notes/visualization_2/images/jet_perceptually_uniform.png)

![✔️](https://ds100.org/course-notes/visualization_2/images/viridis_perceptually_uniform.png)



# 9. Summary

Good visualizations are always made by ***Intuitive  and Empathetic Person***
