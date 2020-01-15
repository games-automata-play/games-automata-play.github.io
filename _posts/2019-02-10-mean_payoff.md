---
layout:     post
title:      Mean payoff games and universal graphs 
date:       2019-02-10 9:00:00
author:     Nathana&euml;l Fijalkow
category:   
- Parity games
- research
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      A: "{\\mathcal{A}}",
      Parity: "{\\text{Parity}}",
      G: "{\\mathcal{G}}",
      WE: "{\\mathcal{W}_{\\text{Eve}}}",
      U: "{\\mathcal{U}}",
      enc: "{\\text{enc}}",
      deltasucc: "{\\delta_{\\text{succ}}}",
      last: "{\\text{last}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post discusses using separating automata for constructing algorithms for solving mean payoff games.</p>

The goal of this post is to present the main results of [this paper](https://arxiv.org/abs/1812.07072),
meaning upper and lower bounds on separating automata for mean payoff games.
This is a joint work with Pawe&#322; Gawrychowski and Pierre Ohlmann.

We fix two parameters: $n$, the number of vertices in a graph, and $W \subseteq \mathbb{Z}$, a set of integer weights.

We consider the mean payoff condition (parameterised by $W$), given by

$$
\text{MeanPayoff}_W = \left\{ \pi : \liminf_i \frac{1}{i+1} \sum_{j = 0}^i \pi_j \ge 0 \right\} \subseteq W^\omega
$$

We instantiate the (equivalent) notions of **separating automata** and **universal graphs** for the mean payoff condition,
see [the previous post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}).

The notion of separating automata yields a natural family of algorithms for solving mean payoff games.
Given the success story of solving parity games using separating automata, it is natural to ask whether one can use separating automata for constructing efficient algorithms
for mean payoff games.
Since it is very hard to reason on separating automata directly, we use the equivalence with universal graphs.
We state our results using universal graphs, but thanks to the equivalence stated in [the previous post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}),
the same results hold for separating automata.

The answer is **yes**, but also **no**.
Yes, it gives faster algorithms. But no, it does not yield quasipolynomial time algorithms.

We study the question for two parameters of $W$: the maximal weight $B$ in absolute value, and the number $k$ of weights.
More precisely, we consider $$(n,W)$$-universal graphs. In the first case, we focus on $$(n,(-B,B))$$-universal graphs, and in the second case, on $$(n,W))$$-universal graphs
where $W$ has size $k$.

For the sake of comparisons, the best algorithm to date is due to Brim, Chaloupka, Doyen, Gentilini, and Raskin, see [this paper](https://link.springer.com/article/10.1007/s10703-010-0105-x).
Its complexity is 

$$
O(n m B)
$$

Note that it is not polynomial: since weights are usually represented in binary, the number $B$ is exponential.
More precisely, we say that this algorithm is pseudopolynomial, because it is polynomial if weights are represented in unary.

## Parameter: maximal weight in absolute value

> **Theorem:**
* There exists a $$(n,(-B,B))$$-universal mean payoff graph of size $$2n (nB)^{1 - 1/n}$$
* All $$(n,(-B,B))$$-universal mean payoff graphs have size at least $B^{1 - 1/n}$

Since $n$ is polynomial in the size of the input, we can say that $n$ is small, 
while $B$ is exponential in the size of the input when weights are given in binary, hence large.
The multiplicative gap between upper and lower bound is bounded by $2n^2$, hence small.

As a consequence of the upper bound 
we obtain an algorithm for solving mean payoff games improving over the best algorithm to date:
the dependence in $B$ goes from $B$ to $B^{1 - 1/n}$, yielding the first algorithm running in sublinear time in $N$.
The lower bound shows that this dependence cannot be improved for algorithms using separating automata.

> **Corollary:**	
There exists an algorithm for solving mean payoff games of complexity 
$$O(n m (nB)^{1 - 1/n})$$

## Parameter: number of weights

> **Theorem:**
* For all $W$ of size $k$, there exists a $$(n,W)$$-universal mean payoff graph of size $$n^k$$
* For all $k$, there exists $W$ of size $k$ such that all $$(n,W)$$-universal mean payoff graphs have size at least $n^{k-2}$

As a consequence, we obtain an algorithm solving mean payoff games of complexity $O(m n^k)$.
The lower bound shows that algorithms using separating automata cannot break the $O(n^{\Omega(k)})$ barrier,
and in particular do not have quasipolynomial complexity.

> **Corollary:**	
There exists an algorithm for solving mean payoff games of complexity 
$$O(m n^k)$$

