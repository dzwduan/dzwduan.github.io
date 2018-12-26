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

# 前向传播

一般的思想是每次都计算一下梯度，不过这样会很繁琐，在考虑反向传播的时候还要考虑前向传播。

![](img\forward0.PNG)

一种改进如下，根据相邻结点来求梯度，即链式法则。

![](img\forward.png)

他的算法如下：

![](img\autodiff2.png)

