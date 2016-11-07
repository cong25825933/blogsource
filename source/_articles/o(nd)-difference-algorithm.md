---
title: (译) O(ND)字符串差异算法
date: 2016-11-07 09:52:32
categories:
tags: [algorithm]
cover:
---

作者： 亚利桑那大学计算机科学系，EUGENE W.MYERS

众所周知，寻找最长公共子串问题(LCS)与最短编辑脚本问题(SES)是对偶问题。在这篇论文中，他们将被认为在编辑图中寻找最短/最长路径是等价问题。在这个视角看该问题，O(ND)时间空间复杂度的算法就出现了，N表示A串、B串的长度之和，D表示A、B的最小编辑脚本。该算法在A、B串差异不大的时候效率很好(D很小)。该算法在一般随机的情况下，有O(N+D²)的预计时间效率和O(N)的空间复杂度，如果使用前缀树的话，时间复杂度可以优化至O(NlgN+D²).

关键词：  
最长公共子串；最短编辑脚本(LCS)，编辑图(SES)，文件比较

## 引言

两个字符串差异问题已经在其他的文献中有过学习[1,8,11,13,16,19,20]。应用该算法的应用有许多，包括拼写校对系统，文件比对工具，基因演化的研究[4,5,17,18]。进入正题，寻找最长公共子串的问题等价于最短编辑脚本(A串通过若干插入和删除操作变为B串的操作数)。其中最早的算法是来自Wagner和Fischer的"串串"校对问题，需要O(N²)的时空复杂度。之后是来自Hirschberg的优化，只需要O(N)线性空间复杂度[7]。当差异算法是基于相等不相等的比较操作，则被认为Ω(N²)是必须的[1]。随后有四位俄罗斯人做出了略微地优化，将时间复杂度降至O(N²lglgN/lgN)和O(N²/lgN)，分别对应随机字母情况和有限字母情况。当然还存在更快的基于其他比较操作算法，事实上，使用"<=" "=" ">="比较操作的算法，Ω(NlgN)时间复杂度是最佳的。

最近有研究提升了随机字母情况的O(N²)算法，使得该问题效率对另一个参数敏感。假设输出参数L为最长公共子序列长度，那么对应的对偶参数D = 2*(N-L)，表示最短编辑脚本长度。(在引言中，假定A、B串长都为N) 有两个来自Hirschberg输出敏感算法，分别需要O(NL+NlgN) 和 O(DLlgN) 时间复杂度。还有一种来自Hunt和Szymanski的O((R+N)lgN)算法，R表示AB串相等的位置个数。

在实际情况下，通常参数D比较小。比如程序员想要知道他们曾经对于文本文件的修改。生物学家想知道DNA的突变情况。对于这些情况，O(ND)算法更优于Hirschberg的算法，因为当D比较小的时候，L需要O(N)时间才能求出。

在这篇论文中将给出一个O(ND)时间复杂度的算法。我们的算法简单，并且基于便于理解的编辑图。它利用“贪心”的设计范例，表现了最长公共子序列到单源最短路径的关系。一个其他的O(ND)算法在其他地方出现过。
