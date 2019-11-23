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

>A. Design and implement an efficient algorithm that returns True if and only if there exists a ring in the undirected graph $G = (V, E)$. Moreover, Extend your function to return also a ring, if one exists. Implement your extended algorithm as the Python function `ring(G)`.You can not assume that $G$ is connected. (The packages networkx should be used to make graph)

>B.  Argue that your algorithm has a time complexity of $\mathcal{O}(V)$, independent of $E$.

### Problem Solution
Here a DFS-like algorithm is designed. The basic principle is: if marked all nodes as undiscovered, starting from any of the undiscovered node, DFS is used to explore its connected nodes and noted them as discovered; if any node is discovered twice, then a ring is found. The algorithm is shown as follows: 
```
discovered = *none*
undiscovered = *all_nodes*
resutl = false
while (undiscovered is not empty and result = false):
    start_node = any(undiscovered)
    explore(start_node, none)
return result

def explore(node, its parent): 
    discovered.add(node)
    undiscovered.delete(node)
    connected = find_all_connected_nodes(node).delete(parent)
    if any element in connected is also in discoverd:
        result = true
    else if result = false: 
        explore(each node in connected)
```
The algorithm is designed specially in `while(undiscovered is not empty and result = false)`, that is to say, even if the graph is not connected (containing more than one blocks), the algorithm still works. 

Returning a boolean value indicating if a ring exists in the given graph is solved, however, to return the nodes in the ring remains the problem.  
