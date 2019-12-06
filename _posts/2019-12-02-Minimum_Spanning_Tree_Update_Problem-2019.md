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

We argue that the time complexity of the algorithm is $\mathcal{O}(\abs{V})$. Adding an edge to $T'$ takes a constant time $\mathcal{O}(1)$, retrieving the cycle from $T'$, as proven in [previous article](https://li-ju666.github.io/2019/11/23/Ring_Detect_Problem/), it can take $\mathcal{O}(\abs{V})$ time, and traversing all edges in $C$ order to find $e'$ takes $\mathcal{O}(\abs{E''})$ time. But since our cycle has at most $\abs{E'}$ edges, and $\abs{E'}$ is bounded by $\abs{V}$, the time needed is $\mathcal{O}(\abs{V})$.

In total, time needed is $\mathcal{O}(1)+\mathcal{O}(\abs{V})+\mathcal{O}(\abs{V})=\mathcal{O}(\abs{V})$.

---------

*Case3*: 
Since $e \in E'$, we know that removing this edge and adding another edge $i$ that connects the two cuts created by its removal leads to a spanning tree $T'=(V,E'')$, $i\in E''$, that is either not minimum, or a MST having the same total tree weight with that of $T$. By decreasing the weight of $e$, the total weight of $T$ becomes less than the total tree weight of $T'$ in either case, since $e \notin E''$, so T will remain the MST of $G$.
Therefore, we do not need to make any changes to $T$.
No actions are needed for this case, so our time complexity is $\mathcal{O}(0)$, which is also in $\mathcal{O}(\abs{V})$.

----------

*Case4*: 
The edge updated, $e\in E'$, so we have to check whether its removal from $E'$ and addition of another one from $E$ leads to a new MST.

The algorithm for the update is as follows: We remove $e=(A,B)$ from $E'$. Then $T$ is left with two unconnected cuts, $C_{1}=(V_{1},E_{1})$ and $C_{2}=(V_{2},E_{2})$. We traverse $T'$ using BFS twice, once with node $A$ as root and once with $B$ as root, in order to retrieve $C_{1}$ and $C_{2}$. Then, for all edges $e'(A,B)\in (E-E')$, where $A\in C_{1}$ and $B\in C_{2}$, we find the one with the minimum weight, and add it to $T$. This gives us the updated MST.

The algorithm has a time complexity of $\mathcal{O}(\abs{E})$. Removing an edge has a constant time of $\mathcal{O}(1)$. We also have two BFS traversals for the two cuts, which in total take $\mathcal{O}(\abs{V}+\abs{E'})$ total time since $\abs{V_{1}}+\abs{V_{2}}=\abs{V}$ and $\abs{E_{1}}+\abs{E_{2}}=\abs{E'} -1$. But, since for an MST $\abs{V}=\abs{E'}-1$, and $\abs{E'} \leq \abs{E}$, the total time needed for a traversal is $\mathcal{O}(\abs{E})+\mathcal{O}(\abs{E})=\mathcal{O}(\abs{E})$. Finally, we need to loop all edges in $E-E'$ to find the minimum weight edge, which takes a total time of $\mathcal{O}(\abs{E})$.

In total, time needed is $\mathcal{O}(1)+\mathcal{O}(\abs{E})+\mathcal{O}(\abs{E})+\mathcal{O}(\abs{E})=\mathcal{O}(\abs{E})$.

Following is the implementation of algorithm for case 4: 

```
def update_MST_4(G, T, e, w):
    """
    # Sig: graph G(V,E), graph T(V, E), edge e, int ==> graph G with updated edge e, updated graph T (MST of G)
    # Pre: A connected graph G, a subgraph T, which is the minimum spanning tree (MST) of G, an edge e to be
           updated which is in both G and T with a weight w greater than original weight of e.
    # Post: G is updated with the weight w(e), T is a minimum spanning tree of G.
    # Ex:  TestCase 4 below
    """
    (u, v) = e
    ## To check if given e is satisfied with the case:
    assert (e in G.edges() and e in T.edges() and w > G[u][v]['weight'])

    ## To update G with given edge e
    G.add_edge(*e, weight = w)

    ## remove the changed edge, for it is not guaranteed to be in the MST
    T.remove_edge(u, v)
    # print(list(T.edges))
    # print(list(T.adj['a']))

    ## DFS traversal, returning all nodes in a tree.
    def return_cut(n,p):
        """
    	# Sig:  node, node --> list
    	# Pre:  n,p belong in V and there exists an edge e=(p,n) which belongs in E
    		    for an acyclic undirected graph G=(V,E)
    	# Post: list of n and all of its children
    	# Ex:   V=[1,2,3,4], E={(1,2),(2,3),(2,4)}
    			return_cut(1,None)=[1,2,3,4]
    			return_cut(2,None)=[2,1,3,4]
    			return_cut(2,1)=[2,3,4]
    	"""
        cut = [n]
        children = list(T.adj[n])
        if p is not None:
            children.remove(p)

        # Loops over every node N belonging to G
        # Loop variant: number of children not checked decreases
        for c in children:
            # Recursive call to return lists of all children of n
            # Recursive variant: number of unvisited nodes decreases
            cut = cut+return_cut(c,n)
        return cut

    ## DFS to find out two unconnected cuts of T created by removal of e
    # Recursive variant: number of unvisited nodes in cut_a that can be reached from u decreases.
    cut_a = return_cut(u, None)
    # Recursive variant: number of unvisited nodes in cut_b that can be reached from u decreases.
    cut_b = return_cut(v, None)

    ## To define a dictionary, in which nodes in cut 1 are noted as -1, while nodes in cut 2 are noted as -1
    dic = dict.fromkeys(cut_a, -1)
    dic.update(dict.fromkeys(cut_b, 1))

    ## To find all edges connnecting the two cuts
    edges = list(G.edges)
    cut_set = []
    ## For those edges whose two nodes are in different blocks, the sum of dictionary values of two nodes are exactly 0
    # Loops over all edges in E
    # Loop variant: number of edges in E not checked decreases
    for each in edges:
        if dic[each[0]]+dic[each[1]] == 0:
            cut_set.append(each)

    ## To find the minimum edge connecting the two cuts
    weight = []
    # Loops over all edges in cut_set
    # Loop variant: number of edges in cut_set not checked decreases
    for each in cut_set:
        weight.append(G.edges[each]['weight'])

    ## add the minimum edge into graph T, which is the recomputed MST of updated G
    T.add_edge(*cut_set[weight.index(min(weight))], weight = min(weight))
    return None
```


