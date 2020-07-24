---
layout:     post   				    # 使用的布局（不需要改）
title:      Neural Network				# 标题 
subtitle:   basic concepts and C implementation
date:       2020-07-09 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    cloud_computing
---
This article will introduce some very basic concepts of neural network and implement a simple single-layer
neural network in C. 

## What is neural network 
Neural network is basically a statistical classification or regression model, and nothing more. Like other models like
Support Vector Machine (SVM) and linear regression, with a training dataset, a model can be created. Based on the 
model, whenever a new sample is given, the new sample will be fed in the model and a prediction will be made. 

Of course, comparing with other statistical models, neural network have its own pros and cons: 
#### Pros: 
1. Neural network is very flexible, thus it can be used in many different scenarios including both linear and nonlinear
relations; 
2. Almost no assumptions for data are required for neural network. For instance, linear discrimination analysis is
commonly used to classify data, however the distribution of its input data should be normal (gaussian), otherwise it
is incorrect. However for neural network, such assumptions are not required; 
3. Neural network enjoys a great predicting ability, or we can say, it is generalizable. 

#### Cons: 
1. Building a neural network is costly in computation; 
2. It is always hard to explain neural network models how they perform well. In some real life, it is often not enough
to do prediction only. More importantly, one need to clarify why the prediction is correct. Neural network always lack
such ability (interpretability); 
3. Flexibility is also a con for neural network: there are so many different architectures and functions to choose from
to build a neural network for a specific problem, so it is always a problem: how to build a proper network for the
facing problem. 

## Architecture
Neural network is constructed by layers. Each layer is basically an operator, with 3 elements, weight matrix $W$, 
bias term $B$ and an activation function $\sigma$. The input of layer $i$ is actually the output of previous layer 
$i-1$. Data flow into network layer by layer (operated by a series of operators) and finally networks will produce an
output matrix. To quantify how biased the output is away from target output, error $E$ is also defined by an error 
function, taking output matrix and target output matrix as arguments. 

If you google "neural network keras", tons of example codes will be found, and you could easily "build your own neural
network" with keras API within several lines. However, for a guy who is studying in SCIENCE, rather than calling some 
high-level functions in which you never know what is going on, mathematics always makes me feel safe about what I am 
doing. To make our lives easier, a simple fully-connected network will be explained in mathematical details. One 
can easily and the simple one into more complex network. 

Following notations will be used:
![avatar](/img/20-07-09/notation.png)
#### Forward propagation
How neural network is dealing with input data is basically represented as following equation: 
![avatar](/img/20-07-09/NNequation.png)

The inner part $\sigma^1(W^1\times X + B^1)$ is the output of 
the first layer: the input matrix is multiplied by the weight matrix of the 1st layer and also the bias matrix is 
added. Then with the weighted matrix $Z^1$, the activation function will operated on $Z^1$ element-wise and
finally output matrix $A^2$ is obtained, which has the same dimension $m\times n^1$ as matrix $Z^1$. 
A Small point should be paid attention to that the dimension of mat $W\times X$ is $m\times n^1$, while the dimension
of bias term is $1\times n^1$. Commonly matrices with different sizes are unable to be added, however here it is
actually not ordinary sum: the bias vector $B$ is added to each row of matrix $W\times X$ row-wise. Therefore the 
output matrix of the 1st layer $A^2$ is of dimension $m\times n^1$. 

Likewise, $A^2$ is fed into the 2nd layer and multiplied then added by $W$ and $B$ respectively. Finally with
element-wise operation be activation function, the output matrix $A^3$ will be obtained with dimension of 
$m\times n^2$. Here we can see that the output of layer $i$ is of dimension $m\times n^i$: the column number of matrix
is varying according to the number of nodes of each layer, while the number of rows keeps the same, which is the number
of input samples. The dimension is actually change by the weight matrix $W^i$ of layer $i$. 

For the last layer, things are a bit different: the number of nodes should be fixed as the dimension of target output. 
To be more precise, for single-value regression task, there should be only one node in the last layer, and for 
multi-class classification, the number should be the number of categories of output. This ensure the dimension of
output matrix keeps the same with target matrix: $m\times t$. 

The whole procedure is actually propagating data from the top of the network layer by layer, or we can call this
forward propagation. 

#### Error definition
To compare how far the output matrix is away from target output, error function is defined to get a scalar named 
error $E$. Here mean square error is used:
![avatar](/img/20-07-09/Errorfunc.png)

