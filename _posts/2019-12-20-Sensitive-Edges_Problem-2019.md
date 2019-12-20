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

### Problem Solution
For the problem given, which we will refer to as $sensitive(G,s,t,F)$, $G$ is a network flow graph with $s$ as its source and $t$ as its sink, and $F$ is a calculated maximum flow array, where $F[a][b]$ is an integer flow amount over the edge $(a,b)$. In this task, one of the sensitive edges in graph $G$ is supposed to be returned. Our solution is: all edges which are in the collection of edges of minimum s-t cut $C=(S, T)$ ($s\in S$ and $t \in T$), are sensitive and returning any of these edges will solve the task. The solution can be justified as follows: 

1. According to the max-flow min-cut theorem, for $G$, if a flow $F$ is maximum, $val(F)=cap(S,T)$ while $(S, T)$ is the minimum cut of the graph $\implies$ any reduction of $cap(S, T)$ will reduce $val(F)$; 

2. $cap(S, T) = \sum\nolimits_{e \in (S, T)} cap(e)$ $\implies$ reduction of capacity of any edge in $(S, T)$ will reduce $cap(S, T)$ and $val(F)$ correspondingly $\implies$ any edge in $(S, T)$ is sensitive. Moreover, according to the max-flow min-cut theorem, it is guaranteed that at least one sensitive edge exists, therefore it is not possible to return a result with `(None, None)`.

Notation: 

$G$: given graph; 

$E$: edges of given graph; 

$(S, T)$: collection of edges in the cut between $S$ and $T$ part of a graph;

$val(F)$: flow value of the given flow; 

$cap(S, T)$: flow capacity between $S$ and $T$ part of a graph; 

$cap(a)$: flow capacity of node $a$. 
-------------------
The algorithm for $sensitive(G,s,t,F)$ problem is designed as following: 

1. Firstly we create a function named $res_graph(G, F)$ to construct the residual graph of given graph $G$ with its flow $F$;

2. Then a function named $explore(G, node, reach\_dict)$ is created to explore all reachable nodes from a given node in a DFS-like approach. Parameter $reach\_dict$ is the dictionary recording the reachable nodes, in which reachable nodes are marked as 1 while unreachable nodes and unexplored are noted as -1;

3. With the above two functions, we construct the residual graph $rG$ of given graph $G$ and its maximum flow $F$ and initialize a $reach\_list$ with all nodes noted as -1. Then from the resource node $s$ we start to explore the graph to find all reachable nodes from $s$ in $rG$. The collection of all reachable nodes from $s$ are noted as $S$ while all unreachable nodes collection are $T$. According to Ford–Fulkerson algorithm, $(S, T)$ is the minimum cut; 

4. Then we check all edges to record saturated nodes whose flow equals its capacity in a list named $satur\_edges$. Saturated edges whose start node is in $S$ and ending node is in $T$ are sensitive. We return the first edge which satisfies this condition. 


