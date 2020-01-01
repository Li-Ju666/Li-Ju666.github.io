---
layout:     post   				    # 使用的布局（不需要改）
title:      Minimum String Difference Problem				# 标题 
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

>DINAM-CK
>
>DYNAMIC-

>The *difference* of a positioning is then the sum of the resemblance costs of the character pairs
in each column of the positioning, as given by a resemblance matrix $R$. For an alphabet $A$, we
have that $R$ is an $(\abs{A}+ 1)\times (\abs{A}+ 1)$ matrix, as it must include the dash in addition to the $\abs{A}$
characters of the alphabet. For example, the positioning above has a difference of:

>$R[D,D] + R[I,Y] + R[N,N] + R[A,A] + R[M,M] + R[–,I] + R[C,C] + R[K,–]$

>For example, if $R[x, y] = 1$ for all $x, y \in A\cup \{–\}$ with $x \neq y$ and if $R[x, x] = 0$ 
for all $x\in A\cup {–}$, then the difference of a positioning is the number of changes; 
another resemblance matrix could store the Manhattan distances on a QWERTY keyboard between characters of the alphabet. 

>Given two strings $u$ and $r$ of possibly different lengths over an alphabet $A$ that does not
contain the dash character, and given an $(\abs{A}+ 1)\times (\abs{A}+ 1)$ resemblance matrix $R$ of integers,
which cannot be assumed to be symmetric, perform the following tasks:

>A. Give a recursive equation for the minimum difference of $u$ and $r$, including the semantics
of all the parameters. Use it to justify that dynamic programming is applicable to the
problem of computing this minimum difference.
>
>B. Design (including the choice between top-down recursive and bottom-up iterative) and
implement an efficient dynamic programming algorithm for this problem as a Python
function `min_difference_align(u, r, R)` to return both the number and a positioning for the minimum difference, 
assuming that the last row and last column of $R$ pertain to the dash character. 

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

*B. Algorithm design: returning the positioning of minimum difference*
For the `min_difference(u, r, R)` problem and considering the equation given above, we create a top-down dynamic 
programming algorithm, and use memoization to reduce complexity. For that purpose, we define a matrix $A$ of size 
$[u.length+1,r.length+1]$, which will save the values calculated for each sub_problem. The following algorithm is 
designed: 
```
min_difference(u, r, R):
    A[u.length+1][r.length+1]=$\emptyset$   
    min_dif(i,j):
        if i<0 or j<0: result = inf
        else if A[i][j]=$\emptyset$:
            if i=0 and j=0:
                result=0
            else:
                result=min(
                    min_dif(i-1,j)+R[u[i-1]][-], 
                    min_dif(i,j-1)+R[-][r[j-1]], 
                    min_dif(i-1,j-1)+R[u[i-1]][r[j-1]], 
                    )
            A[i][j]=result
        else: result=A[i][j]
        return result
    
    A[n][t]=min_dif(u.length,r.length)
    return A[u.length][r.length]
```
Executing the algorithm described will return the minimum difference between the given two strings $u$ and $r$.

Since we have designed to solve the problem of returning the minimum number of differences, we can further 
extend that in order to return the positioning. For this purpose, a function named `trace_letter` is defined 
to retrieve `checked_u` and `checked\_r` using backtracking, based on the values already calculated.

This is done by simply backtracking to the previous letter of one or both of the strings, depending on the value found 
on the according positions calculated for $(i,j)$ of our dynamic programming matrix $A$, and the matching matrix $R$ 
for the alphabet. 

If we check the value at the position $A[i,j]$, then three possibilities arise:
(1). the value comes from $A[i-1][j] + R[u[i-1]]['-']$, then we add the letter at $u[i-1]$ into `checked_u` and
$\mbox{-}$ into `checked_r`; 

(2). the value is from $A[i][j-1] + R['-'][r[j-1]]$, then we add a hyphen into `checked_u` and 
the letter $r[j-1]$ into `checked_r`; 

and (3). the value comes from $A[i-1][j-1] + R[u[i-1]][r[j-1]]$, then we add both $u[i-1]$ and $r[j-1]$ 
into `checked_u` and `checked_r` accordingly.

The function follows: 

```
trace_letter(i,j):
    if i<1 or j<1:
        checked_u=j*'-'+u[0:i]
        checked_r=i*'-'+r[0:j]
    else: 
        cases=(
            (A[i-1][j] + R[u[i-1]]['-'], 
             A[i][j-1] + R['-'][r[j-1]],
             A[i-1][j-1] + R[u[i-1]][r[j-1]])
            )
        true_case=min(cases).index
        if true_case=0: 
            checked_u, checked_r = trace_letter(i-1, j)
            checked_u = checked_u + u[i-1]
            checked_r = checked_r + '-'
        else if true_case=1: 
            checked_u, checked_r = trace_letter(i, j-1)
            checked_u = checked_u + '-'
            checked_r = checked_r + r[j-1]
        else: 
            checked_u, checked_r = trace_letter(i-1, j-1)
            checked_u = checked_u + u[i-1]
            checked_r = checked_r + r[j-1]
return checked_u, checked_r
```

