---
layout:     post   				    # 使用的布局（不需要改）
title:      Optimization of Database III				# 标题 
subtitle:   Query Process
date:       2020-01-10 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database
---
In this series of articles, the mechanism behind the database together with its optimization will be introduced. 

# Query Process #
In database management system, when you input a query statement, what does the system actually do to fetch the records
you need? This article will discuss this question. 

Before discussion, following are the notations we will use and term explanations in this article: 

Select $\delta_{<\text{condition}>} (R)$: select records that fulfill condition from relation $R$; 

Project $\Pi_{<\text{attributes}>} (R)$: project certain attributes of all records from relation $R$; 

Natural join $A\bowtie B$: join relation $A$ and $B$ naturally by their same-name attributes; 

Equijoin $A\bowtie _{<\text {equal\_condition}>} B$: join relation $A$ and $B$ by the equal condition given; 

Thetajoin $A\bowtie _{<\text{general\_condition}>} B$: join relation relation $A$ and $B$ by a more general condition given; 

natural join $\subset$ equijoin $\subset$ thetajoin $=$ inner join

Query tree: use a tree structure to visualize query process. In the tree, two-branch structure stands a join, one-branch
means select, the root stands for project and the leaves are relations. 

---------------------------------------------------------------------

The query process mainly are 3 steps, translation, evaluation and actual process. In translation step, the query statements
are translated into relational algebra statements, then the system will evaluation the cost of different ways to fetch required
records, and finally return the records with the cheapest way. 

As we can see, in all 3 steps, the evaluation step is what we actually concern about. Problems arise in this step:
how can we get different ways to fetch records and how does system visit records via each approach, and how to evaluate 
the cost of each approach? 

