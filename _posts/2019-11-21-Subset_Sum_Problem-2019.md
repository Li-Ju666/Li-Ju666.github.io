---
layout:     post   				    # 使用的布局（不需要改）
title:      Subset Sum Problem				# 标题 
subtitle:   present problem #副标题
date:       2019-11-21 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    algorithm
---


## Subset Sum Problem
This is the problem in the assignment 1 of Course Algorithm and Data Structure 2. Here are my solutions.  
### Problem Description
> Given a set P of prices for items in a gift shop, as well as a total amount t of money collected by a group of people towards buying birthday presents for a common friend, 
the birthday present problem is to determine whether there exists a subset P'![](http://latex.codecogs.com/gif.latex?%5Csubseteq)P whose sum is exactly t, since we do not want to distribute any excess money back to the donors. For example, if P={1, 7, 2, 32, 56, 35, 234, 12332} and t = 299, then the subset P'={2, 7, 56, 234} is a solution, but there is no solution for t = 11. Perform the following tasks:

>Give a recursive equation for the required Boolean, including the semantics of all the parameters. Design and implement an efficient dynamic programming algorithm for the birthday present problem as a Python function birthday present(P, n, t) that returns True and the chosen subset if and only if there exists a subset P' with sum t ≥ 0 of the set P, which is given as an array of n non-negative integers.If there is not a valid subset, return False and an empty list. Analyze the complexity of your algorithm in the worst case. 

### Problem Solution
To solve the problem, following points need to be thought carefully: 

1. As a dynamic programming-related problem, what is the substructure of the problem? In another word, how to convert the whole problem into the same problem with smaller sizes of input? 

Lets assume an simple case, P={1,2,3,4} and t=7. For every element (for instance: 4) in P, there are only two cases: the element is included in the subset P', or not. If included, the next step is to solve the (exactly) the same problem, in which P={1,2,3} and t=3. If not, the problem remained is to solve: P={1,2,3} and t=7. If any of the two situations is true, then the answer of the whole problem is true. Then we comes the recursive function: subset_problem(P, t) = subset_problem(P-{P[n]}, t) or subset_problem(P-{P[n]}, t-P[n]), where P is the given list, t is the given sum and n indicates that the nth elements in P is being considered if to be chosen or not. 

Since the recursive function (also known as OPT in dynamic programming) has been obtained, let's step on to find the base cases: 1. if given t=0, any given list will return an empty subset and true; 2. if given P has only one value P[1], the result will be t==P[1]; 3. if given t is bigger than the element being considered (P[n]), we directly jump to subset_problem(P-{P[n]}, t). 

Therefore, the whole OPT is as shown as follows: 

![](http://latex.codecogs.com/gif.latex?%24%24OPT%28i%2Cj%29%3D%20%5Cbegin%7Bcases%7D%20True%20%26%20%5Ctext%7Bif%7E%7D%20j%3D0%5C%5C%20False%20%26%20%5Ctext%7Bif%7E%7D%20j%5Cneq0%2C%20i%3D0%5C%5C%20OPT%28i-1%2Cj%29%20%26%20%5Ctext%7Bif%7E%7D%20i%3E0%2Cj-P%5Bi%5D%3C0%5C%5C%20OPT%28i-1%2Cj%29%20%5C%20or%5C%20OPT%28i-1%2Cj-P%5Bi%5D%29%20%26%20%5Ctext%7Botherwise%7D%20%5Cend%7Bcases%7D%24%24)
where i: the ith element being considered and j: the sum which we are seeking a solution for.


