---
layout:     post   				    # 使用的布局（不需要改）
title:      Party Seating Problem				# 标题 
subtitle:    #副标题
date:       2019-12-20 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm
---


## Party Seating Problem
This is 2nd the problem in the assignment 3 of Course Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
>A number of guests will attend a party. For each guest $g$, we are given a duplicate-free
list $known[g]$ of the other guests known by $g$; these lists are symmetric in the sense that if
guest $g_{1}$ knows guest $g_{2}$, then $g_{2}$ also knows $g_{1}$; the sum of the lengths of these lists is denoted
by $\ell$. To encourage their guests to meet new people, the party organisers would like to seat all
of the guests at two tables, arranged in such a way that no guest knows any other guest seated
at the same table. We call this the two-table party seating problem. Perform the following tasks:
>
>1. Formulate the two-table party seating problem as a graph problem; 
>
>2. Design and implement an efficient algorithm as a Python function `party(known)` that
returns `True` if and only if the two-table party seating problem has a solution. If such
an arrangement exists, then the algorithm should also return one, in the form of two
duplicate-free lists of guests, namely one list for each table; else the algorithm should
return `False` and two empty seating lists.
>
>3. Argue that your algorithm has a time complexity of $\mathcal{O}(\abs{known}+\ell)$; 
>
>4. We can generalise the party seating problem to more than two tables in the following
manner. The party is attended by p groups of guests, and there are q tables. All members
of a group know each other; no guest knows anyone outside his or her group. The sizes of
the groups are stored in the array Group, and the sizes of the tables in the array Table.
The problem is to determine a seating arrangement, if it exists, such that at most one
member of any group is seated at the same table. Give a formulation of this generalised
party seating problem as a maximum flow problem.

### Problem Solution
A. The Party Seating Problem can be expressed as a graph problem. Since the goal is to retrieve 
a table arrangement such that no two people in the same table know each other, we can create a 
graph where every person is represented with a node, and if a person $g_{1}$ knows another person $g_{2}$, 
so $g_{2}\in{known[g_{1}]}$, there is an edge $(g_{1},g_{2})$. 

Then the problem can be reformulated as following: 

Given a graph $G(V,E)$, does there exist a set of nodes $A$ and its complement $B = V \setminus A$ so that 
for all nodes $x,y\in{V}$ for which there exists an edge $(x,y)\in{E}$ are split between $A$ and $B$?

------
B. For this problem, a DFS-like algorithm is designed to split the graph $G$ into two sets while all edges are 
cut between two sets, $A$ and $B$. As per the problem's description, there are no any two connected nodes which 
are supposed to be arranged in one set, so we keep arranging nodes along the edge.

If there are any nodes which are still unexplored, we choose one of them, $a$, put it into set $A$ and check 
all its edges. Following each edge of $a$,

1. if the node connects with a node which is not yet assigned, put this node in the opposite set $B$ and explore 
this node recursively; 

2. if the node connects with a node which has been assigned in the opposite set $B$, do nothing; 

3. if the node connects with a node which has been assigned in the set $A$, return False. 

If every node can be put into one of the two sets without any contradiction, the arrangement is available, 
otherwise contradiction (two arranged nodes which are in the same set are connected) arises and False is returned. 

The implementation of the algorithm is shown as following: 

1. A status dictionary is created to record the status of each node, in which all nodes are keys and all values 
are set as 0 initially. In this dictionary, the value 1 of a node means the node is arranged in set $A$, -1 means 
it is in set $B$ and 0 means it is not arranged. Also a dictionary named $unarranged\_dict$ is defined to record nodes
which have not been arranged into a set yet. 

2. A function named `arrange(node, parent)` is defined, to arrange a node and its children. If its parent is None, 
the node is put into $A$, otherwise it is put into $B$. Then its connected nodes are saved to be checked and arranged.

3. From the unarranged node dictionary, we arrange all nodes as defined in step 2. If all nodes are assigned without 
any contradiction, return the two sets, otherwise return two empty lists. 

