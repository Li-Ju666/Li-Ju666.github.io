---
layout:     post   				    # 使用的布局（不需要改）
title:      Optimization of Database II				# 标题 
subtitle:   Index structure of database
date:       2020-01-02 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database
---
In this series of articles, the mechanism behind the database together with its optimization will be introduced. 

# Index Structure
This article will introduce how various kinds of indexes are arranged and how they connect with physical records. 

From the [1st article](https://li-ju666.github.io/2020/01/01/Optimization-of-database/) of this series, we know that
in most cases, records in a file/relation are ordered/hashed by one or several specific attributes so that we can access them
in a rapid way. How can these ordered attribute achieve a fast access and if we want to access records via other attributes, 
how can we make it faster? Actually it is index that achieve this, and we can create more than one index to offer several
paths to access our database. 

**Index:** a file storing a/several attribute(s)' values and memory address(es) of their corresponding record(s) . 

## Different Types of Indexes: 
### First Index ###
1. Primary Index: Primary index is a file storing 1): each record's unique-attribute value K(i) and 
2): the pointer to the record's disk address P(i). For primary index, the physical order of records is exactly the same 
as the order of records in primary index. 

2. Clustering Index: Clustering index is a file storing 1: each non-unique attribute's distinct values A(i) and 2: the pointer 
to the value's corresponding disk address P(i) (Only one address though there may be several records). For clustering
index, the physical order of records is also exactly the same as the order of records in clustering index. 

For primary index and clustering index, we can generally call them first index or ordered index,
because the physical orders of records are the same as their order in the index. The file is only possible to follow 
one order, therefore, there is at most *ONE* first index. **For first index**, we could store them sparsely or densely, 
which is called sparse index or dense index, respectively. The word sparse or dense means: if we store every key-address
(value-address) pair in the index. As the file is ordered by the key/value, with part of key-address pairs, we still 
access them with slightly more cost. 

### Secondary Index ###

Secondary indices are also files with two attributes: an index field and the corresponding pointer
to the actual record of the original data. However, different from primary and clustering index, the physical order
of records in original data is not the same as the index field. Secondary index can also be based on unique attributes
or non-unique attributes. 

As the physical order of original data differs from the index field, it is not possible to find another record from a record. 
Therefore, every record needs a pointer in the index. So, for secondary index, only dense index is applicable. 

### Multilevel Index & B+ Tree ###
Multilevel index is a simple idea, that is create an index for the index file. Following the hierachy structure, we can
finally reach the queried record. Though we can create multilevel index with secondary indices, however to promise the
access and query speed, we defaultly regard multilevel index created with a series of first indices. B+ tree is one of 
the most commonly used multilevel index. 

The details about how to add, delete and update elements in a b+ tree is demonstrated [before](https://li-ju666.github.io/2019/11/24/B+-Tree-2019/). 
However there are few important features of b+ tree that have to be remembered: 

1. the *n* of b+ tree means the maximum pointers of a single unit, which means that at most *n-1* elements are in one unit; 
<<<<<<< HEAD
2. b+ tree is a balanced tree, and the height of the tree is no more than &\lceil {\log_{\lceil {n/2}\rceil} (K)} \rceil&, 
$K$ is the number of elements in the tree; 
3. when deleting records, the unit will "borrow" elements from its adjacent unit when the elements in the current
unit $num < \lceil {(n-1)/2} \rceil$, if "borrowing" is not inapplicable, it will combine with its adjacent unit and update
the whole tree. 
=======
2. b+ tree is a balanced tree, and the height of the tree is no more than $\lceil {\log_{\lceil {n/2}} (K)}$, K is the number
of elements in the tree; 
3. 
>>>>>>> 9b4525640ad6e1eb9eb9e4dfa69037451171a589



