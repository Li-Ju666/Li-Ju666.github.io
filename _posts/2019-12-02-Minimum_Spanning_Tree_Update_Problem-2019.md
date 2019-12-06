---
layout:     post   				    # 使用的布局（不需要改）
title:      MST_Update_Problem				# 标题 
subtitle:   minimum spanning tree #副标题
date:       2019-12-02 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm  
    data_structure
---


## Minimum_Spanning_Tree_Update_Problem
This is the problem in the assignment 2 of Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
>Given a connected, weighted, undirected graph $G = (V, E)$ with non-negative edge weights, as well as a minimum(-weight) spanning tree $T = (V, E')$ of $G$, with $E' \subset E$, consider the problem of incrementally updating $T$ if the weight of a particular edge $e \in E$ is updated from $w(e)$ to $\widehat{w}(e)$. There are four cases:

>case1: $e\notin{E'}$ and $\widehat{w}(e)>w(e)$

>case2: $e\notin{E'}$ and $\widehat{w}(e)<w(e)$

>case3: $e\in{E'}$ and $\widehat{w}(e)<w(e)$

>case4: $e\in{E'}$ and $\widehat{w}(e)>w(e)$

>Perform the following tasks:

>A. For each of the four cases, describe in plain English with mathematical notation an efficient algorithm for updating the minimum spanning tree, and argue that each algorithm is correct and has a time complexity of $\mathcal{O}(\abs{V})$ or $\mathcal{O}(\abs{E})$.

>B. For at least one case that does not take constant time, say case $i \in case(1, 2, 3, 4)$, implement your algorithm as a Python function update MST $i(G, T, e, w)$ for $w = \widehat{w}(e)$. If you give functions for multiple values of $i$, then indicate which one you want to be graded, else we choose the one with the lowest $i$.
