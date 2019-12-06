---
layout:     post   				    # 使用的布局（不需要改）
title:      Minimum_String_Difference Problem				# 标题 
subtitle:    #副标题
date:       2019-12-06 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    algorithm  
---


## Minimum_String_Difference Problem
This is the problem in the assignment 2 of Algorithm and Data Structure 2. Here are my solutions. 
### Problem Description
>A useful feature for a search engine is to suggest a replacement string when a search string
given by the user is not known to the search engine. In order to suggest and rank replacement
strings, the search engine must have some measure of the *minimum difference* between the
given search string and a possible replacement string. For example, over the alphabet $A =
{A, . . . , Z}$, let the user’s search string be $u = DINAMCK$ and let a suggested replacement
string be $r = DYNAMIC$. The *minimum difference* of strings u and r is the minimum cost of
changes transforming $u$ into $r$, where a change is either altering a character in $u$ in order to get
the corresponding character in $r$, or skipping a character in $u$ or $r$. A *positioning* of two strings
is a way of matching them up by writing them in columns, using a dash (–) to indicate that a
character is skipped. For example: 
>
>DINAM-CK
>
>DYNAMIC-
>
>The *difference* of a positioning is then the sum of the resemblance costs of the character pairs
in each column of the positioning, as given by a resemblance matrix $R$. For an alphabet $A$, we
have that $R$ is an $(\abs{A}+ 1)\times (\abs{A}+ 1)$ matrix, as it must include the dash in addition to the $\abs{A}$
characters of the alphabet. For example, the positioning above has a difference of:
>
>$R[D,D] + R[I,Y] + R[N,N] + R[A,A] + R[M,M] + R[–,I] + R[C,C] + R[K,–]$
>
>For example, if $R[x, y] = 1$ for all $x, y \in A\cup {–}$ with $x \neq y$ and if $R[x, x] = 0$ 
for all $x\in A\cup {–}$, then the difference of a positioning is the number of changes; 
another resemblance matrix could store the Manhattan distances on a QWERTY keyboard between characters of the alphabet. 
>
>Given two strings $u$ and $r$ of possibly different lengths over an alphabet $A$ that does not
contain the dash character, and given an $(\abs{A}+ 1)\times (\abs{A}+ 1)$ resemblance matrix $R$ of integers,
which cannot be assumed to be symmetric, perform the following tasks:
>
>A. Give a recursive equation for the minimum difference of $u$ and $r$, including the semantics
of all the parameters. Use it to justify that dynamic programming is applicable to the
problem of computing this minimum difference.
>
>B. Design (including the choice between top-down recursive and bottom-up iterative) and
implement an efficient dynamic programming algorithm for this problem as a Python
function `min_difference_align(u, r, R)` to return both the number and a positioning for the minimum difference, 
assuming that the last row and last column of $R$ pertain to the dash character. 
>
>Argue that your algorithm has a time complexity of $\mathcal{O}(\abs{u} \times \abs{r})$. 

### Problem Solution

*A. Recursive Function*: 

For the problem given, which we will refer to as `min_difference(u, r, R)`, in which $u$ and $r$ are two 
strings to be compared and $R$ is the difference matrix given. we recognize that the solution can be split 
into numerous sub-problems by summing results of selected sub_problems (minimum difference of sub_strings) up. 
Therefore, dynamic programming is applicable to solve the problem.

We define $OPT(i,j)$ as the recursive function for our solution, representing the minimum difference between two 
sub_strings made by first $ith$ characters of $u$ and first $jth$ characters of $r$. $OPT(i,j)$ is the minimum 
of: 

(1). distance between $\mbox{-}$ and $r[j]$ plus $OPT(i, j-1)$; 

(2). distance between $u[i]$ and $\mbox{-}$ plus $OPT(i-1, j)$;

(3). distance between $u[i]$ and $r[j]$ plus $OPT(i-1, j-1)$, 

when $i>0$ and $j>0$. 

When $i$ or $j$ reaches $0$, $OPT(i, j)$ is the sum of difference between $-$ and every left character in the string.

The recursive equation follows: 
$$OPT(i,j)=
    \begin{cases}
    \min \{dis(\mbox{-}, r[j])+OPT(i, j-1), \; dis(u[i], \mbox{-})+OPT(i-1, j), \\
    \; \; \; \; \; \; \; \; dis(u[i], r[j])+OPT(i-1, j-1) \} & \text{if~} i>0, j>0\\
    dis(\mbox{-}, r[j])+OPT(i, j-1) & \text{if~} i=0, j>0\\
    dis(\mbox{-}, u[i])+OPT(i-1, j) & \text{if~} i>0, j=0\\
    0 & \text{if~} i=0, j=0
    \end{cases}$$

where $dis(char1, char2)$: the distance between $char1$ and $char2$ obtained from given distance matrix $R$, $i$ 
and $j$: indicating that the sub_string made by first $ith$ letters of $u$ and sub_string made by first $jth$ 
letters of $r$ are being compared to get their minimum difference.

Our equation is obviously $0$ for $i=0$ and $j=0$ since the minimum difference between two empty string 
is definitely $0$. Also for case when $i=0, j>0$ or when $i>0, j=0$, all we need to do is to sum up distances 
between hyphen and each letter of string which is not empty, as we present in the recursive equation, calculating 
each distance letter by letter and summing up recursively.

As for the rest of the values, we know that when comparing two string $u$ and $r$ at a specific position $i$ and $j$, 
there are three different cases: 

(1). move $u$ left (leave a hyphen), compare $\mbox{-}$ and $r[j]$, and move on to sub_problem $OPT(i, j-1)$; 

(2). move $r$ left (again, leave a hyphen), compare $u[i]$ and $\mbox{-}$, and move on to sub_problem $OPT(i-1, j)$ 

or (3). compare $u[i]$ and $r[j]$ and move on to sub_problem $OPT(i-1, j-1)$. 

Among the three cases, we need to find the minimum one to be the result of current sub_problem. 

When the calculation of all values is done, the solution for `min_difference(u, r, R)` will be equal 
to $OPT(u.length,r.length)$'s value.

--------


