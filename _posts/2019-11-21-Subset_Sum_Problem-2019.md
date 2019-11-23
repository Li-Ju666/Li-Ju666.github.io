---
layout:     post   				    # 使用的布局（不需要改）
title:      Subset Sum Problem				# 标题 
subtitle:   present problem #副标题
date:       2019-11-21 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm
---


## Subset Sum Problem
This is the problem in the assignment 1 of Course Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
> Given a set P of prices for items in a gift shop, as well as a total amount t of money collected by a group of people towards buying birthday presents for a common friend, 
the birthday present problem is to determine whether there exists a subset $P'\subset P$ whose sum is exactly $t$, since we do not want to distribute any excess money back to the donors. For example, if $P={1, 7, 2, 32, 56, 35, 234, 12332}$ and $t=299$, then the subset $P'={2, 7, 56, 234}$ is a solution, but there is no solution for $t=11$. Perform the following tasks:

>Give a recursive equation for the required Boolean, including the semantics of all the parameters. Design and implement an efficient dynamic programming algorithm for the birthday present problem as a Python function `birthday_present(P, n, t)` that returns True and the chosen subset if and only if there exists a subset $P'$ with sum $t \geq 0$ of the set $P$, which is given as an array of n non-negative integers.If there is not a valid subset, return False and an empty list. Analyze the complexity of your algorithm in the worst case. 

### Problem Solution
To solve the problem, following points need to be thought carefully: 

- As a dynamic programming-related problem, what is the substructure of the problem? In another word, how to convert the whole problem into the same problem with smaller sizes of input? 

Let's assume a simple case, $P={1,2,3,4}$ and $t=7$. For every element (for instance: 4) in $P$, there are only two cases: the element is included in the subset $P'$, or not. If included, the next step is to solve the (exactly) the same problem, in which $P={1,2,3}$ and $t=3$. If not, the problem remained is to solve: $P={1,2,3}$ and $t=7$. If any of the two situations is true, then the answer of the whole problem is true. Here comes the recursive function: $problem(P, t) = problem(P-{P[n]}, t)\, \text{or}\, problem(P-{P[n]}, t-P[n])$, where $P$ is the given list, $t$ is the given sum and $n$ indicates that the nth elements in $P$ is being considered if to be chosen or not. 

Since the recursive function (also known as OPT in dynamic programming) has been obtained, let's step on to find the base cases: 1. if given $t=0$, any given list will return an empty subset and true; 2. if given $P$ has only one value $P[1]$, the result will be $t==P[1]$; 3. if given t is bigger than the element being considered ($P[n]$), we directly jump to $problem(P-{P[n]}, t)$. 

Therefore, the whole OPT is as shown as follows: 

$$OPT(i,j)=
    \begin{cases}
    True & \text{if~} j=0\\
    False & \text{if~} j\neq0, i=0\\
    OPT(i-1,j) & \text{if~} i>0,j-P[i]<0\\
    OPT(i-1,j) \ or\  OPT(i-1,j-P[i]) & \text{otherwise}
    \end{cases}$$

where $i$: the ith element being considered and $j$: the sum which we are seeking a solution for.

- As the recursive function will calculate some subproblems repetitively, as the slogan of dynamic programming says: "Those who forget the past is condemned to repeat it again and again", it is required to save the calculated results of subproblems. How can we achieve this? 

As the OPT we get is a two-variable function, therefore, a two-dimension matrix is required to save the results of subproblems. The two dimensions are: the number of elements in the set we are considering ($n=len(P)$), and the target price ($t$). The whole matrix $A$ should have the size of $[n+1, t+1]$, for the range of $i$ and $j$ are from $[0, n]$ and $[0, t]$, respectively. Each position in the matrix ($A[i,j]$) represent the result of $problem(P-{P[i: len(P)]}, j)$, thus, the value of $A[n,t]$ is the result of the whole problem. 

- How can we get the result we need with the result matrix? 

As the slogan of dynamic programming implies, the results of subproblems is repeatitively used during the calculation and the matrix is used to save the results. Therefore, a simple thinking is that, as long as the result of $A[n,t]$ required values of other positions again and again, if the matrix is fulfilled with loop, from left to right, from top row to bottom, there won't be any repetition during the calculation (as OPT shows, one's value is only from base cases or values in its left and upper positions). At the end, $A[n,t]$ is returned as the result of the whole problem. This is called bottom-up way in dynamic programming. 

