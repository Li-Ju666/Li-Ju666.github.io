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
Given a connected, weighted, undirected graph $G = (V, E)$ with non-negative edge weights, as well as a minimum(-weight) spanning tree $T = (V, E_0)$ of $G$, with $E_0 \subset E$, consider the problem of incrementally updating $T$ if the weight of a particular edge $e \in E$ is updated from w(e) to wb(e). There are four cases:
