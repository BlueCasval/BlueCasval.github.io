---
layout:     post
title:      Frank-Wolfe算法
subtitle:   
date:       2019-11-28
author:     shine
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 优化理论
---

本文内容来自Martin Jaggi的[论文](http://proceedings.mlr.press/v28/jaggi13.html),参考[博文](http://fa.bianp.net/blog/2018/notes-on-the-frank-wolfe-algorithm-part-i/)。

## The Frank-Wolfe 算法

Frank-Wolfe (FW)算法是一种用于求解非线性约束优化问题的古老算法，但是近年由于其低内存需求和无投影迭代，又重新引起了人们的兴趣。它可以解决一下形式的问题
$$
minimize_{\boldsymbol{x} \in \mathcal{D}} f(\boldsymbol{x}) ~
$$
其中$f$是可微的，可以直观地解释为目标函数必须“平滑”，即不能有任何扭结或间断。 而定义域$D$是一个凸集，FW算法不要求目标函数$f$是凸集。

Frank-Wolfe是一种非常简单的算法，给定初始假设值$x_0$构造一系列估计值$\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots$。它们会收敛于优化问题的解。该算法定义如下：

![Frank-Wolfe algorithm](img/frank-wolf.png)

与其他受约束的优化算法（例如投影梯度下降）相反，Frank-Wolfe算法不需要进行投影，因此为什么有时将其称为无投影算法。算法[示意图](https://twitter.com/gabrielpeyre/status/945210545166258176)如下：

![Frank-Wolfe algorithm on a toy problem](img/FW_iterates.png)

Frank-Wolfe算法主要分为两个部分，第一部分为解决定义域上的线性问题
$$
\quad\boldsymbol{s}_t \in argmax_{\boldsymbol{s} \in \mathcal{D}} \langle -\nabla f(\boldsymbol{x}_t), \boldsymbol{s}\rangle\label{eq:lmo}\\
$$
该算法的其余部分主要涉及寻找合适的步长以在下降方向$s_0 - x_0$上移动。**Variant 1**与**Variant 2**表示了2种步长选择策略。

## 收敛理论

**定理1：**一般目标的收敛率。如果$f$是$L$-Lipschitz梯度可微，则在最佳Frank-Wolfe间隙上有以下$\mathcal{O}(1/\sqrt{t})$界：
$$
\begin{equation}
  \min_{0 \leq i\leq t} g_i \leq \frac{\max\{2 h_0, L \ diam(\mathcal{D})^2\}}{\sqrt{t+1}}~,
  \end{equation}
$$
其中$h_0 = f(x_0) - \min_{x \in \mathcal{D}} f(x)$是初始全局次优。

**定理2：**凸目标的收敛率。如果$f$是凸的并且$L$-Lipschitz梯度可微，那么对于函数次优解我们有以下收敛率：
$$
\begin{equation}
f(x_t) - f(x^\star) \leq \frac{2 L \ diam(\mathcal{D})^2}{t+1}
\end{equation}
$$
