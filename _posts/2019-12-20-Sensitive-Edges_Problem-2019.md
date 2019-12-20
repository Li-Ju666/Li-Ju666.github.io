---
layout:     post   				    # 使用的布局（不需要改）
title:      Sensitive Edge Problem				# 标题 
subtitle:   max-flow min-cut #副标题
date:       2019-12-20 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm
---


## Sensitive Edge Problem
This is the problem in the assignment 3 of Course Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
> A flow network is a directed graph $G = (V, E)$ with a source $s$, a sink $t$, and a nonnegative
capacity $c(u, v)$ on each edge. An edge of a flow network is called sensitive if decreasing its
capacity always results in a decrease of the maximum flow; in other words, decreasing the flow
of a sensitive edge by a single unit reduces the maximum flow of the entire network. Perform
the following tasks:
>
>Design and implement an efficient algorithm as a Python function `sensitive(G, s, t, F)` for
a flow network $G$ with source $s$ and sink $t$, and a previously computed maximum flow
matrix $F$, where the element $F[a][b]$ is an integral flow amount over the edge $(a, b)$. Your
function should return a sensitive edge $(u, v)$ if and only if one exists. If no sensitive edge
exists, then the function should return `(None, None)`. Then analyze the time complexity of the algorithm. 

### Solution for the problem