The code is shown as following: 
```
def party(known):

    """
    Sig:  int[1..m, 1..n] ==> boolean, int[1..j], int[1..k]
    Pre:  a list of list, in which stored the edges between nodes
    Post: a boolean indicating if such arrangement can be made, if True, two list containing the plan are returned,
          otherwise two empty lists are returned.
    Ex:   [[1,2],[0],[0]] ==> True, [0], [1,2]
    """
    ## construct a dictionary to store the current status of every node, 0 means the node has not been arranged,
    ## 1 means it is set in the list1, while -1 means it is set in the list2. The initial status is all nodes are 0.
    status_dict = dict.fromkeys(range(len(known)), 0)

    ## A dictionary for storage of unarranged nodes:
    ## the reason for using dictionary instead of list is that the complexity of dictionary's deletion is of constant time,
    ## removal of an arranged node requires operated |nodes| times, this is used for reduction of time complexity.
    unarranged_dict = dict.fromkeys(range(len(known)), 0)

    ## The default result if such arrangement is available is True
    result = True

    ## the function arrange is defined to arrange the given node and its children recursively
    def arrange(node, parent):
        ## update the status of current node in status_dictionary according to its parent's status: opposite to its
        ## parent's status
        if parent is None:
            status_dict[node] = 1
            linked_nodes = known[node]
        else:
            status_dict[node] = -status_dict[parent]
            linked_nodes = known[node]
            linked_nodes.remove(parent)

        ## delete the current node from dictionary of unarranged nodes
        unarranged_dict.pop(node)

        ## The default result if such arrangement is available is True, if every node is put with no contradiction,
        ## it keeps True
        result = True

        ## arrange given node's children
        if linked_nodes is not None:
            for each in linked_nodes:
                ## if contradiction occurs, end the loop
                if result == True:
                    ## a child's status equals its own status: contradiction occurs
                    if status_dict[each] == status_dict[node]:
                        result = False
                    ## the child has not been arranged: recursively arrange the child
                    elif status_dict[each] == 0:
                        # Recursive variant: number of not explored nodes in known decreases
                        result = arrange(each, node)
        return result

    ## arrange all the unarranged nodes: if contradiction occurs, end the loop
    # Loop variant: number of not arranged nodes in known decreases
    while result is True and len(unarranged_dict) > 0:
        # Recursive variant: number of not arranged nodes in known decreases
        result = arrange(unarranged_dict.keys()[0], None)
    table1, table2 = [], []
    if result == True:
        ## traverse the status dictionary: nodes whose value are 1 is put in list1, while those whose value is -1
        ## are appended in list2
        for each in status_dict:
            if status_dict[each] == 1:
                table1.append(each)
            else: table2.append(each)
    return result, table1, table2
```
------------------------------
C. The time complexity of the algorithm above is analyzed as following: 

1. Firstly two dictionaries are created, status dictionary and $unarranged\_dict$: for each node a constant time will 
be cost, therefore, this step will is of the time complexity of $\mathcal{O}(\abs{V})$; 

2. Then nodes are to be arranged: in the worst case, each nodes are well-put in sets following each edge. 
In another word, all nodes and edges are visited. Each visit takes a constant time and number of visit is 
of $\mathcal{O}(\abs{V} + \abs{E})$. Therefore, this step is of the time complexity of $\mathcal{O}(\abs{V} + \abs{E})$;

3. If all nodes are arranged, we need to return the right arrangement: if this step, each node in status dictionary is 
visited, while each visit takes a constant time, therefore the time complexity of this step is of $\mathcal{O}(\abs{V})$. 

Therefore, the overall time complexity of the designed algorithm is of $\mathcal{O}(\abs{V} + \abs{E})$. 
Since, though, $\abs{known}$ is essentially the number of nodes contained in our graph, and the sum of these 
lists $\ell$ is precisely the number of edges we have, we can rewrite it as $\mathcal{O}(\abs{known} + \ell)$.