The implementation of the algorithm is shown as following: 
```
def sensitive(G, s, t, F):
    """
    Sig:  graph G(V,E), int, int, int[0..|V|-1, 0..|V|-1] ==> int, int
    Pre:    G: a directed graph, in which every edge has its capacity;
            s: flow starts from node s; t: flow ends at node t;
            F: the given max flow matrix, in which flows each edge holds are stored
    Post:   return a sensitive edge by giving its starting node and ending node
    Ex:   sensitive(G,0,5,F) ==> (1, 3)
    """
    ## Function to construct residual graph of given graph with its flow
    def res_graph(Graph, Flow):
        """
        Sig:  graph G(V,E), int[0..|V|-1, 0..|V|-1] ==> graph eG(V, E)
        Pre:    G: a directed graph, in which every edge has its capacity;
                F: the given max flow matrix, in which flows each edge holds are stored
        Post:   return the residual graph of given graph and flow
        Ex:   res_graph(G,F) ==> rG
        """
        rG = Graph.copy()
        ## update the copy of original graph edge by edge
        # Loop variant: number of nodes in G not added to rG decreases
        for start_node in Flow:
            # Loop variant: number of nodes in a flow starting from start_node not checked decreases
            for end_node in Flow[start_node]:
                ## add the reversed edge with capacity of current flow
                rG.add_edge(end_node, start_node, capacity = Flow[start_node][end_node])
                ## update the original edge's capacity to original capacity minus flow
                rG[start_node][end_node]['capacity'] -= Flow[start_node][end_node]
        return rG

    ## build a function to explore all reachable nodes from a given node,
    ## reach_dict is the dictionary recording the reachable nodes,
    ## in which reachable nodes are marked as 1 while unreachable nodes are noted as -1
    def explore(G, node, reach_dict):
        """
        Sig:  graph G(V,E), int, dictionary ==> dictionary
        Pre:    G: a directed graph which is the residual flow graph for a graph and its flow;
                node: node we start to explore
                reach_dict: a dictionary in which all nodes are discovered or not from node
                are recorded
        Post:   return the dictionary in which every node are reachable or not are recorded:
                if reachable, it is noted as1 otherwise it's noted as -1
        Ex:   explore(G,s,reach_dict) ==> reach_dict
        """
        reach_dict[node] = 1
        ## a DFS-like approach, to explore each node directly linked with current node
        # Loop variant: number of nodes in graph G not checked decreases
        for each in G[node]:
            if G[node][each]['capacity'] > 0 and reach_dict[each] == -1:
                # Recursive variant: number of unexplored nodes in G decreases
                explore(G, each, reach_dict)
        return reach_dict

    ## construct the residual graph rG of given graph G
    rG = res_graph(G, F)
    ## build a the status dictionary and mark every node in rG as -1,
    ## which means unreachable (currently)
    reachable_dict = dict.fromkeys(list(rG.nodes), -1)
    ## explore the graph from the the node s, to find all reachable nodes
    # Recursive variant: number of nodes in residual graph unexplored decreases
    reachable_dict = explore(rG, s, reachable_dict)

    ## find all saturated edges, whose capacity equals the flow it holds
    satur_edges = []
    # Loop variant: number of nodes in flow F not checked decreases
    for start_node in F:
        # Loop variant: number of nodes in a flow starting from start_node not checked decreases
        for end_node in F[start_node]:
            if F[start_node][end_node] == G[start_node][end_node]["capacity"]:
                satur_edges.append([start_node, end_node])

    ## NOTICE: not all saturated edges are sensitive edges -- only those which are in minimum
    ## cut are sensitive
    sens_edges = []
    # Loop variant: saturated edges not checked decreases
    for each in satur_edges:
        ## to find which saturated edges are among the minimum cut
        if reachable_dict[each[0]] == -1 and reachable_dict[each[1]] == 1:
            sens_edges.append(each)
    ## return any edge of sensitive edges: because of the theorem of maxflow-minicut, it is
    ## guaranteed that there is at least one edge is sensitive
    return sens_edges[0]

```
-----------------
For the algorithm designed in the previous section, we analyze its comprising parts' time complexity to find our algorithm's complexity.

1. Firstly a residual flow of $G$ is created: in this step, all edges are visited and for each edge a constant time is required. Therefore, the time complexity of this step is of $\mathcal{O}(\abs{E})$; 

2. From $s$ we explore the residual graph $rG$: as we only explore from one node following edges, therefore the time complexity of this step is up bounded by $\mathcal{O}(\abs{E})$; 

3. Then all edged are checked to find all saturated edges, while each edge could be examined in a constant time. Also the time complexity of checking which saturated edges are in minimum cut is also up bounded by the number of saturated edges. So the time complexity of this step is of $\mathcal{O}(\abs{E})$; 

Therefore, the overall time complexity of the algorithm is $\mathcal{O}(\abs{E})$. 
