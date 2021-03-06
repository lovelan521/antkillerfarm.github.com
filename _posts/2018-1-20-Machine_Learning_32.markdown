---
layout: post
title:  机器学习（三十二）——XGBoost
category: ML 
---

# XGBoost

XGBoost是陈天奇于2014年提出的一套并行boost算法的工具库。

>注：陈天奇，华盛顿大学计算机系博士生，研究方向为大规模机器学习。上海交通大学本科（2006～2010）和硕士（2010～2013）。   
>http://homes.cs.washington.edu/~tqchen/

论文：

《XGBoost: A Scalable Tree Boosting System》

参考文献中的部分结论非常精彩，摘录如下。

从算法实现的角度，把握一个机器学习算法的关键点有两个，一个是loss function的理解(包括对特征X/标签Y配对的建模，以及基于X/Y配对建模的loss function的设计，前者应用于inference，后者应用于training，而前者又是后者的组成部分)，另一个是对求解过程的把握。这两个点串接在一起构成了算法实现的主框架。

GBDT的求解算法，具体到每颗树来说，其实就是不断地寻找分割点(split point)，将样本集进行分割，初始情况下，所有样本都处于一个结点（即根结点），随着树的分裂过程的展开，样本会分配到分裂开的子结点上。分割点的选择通过枚举训练样本集上的特征值来完成，分割点的选择依据则是减少Loss。

XGBoost的步骤：

I. 对loss function进行二阶Taylor Expansion，展开以后的形式里，当前待学习的Tree是变量，需要进行优化求解。

II. Tree的优化过程，包括两个环节：

I). 枚举每个叶结点上的特征潜在的分裂点

II). 对每个潜在的分裂点，计算如果以这个分裂点对叶结点进行分割以后，分割前和分割后的loss function的变化情况。

因为Loss Function满足累积性(对MLE取log的好处)，并且每个叶结点对应的weight的求取是独立于其他叶结点的（只跟落在这个叶结点上的样本有关），所以，不同叶结点上的loss function满足单调累加性，只要保证每个叶结点上的样本累积loss function最小化，整体样本集的loss function也就最小化了。

**可见，XGBoost算法之所以能够并行，其要害在于其中枚举分裂点的计算，是能够分布式并行计算的。**

官网：

https://xgboost.readthedocs.io/en/latest/

GitHub：

https://github.com/dmlc/xgboost

中文文档：

http://xgboost.apachecn.org/cn/latest/

编译：

{% highlight java %}
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost; make -j4
{% endhighlight %}

python安装：

`cd python-package; sudo python setup.py install`

XGBoost提供了两种接口：普通接口和sklearn接口。后者的示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/hello/decision_tree.py

## 参考

https://www.zhihu.com/question/41354392

机器学习算法中GBDT和XGBOOST的区别有哪些？

http://blog.csdn.net/sb19931201/article/details/52577592

xgboost入门与实战

https://mp.weixin.qq.com/s/x06axCC1ZTgezqEYjjNIsw

Xgboost初见面

https://mp.weixin.qq.com/s/f3QVbJiC6gLEptKwFZ-7ZQ

竞赛大杀器XGBoost，你还可以这样玩

http://blog.csdn.net/u013709270/article/details/78156207

Python机器学习实战之手撕XGBoost

https://mp.weixin.qq.com/s/xHVkc1NP2oodU7Hb0Xb_jA

为什么XGBoost在机器学习竞赛中表现如此卓越？

https://mp.weixin.qq.com/s/pn_qn6uRz2-9DmAK4sp35g

史上最详细的XGBoost实战（上）

https://mp.weixin.qq.com/s/xzZvIX0QaCPNSyGfHT3beQ

史上最详细的XGBoost实战（下）

https://mp.weixin.qq.com/s/T3NgIuGZIvmPSMNFyQeeGw

XGBoost原理解析

https://mp.weixin.qq.com/s/JYPnzgzBMSGx09ltBtfwqg

理解XGBoost机器学习模型的决策过程

http://www.cnblogs.com/qcloud1001/p/7542128.html

小巧玲珑：机器学习届快刀XGBoost的介绍和使用

https://mp.weixin.qq.com/s/__ESveAdBS9KJf26R3EMVA

对比TensorFlow提升树与XGBoost：我们该使用怎样的梯度提升方法

