---
title: Finding a feasible course schedule using Tabu search(3)
date: 2016-03-27 00:12:55
tags: 论文翻译
description: "论文翻译：基于遗传禁忌算法解决排课问题"
categories: 论文
---
## 文章地址[Finding a feasible course schedule using Tabu search](http://xueshu.baidu.com/s?wd=paperuri%3A%28f74ffc220b033483ff393a199cb46809%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http%3A%2F%2Fwww.sciencedirect.com%2Fscience%2Farticle%2Fpii%2F0166218X92902489%2Fpdf%3Fmd5%3D94d61449ea5a57cc8289de31b6da1af9%26pid%3D1-s2.0-0166218X92902489-main.pdf&ie=utf-8)

## 翻译进度

- [#] 介绍
- [#] 问题描述
- [#] 禁忌搜索技术
- [#] 修改禁忌搜索
- [ ] 数字化结果
- [ ] 扩展
- [ ] 结束语

-----

[上一篇链接](http://mxlwq.github.io/2016/03/16/基于遗传禁忌算法解决排课问题-2/)

## Adaptation of Tabu search
Many authors have formulated the course scheduling problem as an assignment
problem [3,4,9,12]. Guidelines for adapting Tabu search to assignment problems
have been described in [12]; the same paper contains a description of the algorithm
TAT1 which is an example of such an adaptation to a course scheduling problem
where starting times have to be assigned to courses. For our timetabling problem
we cannot use the algorithm TAT1 since the number of courses and their length are
not known in advance.
Let us however formulate our course scheduling problem as an assignment problem
where each element from a set S of conflicting objects is assigned to exactly one
element of a set P.
The daily quantums of a static topic are fixed in advance. They induce courses
of given length. These courses are defined to be s-objects.
<!--more-->

 
We shall consider each time period of a dynamic topic as a second kind of objects
which are called d-objects.
The set S consists of all s-objects and d-objects, and the set P consists of all
available time periods. Now, an available time period of a working day has to be
assigned to each object of S; for an s-object the time period represents the starting
time of the course. Any assignment satisfying all the constraints (a)-(i) induces a
feasible course schedule.

### The set X of feasible solutions
Eiselt and Laporte [7] have divided the requirements into hard, medium and soft
ones. The hard requirements have to be respected at all costs while soft ones are only
desirable; medium requirements should be satisfied but are nevertheless relaxable.
In most approaches, a timetable is defined to be feasible if it satisfies all hard requirements:
soft requirements are modelled as objectives while hard ones appear as
constraints. The solution methods designed for dealing with timetabling problems
usually consist of two phases: in phase I a heuristic procedure tries to find a feasible
timetable, and in phase II the current solution is improved by satisfying more
medium or soft requirements and without violating any hard one.
Let us define the following state-space graph G:
- each feasible timetable is a node in G,
- an arc links node x to node y if y is considered as an improvement of x in phase
Phase I can be viewed as a generation of a node in G, and phase II is a descent
method which moves step by step in the state-space graph G until a local optimum
is reached. For large scale timetabling problems, finding a feasible course schedule
is not an easy task. Moreover, if phase I succeeds in finding a feasible solution, this
solution and the best one are not necessarily in the same connected component of G.
We shall define a connected state-space graph G: and phase II will be replaced
by an adaptation of Tabu search techniques. We have now to define the set X of
feasible solutions, that is the nodes of the state-space graph G.
There are two ways for handling with a constraint: it can either be satisfied by
each solution of the set X of feasible solutions, or else it can be penalized in the objective
function. It is to be decided which from among all the requirements will
always be satisfied and which one will be penalized.
An object should never be assigned to a time period if such an assignment can
only induce course schedules which are not feasible. For example, each object is a
portion of a subject x and thus should never be scheduled after the due date or
before the release date of x. For this reason, constraint (e) should always be
satisfied.
On the other hand, a conflict between two objects should not be a sufficient condition
for hindering an assignment.
Constraint (a) for example should only be penalized and not always satisfied.
Guided by these ideas, it was decided to penalize constraints (a)-(d) and always
satisfy constraints (e)-(h).
Constraint (i) has been split into two distinct requirements: (i1) (respectively (i2))
requires that the minimal (respectively maximal) number of consecutive time periods
of each daily quantum of a dynamic topic is not violated.
It was decided to penalize (i1). This choice is justified by the fact that the assignment
of exactly one object is changed at each iteration of the Tabu search procedure:
for dynamic topics this means that one time period is moved at a time, and
removing from a working day a whole daily quantum with minimum value > 1 can
only be achieved by violating constraint (il). Constraint (i2) is included in the set
of always satisfied requirements since any feasible schedule can always be obtained
without violating this requirement.
For a dynamic topic, the set of d-objects which are scheduled on the same day
is defined to be a course. Let us consider a course of a dynamic topic satisfying constraint
(g). The following modifications can be obtained with several steps of the
Tabu search procedure while always satisfying constraint (g):
- the amount of consecutive time periods is not changed but the course is moved
to another portion of the working day,
- the daily quantum is increased or decreased.
These justify our decision to never violate requirement (g).
Constraint (h) has to be considered more carefully. If a static topic can only be
scheduled in a number n of different working days and if its number of daily quantums
is equal to n then constraint (h) should only be penalized since the daily quantums
are not necessarily correctly ordered, and satisfying constraint (d) can then
only be achieved by violating requirement (h). But this case is not frequent in real life
timetabling problems and it was supposed that such a situation does not occur.
Moreover, the definition of a course of a dynamic topic implies that constraint (h)
is satisfied for any dynamic topic. For these reasons, it was decided to forbid any
solution violating requirement (h).
In summary, the set X of feasible solutions contains any assignment of time
periods to the objects of S such that constraints (e), (f), (g), (h) and (i2) are satisfied.

### The objective function
For each topic t, we denote:
n(t): its number of daily quantums,
qi(t): (i= l,..., n(t)) one of its courses,
r(t): the minimum number of time periods in a daily qantum,
b(t): the first time period of course qi(t)
ei(t): the last time period of course qi(t)
B(t): the first time period of t (B(t) = min{ b,(t) |i = 1, . . . , n(t)}),
E(t): the last time period of t (E(t)=max(t) |i = 1, …,n(t))),
P(t): the set of immediate predecessors of t (if t1and t2 precede t and t1
precede t2, then only t2 belongs to P(t)).
If t is a static topic, then n(t) is fixed and qi(t) must precede qj(t) for any j>i.For a dynamic topic i, n(t) and the values ei(t) - bi(t) are variable.For two objects i and j of S, the value d(i, j) denotes the number of time periods during which i and j are held simultaneously. Since the length of an s-object may be greater than one, it follows that two objects assigned to different time periods can be in conflict. The class involved in an object i of S is denoted c(i), and t(i), is the teacher which gives the course. Let ntopic denote the number of topics.
For any solution s of X, the fallowing objective function f is defined:

The parameters p1, . . . ,p5 give relative weights to constraints (a), (b), (c), (d) and
(i1) respectiveiy. A solution s induces a feasible course schedule if and only if
f(s)=O= f *.

### The neighborhood N(s)
A solution S’ ∈ X is considered as a neighbor solution of s in X if it is obtained
by changing the assignment of exactly one object o of a topic t. Let d be the current
working day in which o is assigned. In order to satisfy constraints (g) and (h), the
set of possible modifications must be restricted to ten types which are represented
in Fig. 2:
 
- If o is an s-object, then it can be assigned to a new working day d’≠d which
does not contain any course of I (type 1), or it can be moved to another portion of
d (type 2).
- If t is a dynamic topic and o is the first or the last time period of a course, then
it can be assigned to any working day d’. If d’≠ d and d’ already contains a course
q of t, then o can be placed exactly before the first time period of q (types 3, 4)
or exactly after its last time period (types 5,6). If d’ ≠ d and d’ does not contain any
course of I, then o can be assigned to any time period of d’ (types 7, 8). If d’= d,
then the first time period of a course can be moved exactly after the last one (type
9) and the last time period can be moved before the first one (type 10).
Of course, all these modifications m are acceptable at s only if s⊕m = S’ belongs
to x.
### The other ingredients
When an object o currently scheduled on the working day d is moved to a new
time period, the pair (0, d) is introduced in the tabu list T. This means that for |T|
steps of the Tabu search procedure, it is not allowed to assign any time period of
d to 0.
The set V* is generated randomly but contains only new assignments obtained by
changing the starting time of an object which gives a strictly positive contribution
to f(s). In other words, objects which respect all the constraints are not moved.
In order to allow the tabu status of a move to be cancelled, we consider the
penalization function a(s, m) = f (s⊕m) and the aspiration function A(s,m)= f (s*).
4.适配禁忌搜索
许多作者都将排课问题形式化为分配问题[3,4,9,12]。为适应禁忌搜索到分配问题的指导方针在[12]已经描述; 包含算法的描述TAT1的相同的论文，这是开始时间必须被分配课程的一个课程调度问题的例子。对于我们的时间表问题我们不能使用算法TAT1，因为课程的数量和它们的长度是事先不知道的。
然而，让我们制定我们当然调度问题作为一个分配问题，冲突对象的集合S的每个元素被分配到一个单独的集合P的元素。
静态主题的日常量是固定的提前下。这导致课程是给定长度的。这些课程被定义为S-对象。



![fig01](http://7xruee.com1.z0.glb.clouddn.com/fig01.png)


我们应考虑将动态主题的每个时间段作为次要目标，称为d-目标。
集合S包含所有s-对象和d-对象，集合P由所有的可用的时间段组成。现在，一个工作日的可用时间周期要分配给S的每个对象;对于一个s-对象，时间段表示出课程开始的时间。任何满足所有约束分配(a)-(i)的分配能够产生一个可行的课程安排。
4.1可行解集合X
Eiselt和Laporte [7] 将需求划分为硬性、中性、软性。硬要求必须找到所有的解，而软要求是可取的，中性要求应该得到满足，但仍然比较宽松。
在大多数的方法，如果满足所有硬性要求，则时间表可行，当硬性要求表现得强制的时候，软性要求被模拟为目标。被用来设计解决时间表问题的解决方法通常包括两个阶段：在第一阶段启发式方法试图找到一种可行的时间表，并且在第二阶段当前解通过满足更多的中性或者软性要求得到改进并且不违反任何硬性要求。
让我们定义以下状态空间图G：
- 每一个可行的时间表是在G中的一个节点，
- 第二阶段如果y被认为是x的改进，画出节点x到节点Y的圆弧

第一阶段可以被看作是图G中的一个节点的扩展，而第二阶段是一个下降方法：通过在状态空间图G一步一步移动，直到达到局部最优。对于大型时间表的问题，找到一个可行的课程安排是不容易的事。此外，如果第一阶段成功地找到一个可行的解决方案，这个解和最好的不必须在一个图G中的相同连接中。
我们定义一个连接的状态空间图G，第二部分将会被禁忌搜索技术的适配替代。现在，我们要定义一个可行的解集合X，这是状态空间图G中的节点。
有两种方法用于处理约束：它可以通过满足可行解集合X的每一个解，或者它会被在目标函数中禁忌。这是用来决定哪个将会满足，哪个会被禁忌。
如果分配只能产生不可行的解，那么目标永远都不会被分配一个时间段。例如，每一个对象是一个主题x的一部分，因此不应该在截止期日之后安排或在开始时间x之前安排。出于这个原因，约束（E）应始终满意。
另一方面，两个对象之间的冲突应是一个充分条件 用于阻止分配。例如约束（a）应该阻碍，而且不总是满足。
通过这些思想的指导下，就决定惩罚的限制(a)-(d)并总满足约束条件(e)-(h)。
约束(i)已被分成两个不同的要求：（i1）（i2）要求连续时间周期动态主题的每天量的最小（最大）值不能违反。
这由约束i1决定的。这种选择是由分配合理的恰好一个目的是在禁忌搜索过程的每一次迭代而改变：为动态的主题，这意味着一个时间段只能有一个移动，并通过约束i1从每天量与最小值> 1的工作日中去除。约束（I2）被包括在总是满足需求的集合中，因为总是可以在不违反这一要求的情况下得到任何可行的调度。

对于动态主题，同一天中目标d的集合被定义为一门课。考虑一个动态主题满足约束的过程（G）。以下改变可以用的几个步骤而获得禁忌搜索过程，同时始终满足约束条件（g）：
- 连续时间周期的量没有改变，但课程被移动到其他工作日，
- 每日量增加或减少。
这证明我们的决定从来没有违反约束（g）。
约束（H）必须更仔细地考虑。如果静态主题只能在不同工作日数n被安排，如果其每日量数等于n，那么约束（H）应该被禁止，因为日常的量程不必正确排序，并且满足约束d只能通过违反要求h。但是这种情况在现实生活的时间表问题并不常见。因此这样的情况被认为是不会发生的。此外，动态主题的课程的定义意味着约束（H）满足任何动态的主题。由于这些原因，决定禁止任何违反约束（h）的解。
综上所述，切实可行的解决方案的集合X包含的任何时间分配时期S的对象，满足约束（e）、（F）、（G）、（h）和（I2）。
4.2目标函数
每个主题T，我们表示：
N（t）为日常量，
Qi(T）：（i =1，...，N（t））课程之一，
R（T）：时间周期中每日量最小数，
B（T）：当然补气的第一时间段qi(t)
EI（T）：课程qi（t）的最后一个时间段。
B（t）:T的第一时间段（B（T）=min{B(t)|i=1,...，N（t）}），
E（T）：T的最后时间段（E（T）= max(t）|i = 1，...，N（T）））
P（t）:集合T的直接前辈（如果t1和 T2优于T、T1优于t2时，则T2只属于P(t)）。
如果T是一个静态的主题，那么n（t）是固定的，对于任意的j>I,qi(T)都优于qj(t)。对于动态主题I，N（t）,值ei（t）到bi（t）都是可变的。
S的两个对象i和j，值d（I，J）表示ij之间的时间段的数目。因为一个s-对象的长度可大于一，分配到不同的时间段的两个对象可能会出现冲突。
涉及S对象i的班级被表示为c（i），t（i）是老师这给出课程。用ntopic表示主题的数目。
如果X中的任何解S，以下目标函数f定义为：



![form](http://7xruee.com1.z0.glb.clouddn.com/form01.png)


参数p1…..P5给出了约束(a), (b), (c), (d), (i1)相对权重。当且仅当f(s)=O= f *，解S可以归纳出可行的课程表。

4.3邻域N（S）
如果X集合中的解S’只是s只是通过改变主题t的一个对象就被认为是X集合中s的一个邻域解，设d是o被分配的当前工作日。为了满足约束（g）（h）中，在可能的移动的集合必须被限制为图2中展示的10个类型：
- 如果o为s-对象，那么他可以被安排到一个新的工作日d’≠d，且不包含任何类型1课程。
- 如果t是一个动态的主题且o为所述第一或课程的最后一段时间，然后它可以被分配给任何一个工作日D'。如果D'≠D和D'已经包含t中的课程Q，那么O可以被确定的放到第一个时间之前（类型3，4）或确定的放到时间段之后（5,6型）。如果D'≠ D并且D'不包含任何课程I，可那么o可以被分配至d的任意时间段（类型7，8）。如果D'= D，那么课程的第一个时间段可以被移动到最后一个时间段之后（类型9），并且最后一个时间段可以移动到第一个时间段之前（类型10）。当然，当且仅当s⊕m = S’，那么这些修改m都是可接受的。


![fig02](http://7xruee.com1.z0.glb.clouddn.com/fig02.png)

### 其它成分
当最近被分配到工作日d的目标o被移动到一个新的时间段，那么（o,d）被加入禁忌列表T，这意味着禁忌搜索的步长是|T|，不允许分配从d到o的任何的时间段。
集合V *是随机产生的，但仅包含只改变起始时间的为f(s)做出正贡献对象的分配。换句话说，满足所有约束的对象是不移动的。
为了禁忌状态取消，我们定义惩罚函数a(s, m) = f (s⊕m)和启发函数A(s,m)= f (s*).