However if one reconsiders the bottom-up approach, (s)he will find that, many values in the matrix is actually useless to get $A[n,t]$, which is actually what we require. For instance like following examply: 
![avatar](/img/19-11-21/01.jpg)
To get the value of $A[5, 4]$, as OPT shows only partly values in the matrix are visited. To optimize the time complexity of the algorithm, a top-down algorithm is developed: Firstly a function is defined: to get the value of a $subproblem(i, j)$, the value of $A[i,j]$ in the matrix is visited firstly, if it is not none (which means it has been calculated and saved in the matrix), return $A[i, j]$; if it is none (which means it has not been calculated yet), OPT is used to get the required value recursively, and save the result in the matrix. The result of the whole problem is $subproblem(n, t)$: in the calculation, only values required to solve the whole problems are calculated and saved, while others positions keep empty. In this recursive way, the problem solved. 

- As long as if the problem has a valid solution or not has been solved, how can we return the solution: the chosen subset?

To return the chosen subset, it is required to trace back, which elements are selected one by one. So far, as the result matrix $A$ has been obtained, $A$ can be used to track chosen ones. For each "true" value in $A$ (let's say, $A[i,j]$), there are two possible situations the value is from: if $A[i-1,j]$ is also true, then $P[i]$ is not chosen, we should check $A[i-1, j]$ further; else $P[i]$ is chosen, we should add $P[i]$ into the list (to save chosen elements), and check $A[i-1, j-P[i]]$ further. We can trace all the chosen elements back starting from the most right-bottom "true" value recursively. 

Our algorithm ends, returning a boolean value indicating whether a valid solution exists or not, and a list with all the chosen elements. Following is the implementation of the algorithm: 
```
def birthday_present_subset(P, n, t):
    '''
    Sig:  int[0..n-1], int, int --> int[0..m]
    Pre:  P is an array of n non-negative integers, t>=0
    Post: If there is a valid subset P' of P that solves the problem,
          return P'. Return empty list otherwise.
    Ex:   P = [2, 32, 234, 35, 12332, 1, 7, 56]
          birthday_present_subset(P, len(P), 299) = [56, 7, 234, 2]
          birthday_present_subset(P, len(P), 11) = []
    '''


    # Like function birthday_present(), A is the result matrix.
    A = [[None for i in range(t + 1)] for j in range(n + 1)]
    # list  'subset' is used to store the chosen elements.
    subset = []

    def birthday_sub(P, i, j):
        '''
        Sig:  int[0..n-1], int, int --> Boolean
        Pre:  i, j are non negative integers, i<=n, j<=t,
              P is a list of n non negative integers.
        Post: True if valid solution exists for birthday_present(P,i,j),
              False otherwise
        Ex:   P = [2, 32, 234]
              birthday_sub(P,0,2) = False
              birthday_sub(P,1,2) = True
        '''
        #nonlocal A
        if A[i][j] == None:
            if j == 0:
                A[i][j] = True
            else:
                if i == 0:
                    A[i][j] = False
                elif P[i-1] > j: 
                    A[i][j] = birthday_sub(P, i-1, j)
                else: 
                    A[i][j] = birthday_sub(P, i-1, j) or birthday_sub(P, i-1, j-P[i-1])
        return A[i][j]

    # find_subset function is defined: to find the chosen elements recursively, by tracing where the 'true' is from.
    def find_subset(i, j):
        '''
        Sig:  int, int --> int[0..m]
        Pre:  n, t are non negative integers, 
        Post: (none)
        '''

         #nonlocal subset, P, A
        if i > 0 and j > 0 and A[i][j] == True:
                # if A[i-1][j] is also true, P[i-1] is not in the chosen subset.
                if A[i-1][j] == True: 
                    find_subset(i-1, j)
                # Otherwise P[n-1] is in the chosen subset.
                else: 
                    subset.append(P[i-1])
                    find_subset(i-1, j-P[i-1])

    A[n][t] = birthday_sub(P, n, t)
    find_subset(n, t)
    return A[n][t], subset
```
