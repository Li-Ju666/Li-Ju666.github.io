---
layout:     post   				    # 使用的布局（不需要改）
title:      Neural Network-basic concepts and implementation in C				# 标题 
subtitle:   
date:       2020-07-09 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    Cloud_Computing Machine_leaning
---
This article will introduce some very basic concepts of neural network and implement a simple single-layer
neural network in C. 

## What is neural network 
Neural network is basically a statistical classification or regression model, and nothing more. Like other models like
Support Vector Machine (SVM) and linear regression, with a training dataset, a model can be created. Based on the 
model, whenever a new sample is given, the new sample will be fed in the model and a prediction will be made. 

Of course, comparing with other statistical models, neural network have its own pros and cons: 
#### Pros: 
1. Neural network is very flexible, thus it can be used in many different scenarios including both linear and nonlinear
relations; 
2. Almost no assumptions for data are required for neural network. For instance, linear discrimination analysis is
commonly used to classify data, however the distribution of its input data should be normal (gaussian), otherwise it
is incorrect. However for neural network, such assumptions are not required; 
3. Neural network enjoys a great predicting ability, or we can say, it is generalizable. 

#### Cons: 
1. Building a neural network is costly in computation; 
2. It is always hard to explain neural network models how they perform well. In some real life, it is often not enough
to do prediction only. More importantly, one need to clarify why the prediction is correct. Neural network always lack
such ability (interpretability); 
3. Flexibility is also a con for neural network: there are so many different architectures and functions to choose from
to build a neural network for a specific problem, so it is always a problem: how to build a proper network for the
facing problem. 

## Architecture
Neural network is constructed by layers. Each layer is basically an operator, with 3 elements, weight matrix $W$, 
bias term $B$ and an activation function $\sigma$. The input of layer $i$ is actually the output of previous layer 
$i-1$. Data flow into network layer by layer (operated by a series of operators) and finally networks will produce an
output matrix. To quantify how biased the output is away from target output, error $E$ is also defined by an error 
function, taking output matrix and target output matrix as arguments. 

If you google "neural networkeras", tons of example codes will be found, and you could easily "build your own neural
network" with keras API within several lines. However, for a guy who is studying in SCIENCE, rather than calling some 
high-level functions in which you never know what is going on， mathematics always makes me feel safe about what I am 
doing. To make our lives easier, a simple fully-connected network will be explained in mathematical details. One 
can easily and the simple one into more complex network. 

Following notations will be used:
<<<<<<< HEAD
#### Forward propagation
How neural network is dealing with input data is basically represented as following equation: 
![avatar](/img/20-07-09/NNequation.png)

The inner part $\sigma^1(\mat X_{m\times n} \cdot \mat W^1_{n\times n^1} + \mat B^1_{1\times n^1}$ is the output of 
the first layer: the input matrix is multiplied by the weight matrix of the 1st layer and also the bias matrix is 
added. Then with the weighted matrix $Z^1$, the activation function will operated on $Z^1$ element-wise and
finally output matrix $A^2$ is obtained, which has the same dimension $m\times n^1$ as matrix $Z^1$. 
A Small point should be paid attention to that the dimension of mat $W\cdotX$ is $m\times n^1$, while the dimension
of bias term is $1\times n^1$. Commonly matrices with different sizes are unable to be added, however here it is
actually not ordinary sum: the bias vector $B$ is added to each row of matrix $W\times X$ row-wise. Therefore the 
output matrix of the 1st layer $A^2$ is of dimension $m\times n^1$. 

Likewise, $A^2$ is fed into the 2nd layer and multiplied then added by $W$ and $B$ respectively. Finally with
element-wise operation be activation function, the output matrix $A^3$ will be obtained with dimension of 
$m\times n^2$. Here we can see that the output of layer $i$ is of dimension $m\times n^i$: the column number of matrix
is varying according to the number of nodes of each layer, while the number of rows keeps the same, which is the number
of input samples. The dimension is actually change by the weight matrix $W^i$ of layer $i$. 

For the last layer, things are a bit different: the number of nodes should be fixed as the dimension of target output. 
To be more precise, for single-value regression task, there should be only one node in the last layer, and for 
multi-class classification, the number should be the number of categories of output. This ensure the dimension of
output matrix keeps the same with target matrix: $m\times n$. 

The whole procedure is actually propagating data from the top of the network layer by layer, or we can call this
forward propagation. 

#### Error definition
To compare how far the output matrix is away from target output, error function is defined to get a scalar named 
error $E$. Here mean square error is used:
![avatar](/img/20-07-09/Errorfunc.png)

#### Howto learn? 
Like other statistical models, the "training" of neural network is actually the process of looking for $W$s and $B$s 
that minimize the error $E$. Ideally, we need to find every $W$ that makes $dE/dW=0$ and $B$ that makes $dE/dB=0$. 
However in this problem, it is almost impossible to do so for 2 reasons: 1. most neural network are multi-layer, which
makes it very hard to find the equation of $dE/dW$ for every $W$ (and $B$s of course). 2. The variables we are facing 
are all matrices but not scalar, which makes it far more complex to find deviation functions. Therefore, gradient 
descending is introduced. 

=======
#### Initialization
>>>>>>> e2aadfef58b697d6330b72c5b20d839a93398d07


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


