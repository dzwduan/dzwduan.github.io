```yaml
---
layout:     post
title:      autodiff实现
subtitle:   iOS定时器详解
date:       2018-12-26
author:     Dzw
header-img: img/autodiff.png
catalog: 	 true
tags:
    - cse599w
    - autodiff
---
```

# Autodiff算法的思想

一般的思想是每次都计算一下梯度，不过这样会很繁琐，在考虑反向传播的时候还要考虑前向传播。

![](http://dzwduan.github.io/img/forward0.PNG)

一种改进如下，根据相邻结点来求梯度，即链式法则。

![](http://dzwduan.github.io/img/forward.png)

他的算法如下：

![](http://dzwduan.github.io/img/autodiff2.png)



```text
令最后结果out的grad=1,
获取nodes列表
```

![img](https://pic1.zhimg.com/80/v2-a19f3bdb31496252f096ab9256a2b070_hd.jpg)

```text
从后向前遍历nodes列表：
     梯度=相邻输出边的部分和相加
     输入的梯度=各自对应的输入节点梯度
     输入梯度的节点添加进node列表
```

![img](https://pic1.zhimg.com/80/v2-3c5055b47fa91a392bec7c67a7baab54_hd.jpg)

![img](https://pic2.zhimg.com/80/v2-c7e2d15d3fbd688a9a720ad5ef053209_hd.jpg)

> 下面结合上图来讲一下。node_*to_grad列表里面共包含如下。*左边是输入,右边是对应的梯度， ![X_{2}](https://www.zhihu.com/equation?tex=X_%7B2%7D) 传值给了 ![X_{3}](https://www.zhihu.com/equation?tex=X_%7B3%7D) 和 ![X_{4}](https://www.zhihu.com/equation?tex=X_%7B4%7D) 所以有两个梯度，需要进行累加。
> ![\bar{X_{4}}](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B4%7D%7D) = 1
> ![X_{4}](https://www.zhihu.com/equation?tex=X_%7B4%7D) = ![X_{3}](https://www.zhihu.com/equation?tex=X_%7B3%7D) * ![X_{2}](https://www.zhihu.com/equation?tex=X_%7B2%7D) ==> ![dX_{4}/dX_{2}](https://www.zhihu.com/equation?tex=dX_%7B4%7D%2FdX_%7B2%7D) = ![X_{3}](https://www.zhihu.com/equation?tex=X_%7B3%7D) , ![dX_{4}/dX_{3}=X_{2}](https://www.zhihu.com/equation?tex=dX_%7B4%7D%2FdX_%7B3%7D%3DX_%7B2%7D) 
> ==> ![\bar{X_{2}}^1](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B2%7D%7D%5E1) = ![ X_{3}](https://www.zhihu.com/equation?tex=+X_%7B3%7D) , ![\bar{X_{3}}](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B3%7D%7D) = ![X_{2}](https://www.zhihu.com/equation?tex=X_%7B2%7D) ,
> ![X_{3}](https://www.zhihu.com/equation?tex=X_%7B3%7D) = ![X_{2} + 1](https://www.zhihu.com/equation?tex=X_%7B2%7D+%2B+1) ==> ![\bar{X_{2}}^2](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B2%7D%7D%5E2) = 单位矩阵
> ![\bar{X_{2}}= \bar{X_{2}}^1+\bar{X_{2}}^2](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B2%7D%7D%3D+%5Cbar%7BX_%7B2%7D%7D%5E1%2B%5Cbar%7BX_%7B2%7D%7D%5E2) 
> ![X_{2} = e^{X_{1}}](https://www.zhihu.com/equation?tex=X_%7B2%7D+%3D+e%5E%7BX_%7B1%7D%7D) ==> ![dX_{2}/dX_{1}=X_{2}](https://www.zhihu.com/equation?tex=dX_%7B2%7D%2FdX_%7B1%7D%3DX_%7B2%7D) 
> ==> ![\bar{X_{1}}](https://www.zhihu.com/equation?tex=%5Cbar%7BX_%7B1%7D%7D) = ![X_{2}](https://www.zhihu.com/equation?tex=X_%7B2%7D)

## 总结：

backprop和autodiff的对比还是很直观的。backprob只能按照前向传播原路返回,autodiff通过构造计算图可以一次遍历就能求出梯度，通过层层递进的方式进行梯度累加，更加直观，计算也更加方便。

![img](https://pic3.zhimg.com/80/v2-5a028b13fae01de0903bbcc08a3da34a_hd.jpg)

参考论文[[1502.05767\] Automatic differentiation in machine learning: a survey](https://arxiv.org/abs/1502.05767)