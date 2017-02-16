---
title: Finding a feasible course schedule using Tabu search(2)
date: 2016-03-16 22:40:50
tags: 论文翻译
description: "论文翻译：基于遗传禁忌算法解决排课问题"
categories: 论文
---
## 文章地址[Finding a feasible course schedule using Tabu search](http://xueshu.baidu.com/s?wd=paperuri%3A%28f74ffc220b033483ff393a199cb46809%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http%3A%2F%2Fwww.sciencedirect.com%2Fscience%2Farticle%2Fpii%2F0166218X92902489%2Fpdf%3Fmd5%3D94d61449ea5a57cc8289de31b6da1af9%26pid%3D1-s2.0-0166218X92902489-main.pdf&ie=utf-8)

## 翻译进度

- [#] 介绍
- [#] 问题描述
- [#] 禁忌搜索技术
- [ ] 修改禁忌搜索
- [ ] 数字化结果
- [ ] 扩展
- [ ] 结束语

-----

[上一篇链接](http://liwenquan.top/2016/03/12/基于遗传禁忌算法解决排课问题-1/)

## Tabu search techniques 
Tabu search is a metaheuristic designed for getting a global optimum to a combinatorial optimization problem. It has been first suggested by Glover [lo] and independently by Hansen et al. [l 11 for a specific application, and later developed in a more general framework. A description of the method can be found in [6, 10]. Tabu search has already been efficiently adapted to a large collection of applications [6,11-14,16] We shall sketch here the basic ideas of the technique.
An objective function f has to be minimized on a set X of feasible solutions. A neighborhood N(s) is defined for each solution s in X. The set X and the definition of the neighborhood induce a state-space graph G (which may by the way be infinite). Tabu search is basically an iterative procedure which starts from an initial feasible solution and tries to reach an optimal solution by moving step by step in the state-space graph G. Each step consists in first generating a collection V* of solutions in the neighborhood N(s) of the current solution s, and then moving to the best solution s’ in V*, even if f (s?> f (s). Let us write s’=s⊕m with the meaning that s’ is obtained by applying a modification m to s. The solutions consecutively visited in the iterative process induce an oriented path in G. Finding the best solution in V* may sometimes be a nontrivial matter. It may be necessary to solve the optimization problem min(_f(si) 1 Si E V*} by a heuristic procedure. 
A risk of cycling exists as soon as a solution s’ worse than s is accepted. In order to prevent cycling to some extent, modifications which would bring us back to a previously visited solution should be forbidden. But it may sometimes be useful to come back to an already visited solution and continue the search in another direction from there. This is realized in Tabu search by keeping a list T containing the last k modifications (k may be fixed or variable). Whenever a modification m is made for moving from s to s’, m is introduced in T and its reverse is considered as tabu. 
Deciding that some moves are tabu moves may be too absolute: it is shown in [6] that moves to solutions which have not been visited may be tabu. For this reason, it should be possible to cancel the tabu status of a move if it seems desirable to do so. This is realized as follows. Let s be the current solution and m a modification which we want to apply to s. A penalization a(s, m) and a threshold value A(s, m) are computed: if a(s, m) <A(s,m),then the tabu status of m(at s) is cancelled. We can for example define a(s,m)=f(s+m) and A(s,m)=f(s*) where s* is the best solution encountered so far: the tabu status of m is cancelled if the solution s’=s+ m is better than the previous best solution s*.The function A is called the aspiration function. 
Stopping rules have to be defined. if a lower bound f * of the minimum value of f is known then the process may be interrupted when the value of the current solution is close enough to f *, Moreover, the procedure is terminated if no improvement of the best solution s* found so far has been made during a given number nmax of iterations. 
A general description of Tabu search is given in Fig. 1. In the next section we shall describe the adaptation of Tabu search to the course scheduling problem. 
<!--more-->
## 禁忌搜索技术
禁忌搜索是一个亚启发式设计的专为解决组合优化问题得到一个全局最优解。它最先由Glover提出并因一个特定应用被Hansen独立出来，后来在一个更通用的框架中开发。该方法的描述可以在[6,10]中找到。禁忌搜索已经被有效地适应于很多应用[6,11-14,16]。我们将在这里描述该技术的基本思路。
目标函数f必须要在可行解集合X最小化。邻域N（S）是在集合X中的每一个解。集合X和邻域的定义能够得到解空间图G（可以是无限的）。禁忌搜索基本上是从一个初始可行解开始并试图通过在状态空间图G中一步步移动来达到最优解。每一步的关键在于首先在当前解s的邻域N(s)中产生一个解集V*，然后即使f (s‘)> f (s)也在V*中移动到最优解。记s’=s⊕m表示s’用过应用一个从m到s的变换。结果连续地在迭代程序中产生一个有向图。找到在V*中的最优解可能有时候是一个很不容易的事情。通过启发式过程来解决min(f(si) | Si ∈V*}的最优问题是很有必要的。
只要比s 差的结果s’被接受，那么就会存在循环风险。为了在一定程度上防止循环，可能会把我们带回之前访问过的解的变化是被禁止的。但是有时候回溯到一个已经访问过的解可能会有用，并从那里继续从另一个方向上搜索。这在禁忌搜索是通过保持一个包含了最后k（k是固定或可变的）个操作的列表T实现。每当操作m被用于从s移动到s’而取得，m 在T中引入，它的反向被认为是禁忌搜索。
确定一些动作是禁忌搜索动作可能是太绝对了：文献[6]所示，移动到还没有被访问过的解可能是禁忌的解决方案。出于这个原因，如果值得这样做做应该有可能取消一个移动的禁忌状态。实现方式如下。令s为当前解决方案，m是我们希望应用到s的变形。 
一个惩罚a(s, m)和阈值A(s,m)计算：如果a(s, m) <A(s,m)，则m的禁忌状态（在S）被取消。例如，我们可以定义a(s,m)=f(s+m)和A(s,m)=f(s*)，其中S *是目前的最好的解决方案：m的禁忌状态将被取消，如果解s’=s+ m比以前的最优解决方案S * 好.那么函数A被称为启发函数。
停止循环的规则必须定义。如果f的最小值的一个下界f*是已知的，那么该过程可以在当前解的值是接近f *时停止，此外，如果最优解s*在迭代了给定的最大次数之后目前仍没有改进，那么该过程被终止。
禁忌搜索的一般性描述图1中给出。在下一节中，我们将介绍禁忌的适应搜索到排课问题。


----

Firure1
![Figure1](http://7xruee.com1.z0.glb.clouddn.com/Figure1.png)