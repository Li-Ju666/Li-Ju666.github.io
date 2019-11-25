---
layout:     post   				    # 使用的布局（不需要改）
title:      Extendable Hashing				# 标题 
subtitle:    #副标题
date:       2019-11-24 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database  
    data_structure
---


## Extendable Hashing
This is the problem in the assignment 1 of Database Design 2. Here are my solutions. 
### Problem Description
> Load the records: 2369, 3760, 4692, 4871, 5659, 1821, 1074, 7115, 1620, 2428, 3943, 4750, 6975 (in the given order) into an
expandable hash file based on extendible hashing. Show the structure of the directory at each step. Show the directory at each step, and the global and local depths. 
Use the hash function h(k) = K mod 32. Assume that each bucket is one disk block and holds two records. 

### Problem Solution
The key feature of extendible hashing is: extendible as the number of data to be stored increases. The procedures are listed as following: 

1. Use hashing function to calculate data's hashing values and convert them into binary: 
{2369, 3760, 4692, 4871, 5659, 1821, 1074, 7115, 1620, 2428, 3943, 4750, 6975} $\Rightarrow$ {1, 16, 20, 7, 27, 29, 18, 11, 20, 28, 7, 14, 31} $\Rightarrow$ {'00001', '10000', '10100', '00111', '11011', '11101', '10010', '01011', '10100', '11100', '00111', '01110', '11111'}

2. Then we start to add given data to hash table one by one. Firstly we choose the first *1* digit of the binary mod value to be the key of hash blocks (so global depth is *1*), and add given data into table one by one like following figure shows: 

![avatar](/img/19-11-24/1.jpg)

As the problem tells, each block can contain 2 elements. As none of block has more than 2 elements when 4 elements added, everything looks fine. 

3. However when the 5th element is added in, block `1` is overloaded, then the hash table needs to *EXTEND*, increasing its global depth to 2, which means the first *2* digits of binary mod value are used to be the key of blocks. Thus, the tables extends as following:

![avatar](/img/19-11-24/2.jpg)

Though both block `0` and `1` need to be extended, only `1` *ACTUALLY* splited into two different blocks with 4 available positions to store data, while block `1`'s two subblock still point at one block with 2 elements. In another word, data in block `10` and `11` need first *2* digits of hash value to differentiate, while data in `00` and `01` only need one digit. Here the number of digit to differentiate data in each block is defined as the *local depth* of the block. 

4. Again we continue to add data into the hash table. When adding 1074, block `10` is full and needs to extend again. Therefore the table extends as following and global depth increase to 3: 

![avatar](/img/19-11-24/3.jpg)

5. Then when adding 7115, extension is needed again for block `000`, `001`, `010` and `011`, increasing their local deoth to 2 while global depth keeps 3: 

![avatar](/img/19-11-24/4.jpg)

6. We continue to add 1620 and 2428. For 2428, block `110` and `111` need to extend as follows: 

![avatar](/img/19-11-24/5.jpg)

7. When adding 3943, extension is applied as following: 

![avatar](/img/19-11-24/6.jpg)

8. For 6975, the whole table need to extend to global depth of 4: 

![avatar](/img/19-11-24/7.jpg)

If two more elements, 4981 and 9208 are added, the table will be like what following shows: 

![avatar](/img/19-11-24/8.jpg)



