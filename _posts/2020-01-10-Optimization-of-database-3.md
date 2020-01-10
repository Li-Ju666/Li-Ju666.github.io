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
for selection.
 
**A1 -- linear file scan**: 

This is the simplest way to do selection, scanning records from the first one to the last one following the physical order
to check if they fulfill the condition one by one. 

Cost: For unique attributes: #blocks/2 and for non-unique attributes: #blocks

**A2 -- primary index (equality condition)**

To visit the record with given key value, we follow 