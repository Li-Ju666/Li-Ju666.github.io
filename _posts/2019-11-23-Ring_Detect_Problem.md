---
layout:     post   				    # 使用的布局（不需要改）
title:      Ring Detect Problem				# 标题 
subtitle:   #副标题
date:       2019-11-23				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm
---


## Ring Detect Problem
This is the problem in the assignment 1 of Course Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
>In an undirected graph, a path <$v_0, v_1, ..., v_k$> forms a ring if the vertices $v_0$ and $v_k$ are equal and all edges on the path are distinct: $\forall(i, j \in 0, .. k-1)\: i\neq j \Rightarrow (v_i, v_{i+1}) \neq (v_j, v_{j+1})$. Perform the following tasks: 

>A. Design and implement an efficient algorithm that returns True if and only if there exists a ring in the undirected graph $G = (V, E)$. Moreover, Extend your function to return also a ring, if one exists. Implement your extended algorithm as the Python function `ring(G)`.You can not assume that $G$ is connected. 

>B.  Argue that your algorithm has a time complexity of $\mathcal{O}(V)$, independent of $E$.
