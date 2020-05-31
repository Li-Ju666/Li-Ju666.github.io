---
layout:     post   				    # 使用的布局（不需要改）
title:      Overview for Federated Machine Learning				# 标题 
subtitle:   
date:       2020-05-31 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    Cloud Computing
---
This article will summarize federated machine learning. 

## Background
Machine learning has increasingly been a hot topic in both academia and industrial, due to its great performance in 
modelling and prediction[[1]](https://mitpress.mit.edu/books/introduction-machine-learning). 
Classic machine learning requires centralized datasets for training, or more 
precisely, training machines must have access to all required data[[1]](https://mitpress.mit.edu/books/introduction-machine-learning). 
However, in real life, class methods are facing two major challenges: data privacy and storage/transmitting problems
[[2]](https://arxiv.org/abs/1902.04885). Due to commercial competence and data privacy policies, 
it is often hard, even illegal to share data between different parties[[3]](https://eur-lex.europa.eu/legal-content/EN/TXT). 
Even if data sharing is allowed, data integration will still cost a lot when traning datasets are enormous. Therefore, 
federated machine learning is introduced to solve this problem. 

## Definition
Define $N$ data owners 
$\{\mathcal{F}_1, \dots \mathcal{F}_N\}$
, all of whom wish to train a machine-learning model by 
consolidating their respective data 
$\{\mathcal{D}_1, \dots \mathcal{D}_N\}$
. A conventional method is to put all data 
together and use 
$\mathcal{D} = \mathcal{D}_1\cup \dots \cup \mathcal{D}_N$

to train a model $N$ and $\mathcal{M}_{\text{SUM}}$. 

$\mathcal{M}_{\text{SUM}}$.

A federated-learning system is a learning process in which the data owner collaboratively train a model 
$\mathcal{M}_{\text{FED}}$
, in which process any data owner 
$\mathcal{F}_i$ 
does not expose its data 
$\mathcal{D}_i$
to others. In addition, the accuracy of 
$\mathcal{M}_{\text{FED}}$
, denoted as 
$\mathcal{V}_{\text{FED}}$
, should be very close to the performance of 
$\mathcal{V}_{\text{SUM}}$
, which denotes the 
accuracy of 
$\mathcal{M}_{\text{SUM}}$
. Formally, let 
$\delta$
be a non-negative real number, if 
$\mathcal{V}_{\text{FED}}-\mathcal{V}_{\text{SUM}}|<\delta$
we say that the federated learning algorithm has 
$\delta$
-accuracy loss[[2]](https://arxiv.org/abs/1902.04885). 

## Categorization
According to features of distributed datasets, federated machine learning could be categorized into three different types, 
horizontal federated learning, vertical federated learning and federated transfer learning[[2]](https://arxiv.org/abs/1902.04885). 

**Horizontal federated learning**

Horizontal federated learning is introduced in the scenarios in which datasets share similar feature space but different 
sample spaces. The Gboard test is a typical example of horizontal federated learning: data of users have exactly the 
same features but samples vary[[4]](https://ai.googleblog.com/2017/04/federated-learning-collaborative.html).

To construct a horizontal federated learning model, 5 major steps are required: 
1. Participants locally train their local model and send masked results to the server; 
2. The server platforms secure aggregation withoput learning information about any participant; 
3. The server pushes back the aggregated results to participants; 
4. Participants update their respective model with the global model and test as-obtain global model; 
5. repeat 1-4. 

**Vertical federated learning**

Vertical federated learning is appicable to the cases in which datasets share the same sample ID space but differ in 
feature space. An example is that in a certain region, data in the local bank and local supermacket are the scenarios 
of this type: they have similar sample IDs (data of local residents) but different features. 

To construct a vertical federated learning model, 4 major step are required: 
1. Collaborator C creates encryption pairs and sends a public key to local machine A and B; 
2. A and B encrypt and exchange the intermediate results for local models; 
3. A and B compute encrypted gradients and add an additional mask, respectively. A and B also compute encrypted loss and send encrypted values to C; 
4. C decrypts and sends the decrypted gradients and loss back to A and B. A and B unmask the gradients and update the model parameters coordingly. 

**Federated transferring learning**

Federated transfer learning applies to the scenarios in which two datasets differ not only in samples but also feature space. 

## Related conceptions
Federated machine learning enables multiple parties to collaboratively construct a machine learning model while keeping 
data protected. It is rooted on many existing topics and sometimes is confusable with some concepts. 

**Federated learning and distributed machine learning**

Federated learning is similar to distributed machine learning at the first sight. However, federated machine learning 
faces a more complex environment. Distributed machine learning is aimed to train a model distributedly, which means 
during the training process, the central training nodes have full access to all datasets like a master-slave structure. 
However, federated machine learning does not has this feature due to data-privacy protection and all participants are 
also free to join or leave the alliance anytime. 

**Federated learning and edge computing**

Federated learning can be seen as an application of edge computing. Many training work is done locally and central 
server aggregate local models, optimize the global model and push it to edges. 

**Federated learning and differential privacy**

The most significant feature of federated learning is data-privacy protection, which looks similar to differential 
privacy. However, they have totally different mechanisms. Differential privacy exchange user data with encrption 
methods, while federated learning does not exchange local data: only some local model parameters exchange and updating 
are required. Under some strict data-privacy policies, data exchange with differential privacy might be illegal, while 
federated learning will not face such problems at all. 

## Challenges
Though federated learning has many advantages, it faces challenges as well:

1. The diversity in datasets at different parties will affect the the performance of global model, including 
distribution of different features, unbalanced numbers of samples and even different representation of data;
2. Classic machine models are able to provide solid prediction, however, the performance of aggregated models are not 
guaranteed to be solid enough. Therefore, further investigation on algorithms of model aggregation is required;
3. Classic training algorithms require low-latency, high-throughput communications, however it will be very costly for 
federated learning scenarios, in which data are stored distributedly. Therefore algorithms that take few iterations of 
high-quality update are required to reduce communication cost[[4]](https://ai.googleblog.com/2017/04/federated-learning-collaborative.html). 

## Conclusion
Due to data-privacy protection, the size of data and commercial competence, data isolation has become a huge barrier 
for better performance of machine learning model. But federate learning is a novel approach to solve this problem and 
it has attracted many interests of researchers. In this summary, we have generally discussed the basic definition, 
categorization and faced challenges of federated machine learning. We are expecting that federated machine learning 
become a mature technology to share knowledge with safety within parties and benefit all its alliance members. 