Before answering these questions, we have to clarify, how do we define "good performance"? From the 
[first article](https://li-ju666.github.io/2020/01/01/Optimization-of-database/), we know that disk access is 
the bottleneck of database speed, it is also the key metrics we concern here. In this article, all our metrics for 
performance is based on the number of block transfer. 

Then, as we all know, every single query statement can be divided into a series of operations like selection, join, project, etc. 
Before investigation about the whole query, we have to know how system deal with each operation. 

### selection operation ###
Selection is to select a subset of records from a relation which fulfill given conditions. There are a bunch of methods
for selection. All indices referred below are assumed as b+ tree. 
 
**A1 -- linear file scan**: 

This is the simplest way to do selection, scanning records from the first one to the last one following the physical order
to check if they fulfill the condition one by one. 

Cost: For unique attributes: #blocks/2 and for non-unique attributes: #blocks

**A2 -- primary index (equality condition on unique attribute)**

To visit the record with given key value, we follow the multilevel b+ tree and can access the record directly. 

Cost: height(index tree height)+1(from the index to record)

**A3 -- clustering index (equality condition on non-unique attribute)**

To visit records with given non-key value, we follow the multilevel b+ tree and can access the first record fufilling
the condition, then following the first record and check blocks one by one till the first record that does not fulfill the 
condition. 

Cost: height(index tree height)+n(#blocks containing desired records)

**A4 -- secondary index (equality condition on non-unique attribute)**

To visit records with given non-key value, we follow the b+ tree and can access several records fulfilling the condition. 

Cost: height(index tree height)+n(#records fulfilling the condition)

ps: for secondary index which can equality condition on key attribute, it is exactly the same as A2. 

**A5 -- first index (comparison condition)**

For >=: following the tree, fetch the first record that fulfills the condition, then return all records after. If the 
condition is >, drop records that equal to given value. 

For <: following the tree, fetch the first record that fulfills the condition, then return all records before. If the 
condition is <=, also move afterward to fetch those which are equal. 

Cost: height(index height)+n(#blocks containing desired records)

**A6 -- secondary index (comparison condition)**
Do exactly as A5 does, however the cost differs. 

Cost: height(index height)+n(#desired records)

As we can see from the cost, in such case, as records are not ordered as target attribute, for each record, there should
be a block transfer, because they are stored in different stocks following no order. The cost might be extremely high 
if there are too many records that fulfill the condition. For such case, linear file scan (A1) is more applicable. 

### sorting operation ###

Though sorting seems not that important in DBSM system, actually they ARE for two reasons: 1). after projection, sometimes
users might require the relation is sorted by a certain attribute; 2). for a sorted relation, first index is applicable, 
which will make access much cheaper, and for join operation, a sorted relation will take far less cost. 

There are several ways to sort the relation. 

**Small relation**
Here we define small relation as those relations which can fit in memory. They can be applied quick sort, heap sort and 
other common used sorting algorithm to get them sorted. 

**Big relation** In contrast, we define big relation as the relations that cannot fit in our memory. For big relations, 
*external merge sort* is required. 

```
[Algorithm]
Let M denote memory size. 
1. Create sorted runs. Let i be initially. 
    Repeadedly do the following till the end of the relation: 
        a) Read M blocks of relation into memory; 
        b) Sort the in-memory blocks; 
        c) Write sorted data to Run_i. 
    Let the final value of i be N. 
2. Merge the runs: we assume that N<M. 
    (1) Use N blocks of memory to buffer input runs, and 1 block to buffer output. 
        Read the first block of each run into its buffer page; 
    (2) repeat: 
        a) select the first record in sort order among all the buffer pages; 
        b) write the record to the output buffer. If the output buffer is full write
           it to disk; 
        c) delete the record from its input buffer page. If the buffer page becomes
           empty, read the next block of the run into the buffer. 
    (3) untill all input buffer pages are empty. 

If N>=M, several merge passes are required. 
    (1) in each pass, contiguous group of M-1 runs are merged. 
    (2) a pass reduces the number of runs by a factor of M-1, and creates runs longer
        by the same factor. 
    (3) repeated passes are performed till runs have been merged into one. 
    
```
### join operation ###
There are several ways to do join operations on two relations. 

**Nested-loop join**

Nested-loop join is for theta join operation $R\bowtie _{<\text{general\_condition}>} S$. Firstly, the relation R 
is required to be read into the memory. Then read tuples of relation S one by one into the memory and compare with
all tuples of relation A one by one. Therefore, the cost of nested-loop join is: $\text{cost}=b_R+n_R*b_S$, where
$b_R$ is #blocks of R, $b_S$ is #blocks of S and $n_R$ is #records of R. 

**Block nested-loop join**

Block nested-loop join is also for theta join operation $R\bowtie _{<\text{general\_condition}>} S$. Like nested-loop
join, R is firstly read into then memory. However for relation S, data are operated in the unit of block: read a block
of S into the memory and compare each record of block with all tuples in R. Therefore, the cost of block nested-loop
is cheaper than nested-loop join: $\text{cost}=b_R+b_R*b_S$. 

**Index nested-loop join**

Index nested-loop join is for equijoin operation $R\bowtie _{<\text {equal\_condition}>} S$, where there is an tree 
index on relation S. Again all blocks of relation R are read into memory. For each record, b+ tree is used to seek 
the equal value in relation S. Therefore, the cost of index nested-loop join is 
$\text{cost} = b_R + n_R*\text{b+ tree search cost}$. 

**Merge join**

Merge join is for equijoin on relations that are sorted already on the target join attribute
$R\bowtie _{<\text {equal\_condition}>} S$. All blocks of relation R are read into memory and read block of relation
S into memory one by one. Then interleaved linear scans will encounter all these matched records. The cost of merge
join is $\text{cost}=b_R + b_S$. 

## Projection ##
Projection operation only needs to drop some columns of input and it is simple and cost little. However, 
when indicated by keyword distinct, duplicates need to be eliminated. Elimination is usually the most expensive part. 
To eliminate duplicates, commonly there are two ways: 
a) if index exists, index can be used to choose instinct records easily; 
b) if there is no index on the target attribute, partitioning (sorting or hashing) is required. In this case, the cost
will be the cost of hashing or sorting. 

------------------------------------------------------------
These are the commonly used algorithms for each basic database operations, which are of vital importance when analyzing
the cost of the whole operations. 