To retrieve the solution, we start from $A[\abs{u},\abs{r}]$. When one of the strings reaches its end, 
we complete the rest of the results with hyphens.

The implementation of our algorithm follows below,

```
def min_difference_align(u,r,R):
    """
    Sig:  string, string, int[0..|A|, 0..|A|] ==> int, string, string
    Pre: u and r are strings to be compared, while R is the resemblance matrix
    Post: the function returns the minimal differences between two given strings, and the positioning for the
          minimum differences.
    Ex:   Let R be the resemblance matrix where every change and skip costs 1
          min_difference_align("dinamck","dynamic",R) ==>
                                    3, "dinam-ck", "dynamic-"
    """
    ## Matrix A is the result matrix, as it acts in the previous function.
    A = [[None for i in range(len(r)+1)] for j in range(len(u)+1)]
    def min_dif(i, j):
        """
        Sig:  int, int --> int
        Pre:  i, j non negative integers, positions of the string to be checked.
        Post: the function returns the minimal differences for the         subproblem opt(i,j)
        Ex:  
        """

        ## if both strings reduce to empty, return distance between '-' and '-'
        if i < 0 or j < 0: result = float('inf')
        ## if the length of one sub_string (eg. i) to be compared reduce to 0 while another does not,
        # value 'inf' forces j = j-1 and i keeps 0.
        elif A[i][j] is None:
            if i == 0 and j == 0:
                result = R['-']['-']
            else:
                ## Implementation of OPT cases
                result = min(
                    # Recursion variant: i decreases
                    min_dif(i - 1, j) + R[u[i - 1]]['-'],
                    # Recursion variant: j decreases
                    min_dif(i, j - 1) + R['-'][r[j - 1]],
                    # Recursion variant: i,j decrease
                    min_dif(i - 1, j - 1) + R[u[i - 1]][r[j - 1]]
                )
            A[i][j] = result
        else: result = A[i][j]
        return result

    def trace_letter(i, j):
        """
        Sig:  int, int --> string, string
        Pre:  i, j non negative integers, positions of the string to be checked.
        Post: the function returns the proposed positionings for a substring of u having 
                length i and a substring of r having length j.
        Ex:   u="dinamck" r="dynamic"
              trace_letter(0,1)="-","d"
              trace_letter(2,2)="di","dy"
              trace_letter(3,4)="din","dyn"
              trace_letter(7,7)="dinam-ck","dynamic-"
        """

        if i < 1 or j < 1:
            ## if edge of matrix reached complement with hyphens. eg. i = 0 and j is not,
            ## for the 1st string, return j hyphens and for the 2nd string, return first jth chars of original string.
            checked_u = j*'-' + u[:i]
            checked_r = i*'-' + r[:j]
        else:
            ## list cases contains three different cases and variable true_case indicates from which case value in
            ## current position is (in another word, which case has the minimum value)
            cases = (
                A[i - 1][j] + R[u[i - 1]]['-'], 
                A[i][j - 1] + R['-'][r[j - 1]],
                A[i - 1][j - 1] + R[u[i - 1]][r[j - 1]]
            )
            true_case = cases.index(min(cases))
            ## Recursion path depends on case, following solution from OPT
            if true_case == 0:
                # Recursion variant: i decreases by 1
                checked_u, checked_r = trace_letter(i - 1, j)
                checked_u = checked_u + u[i - 1]
                checked_r = checked_r + '-'
            elif true_case == 1:
                # Recursion variant: j decreases by 1
                checked_u, checked_r = trace_letter(i, j - 1)
                checked_u = checked_u + '-'
                checked_r = checked_r + r[j - 1]
            else:
                # Recursion variant: both i,j decrease by 1
                checked_u, checked_r = trace_letter(i - 1, j - 1)
                checked_u = checked_u + u[i - 1]
                checked_r = checked_r + r[j - 1]
        return checked_u, checked_r

    # Recursion variant: number of letters checked in u,r decrease.
    A[len(u)][len(r)] = min_dif(len(u), len(r))
    # Recursion variant: number of letters checked in u,r decrease.
    checked_u, checked_r = trace_letter(len(u), len(r))

    return A[len(u)][len(r)], checked_u, checked_r
```

*C. Time Complexity Analysis*
Our algorithm firstly needs to calculate all values in the matrix A, which is of size $[\abs{u}+1,\abs{r}+1]$. 
In spite of doing the calculation recursively, all values are being calculated at most once. 
For each calculation we have a constant time of $\mathcal{O}(1)$, so in the worst case scenario, 
its time complexity is $\mathcal{O}(\abs{u}\times \abs{r})$.

For the backtracking function in the extended algorithm, we need to retrace values from 3 possibilities in our matrix $A$.
Again, for each cell a check is performed at most once, which takes a constant time of $\mathcal{O}(1)$, 
despite calling the function recursively. Therefore, in the worst case the backtracking algorithm has a time 
complexity of $\mathcal{O}(\abs{u}\times \abs{r})$.