#### How to learn? 
Like other statistical models, the "training" of neural network is actually the process of looking for $W$s and $B$s 
that minimize the error $E$. Ideally, we need to find every $W$ that makes $\partial E/\partial W=0$ and $B$ 
that makes $\partial E/\partial B=0$. However in this problem, it is almost impossible to do so for 2 reasons: 
1. most neural network are multi-layer, which makes it very hard to find the equation of $\partial E/\partial W$ 
for every $W$ (and $B$s of course). 
2. The variables we are facing are all matrices but not scalar, which makes it far more complex to find 
deviation functions. Therefore, gradient descending is introduced. 

The ideology of gradient descending is actually modify $W$s and $B$s step be step: in every iteration, the deviation
matrix of each $W$ and $B$ at the specific point will be calculated, which represent how $E$ will change if every
element of $W$ or $B$ increase by 1: will $E$ increase or decrease and how much will it change (actually this is
the meaning of deviation xD). Then with a negative learning rate $\text{lr}$, matrices $W$ is updated as 
$W-lr\times \partial E/\partial W$ and $B := B-lr\times \partial E/\partial B$, which ensure $E$ is decreased at 
each step. 

It is easy to specify a learning rate, now the problem is the deviation of $W$ and $B$: how can we get them? 
Let us look back how $E$ is obtained from $W$s and $B$s: 
![avatar](/img/20-07-09/A_Z_A.png)
We can see that $E$ is the function of $A^{L+1}$, which is the function of $Z^L$, which is the function of $W^L$ and 
$B^L$... Chain rule can be used to differentiate $E$ on $W$ and $B$ as following: 
![avatar](/img/20-07-09/Chain.png)

Lt's check the deviation for $W^L$ first. The deviation of $E$ for $W$ should be a matrix of $n^{L-1}\times t$, which 
should be the same as the dimensionality of $W$. Firstly, the deviation of $E$ for $A^{L+1}$ is a matrix of 
$m\times t$ and it is very easy to calculate by substracting $Y$ from $A^{L+1}$ element-wise and divided by 
$m\times t$. Deviation of $A^{L+1}$ for $Z^L$ is also simple: because activation function is done on matrix $Z^L$ 
element-wise as well, so this step can also be done by element-wise calculation. So far the dimension of deviation 
matrix of $E$ for $Z^L$ is $m\times t$. Further, we know that $W^L\times A^{L-1} + B^L = Z^L$, so the deviation of 
$Z^L$ for $W^L$ is matrix $A^{L-1}$ with dimension $m\times n^{L-1}$. How can two matrices 
($\partial E/\partial Z^L$ and $\partial Z^L/\partial W^L$) with dimension of $m\times t$ and $m\times n^{L-1}$ 
multiplied and get the matrix $\partial E/\partial W^L$ of dimension $n^{L-1}\times t$? YES! 
$\partial E/\partial W^L = (A^{L-1})^t*\partial E/\partial Z^L$. 

For the second deviation for $B^L$, the matrix should be of dimension $1\times t$, however, $\partial E/\partial Z$
is a matrix of $m\times t$, and $\partial Z/\partial B$ is a constant 1. If multiplied, the deviation matrix would 
be of dimension $m\times t$. How can we deal with this? Actually it is easy to be handled if we think a bit more. This 
$m\times t$ matrix basically means for each sample, how large E will be if $B$ is increased by 1. Because vector
$B$ was added to $W*A$ row-wise, of course here we will get multiple "version" of deviation on $B$. To keep it 
consistent, all we need to do is simply do an column-wise average on matrix $\partial E/\partial B$ and reduce the 
$m\times t$ matrix to a $1\times t$ vector! 

So far, we have already done deviation for $W^L$ and $B^L$, how can we do deviation for other $W$s and $B$s in an
easier way? 

#### Delta
You might have noticed that both deviation of E on $W^L$ and $B^L$ share a common part: $\partial E/\partial Z^L$. 
If we pick this variable out and define it as $\Delta ^L$, which represents the error distributed on layer $L$, life
will be easier: we can easily obtain deviation of E for $W^L$ and $B^L$ from $\Delta ^L$. Also we can compute 
$\Delta ^{i-1}$ from $\Delta ^{i}$ easily with just some proofFollowing are 4 important equations in back 
propagation. 
![avatar](/img/20-07-09/bp4.png)
Here finally we have the term "back propagation". Personally I assume this term is demonstrating that the error $E$
is passed with $\partial E/\partial Z^i$ layer by layer backwards and distributed to each node, just like an 
opposite way forward propagation did. 

#### Training
In each iteration, sample data are fed into network by forward propagation and finally error $E$ is obtained. 
Then with back propagation error is passed backwards and all $W$s and $B$s get updated. After numerous iterations, 
finally $W$s and $B$s will get converged and the model is ready. This is what we called "training of neural network". 

## Code
The complete code and Makefile can be founds 
[here](https://github.com/Li-Ju666/FedML/tree/master/w1/C_implementation/Matrix-based_implementation). 


