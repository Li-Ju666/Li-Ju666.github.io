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

### Problem Solution

*Case 1*: Since $e\notin E'$, we know that a spanning tree $T'=(V,E'')$ where $e\in E''$, is either not a MST, or a MST having the same total tree weight with $T$. By increasing the weight of $e$, the total weight of $T'$ becomes greater than that of $T$ in either of these cases, which does not lead to a MST.

Therefore, for this case, we do not make any changes to $T$.

Since we do not need to do any actions for this case, time needed is $\mathcal{O}(0)$, which by definition also belongs in $\mathcal{O}(\abs{V})$.

--------

*Case 2*: 
Since $e\notin E'$ and its weight is decreased, we have to check whether its addition to T' and a removal of another edge from it leads to a new MST.

We design an algorithm to update the MST based on that principle. Firstly, we add $e$ to $E'$. Based on [MST's cyclic property](https://en.wikipedia.org/wiki/Minimum_spanning_tree#Cycle_property), we know that now we have a single cycle, $C$, in $T$. We retrieve $C=(V',E'')$ traversing the tree using BFS, find the edge $e'\in E''$ having the maximum weight in $T'$, and remove $e'$ from $T'$. This results in the updated MST.

We argue that the time complexity of the algorithm is $\mathcal{O}(\abs{V})$. Adding an edge to $T'$ takes a constant time $\mathcal{O}(1)$, retrieving the cycle from $T'$, as proven in [previous article](https://li-ju666.github.io/2019/11/23/Ring_Detect_Problem/), it can take $\mathcal{O}(\abs{V})$ time, and traversing all edges in $C$ order to find $e'$ takes $\mathcal{O}(\abs{E''})$ time. But since our cycle has at most $\abs{E'}$ edges, and $\abs{E'}$ is bounded by $\abs{V}$, the time needed is $\mathcal{O}(\abs{V})$.\\
In total, time needed is $\mathcal{O}(1)+\mathcal{O}(\abs{V})+\mathcal{O}(\abs{V})=\mathcal{O}(\abs{V})$.


