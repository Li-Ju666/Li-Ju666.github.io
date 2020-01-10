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

Select $\delta_<condition> (R)$: select records that fulfill condition from relation $R$; 

Project $\Pi_<attributes> (R)$: project certain attributes of all records from relation $R$; 

Natural join $A\bowtie B$: join relation $A$ and $B$ naturally by their same-name attributes; 

Equijoin $A\bowtie _<equal\_condition> B$: join relation $A$ and $B$ by the equal condition given; 

Thetajoin $A\bowtie _<general\_condition> B$: join relation relation $A$ and $B$ by a more general condition given; 

Query tree: use a tree structure to visualize query process. In the tree, two-branch structure stands a join, one-branch
means select, the root stands for project and the leaves are relations. 