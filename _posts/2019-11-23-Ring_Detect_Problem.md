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
Here a DFS-like algorithm is designed. The basic principle is: if marked all nodes as undiscovered, starting from any of the undiscovered node, DFS is used to explore its connected nodes and noted them as discovered; if any node is discovered twice, then a ring is found. Psudocode of the algorithm is shown as following: 
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

Returning a boolean value indicating if a ring exists in the given graph is solved, however, to return the nodes in the ring remains the problem. For discovered nodes, each node has at most one parent node, indicating from which node it is discovered (for start_node, its parent is none). Here I use the parent of node to trace back and to find the nodes in the ring. If the node ($n$) find a node that has been discovered ($n_d$) twice, then $n_d$ is the starting node of the ring and edge$(n_d, n)$ is the first edge of the ring. Then starting from $n$,  we start to check its parent node($n.parent$), if $n.parent$ is not $n_d$, we save $n.parent$ in the ring list and assign $n = n.parent$ and keep tracing till one's parent is node $n_d$. The psudocode above does not save the parent of each discovered node, so it can be improved to use a dictionary to track parent nodes, in which the key is the node and the corresponding value is its parent. When exploring node, the pair of key and value is added into the dictionary named `parent_dic`.  The ring can be found by keep tracking with dictionary `parent_dic`. Here is the implementation of algorithm as shown following: 
```

def ring_extended(G):
    result = False
    discovered = []
    undiscovered = list(G.nodes)
    parent_dic = {}
    ring_elements = []

    def explore(node, parent):
        result = False
        if node not in discovered:

            discovered.append(node)
            parent_dic[node] = parent
            undiscovered.remove(node)

            adj = list(G.adj[node])

            if parent is not None:
                adj.remove(parent)

            discovered_again = [i for i in adj if i in discovered]

            if len(discovered_again) != 0:
                print("RING FOUND! ")
                ring_elements.append(random.choice(discovered_again))
                ring_elements.append(node)
                return True
            else:
                for i in adj:
                    if result is False:
                        result = explore(i, node)
                return result

    while (len(undiscovered) != 0 and result is False):
        start_node = random.choice(undiscovered)
        result = explore(start_node, None)
    if (result is True):
        i = 1
        while ring_elements[i] != ring_elements[0]:
            ring_elements.append(parent_dic[ring_elements[i]])
            i = i + 1
    return result, ring_elements
```

Everything seems perfect so far. We continue to analyze the complexity of the algorithm. As we all know, the time complexity of DFS (deep-first search) is of $\mathcal{O}(V+E)$. However the task requires the time complexity if bounded by $\mathcal{O}(V)$, then how to improve the algorithm to meet the requirement? 

Actually our algorithm **IS** of time complexity of  $\mathcal{O}(V)$, though it seems of  $\mathcal{O}(V+E)$. Here is the proof: 
