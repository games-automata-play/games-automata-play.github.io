---
layout:     post
title:      Universal trees
date:       2018-08-03 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Parity games
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      lift: "{\\text{lift}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post introduces the notion of universal trees and poses an open problem:
what is the exact size of the smallest universal tree?
</p>

<p>
The motivation for studying universal trees comes from parity games. This post does not explain this connection at all, the goal is to present an open problem of purely combinatorial nature
as I hope to appeal to my combinatorially-minded friends.
</p>

For the parity games aficionados, a short chronology:
* Jurdzi&#324;ski and Lazi&#263; implicitly constructed a quasipolynomial universal tree in the [succinct progress measure algorithm](https://arxiv.org/abs/1702.05051)
* I defined universal trees to show that it provides a [generic value iteration algorithm for parity games](https://arxiv.org/abs/1801.09618),
and gave upper and lower bounds on the size of universal trees 
* Czerwi&#324;ski, Daviaud, Jurdzi&#324;ski, Lazi&#263;, Parys, and myself later showed that a wide class of [automata-theoretic algorithms for solving parity games need to construct universal trees](https://arxiv.org/abs/1807.10546)

### Definition

<p>
We fix two parameters: $n$, which will be the number of leaves, and $h$, which will be the height.
All trees we consider have height $h$, and more precisely all the leaves have depth exactly $h$.
</p>

<p>
We look at totally ordered trees with unbounded degree, meaning that each node may have arbitrarily many children and the children of a node are totally ordered.
The size of a tree is its number of leaves.</p>

<figure>
	<img src="{{ '/images/tree.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>A tree of size $11$ and height $2$</figcaption>
</figure>

<p>
We say that a tree embeds into another if the first one can be obtained by removing nodes from the second.
We say that a tree $T$ is $(n,h)$-universal if all trees of size $n$ and height $h$ embed into $T$.
</p>

<figure>
	<img src="{{ '/images/embedding_example.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>A tree of size $5$ and height $2$, and one possible embedding into the tree above</figcaption>
</figure>

<p>
An example of a $(n,h$)-universal tree is the complete tree of height $h$ with each node of degree $n$. It has size $n^h$.
</p>

<figure>
	<img src="{{ '/images/tree_naive.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The naive $(5,2)$-universal tree has size $25$</figcaption>
</figure>

> **Objective:** Construct small universal trees

The details for the upper and lower bounds presented in this post can be found in [this technical report](https://arxiv.org/abs/1801.09618).

### Upper bounds

> **Theorem:**
There exists a $(n,h)$-universal tree of size $f(n,h)$, where
$$f(n,h) \le 2^{\lceil \log(n) \rceil} \binom{\lceil \log(n) \rceil + h - 1}{\lceil \log(n) \rceil}$$

A generous upper bound on the expression above is $n^{O(\log(h))}$.
A refined analysis reveals that the expression is polynomial in $n$ and $h$ if $h \le \log(n)$.

The construction of Jurdzi&#324;ski and Lazi&#263; yields such a tree. 
We present here a streamlined version, which is marginally better.

We construct an $(n,h)$-universal tree of size $f(n,h)$ by induction, 
where

$$f(n,h) = f(n,h-1) + f(\lfloor n/2 \rfloor,h) + f(n - 1 - \lfloor n/2 \rfloor,h)$$

$$f(n,1) = n ;\qquad f(h,1) = 1$$

<figure>
	<img src="{{ '/images/smallest_tree_construction.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The inductive construction</figcaption>
</figure>

To construct the $(n,h)$-universal tree $T$, let:
* $T_\text{middle}$ a $(n,h-1)$-universal tree;
* $T_\text{left}$ a $(\lfloor n/2 \rfloor,h)$-universal tree;
* $T_\text{right}$ a $(n - 1 - \lfloor n/2 \rfloor,h)$-universal tree.

Now construct $T$ as in the drawing above.

We argue that it is $(n,h)$-universal.
Consider a $(n,h)$-tree $t$. 
The question is where to cut in the middle. 
Let $v_1,\ldots,v_m$ be the children of the root of $t$, and let $n(v_i)$ be the size of the tree rooted in $v_i$. 
Choose the unique $v_p$ such that 

$$n(v_1) + \cdots + n(v_{p-1}) \le \lfloor n/2 \rfloor$$

and

$$n(v_{p+1}) + \cdots + n(v_m) \le n - 1 - \lfloor n/2 \rfloor.$$

To embed $t$ into $T$, map $v_p$ to the root of $T_\text{middle}$, and then proceed by induction.


### Lower bounds

> **Theorem:**
Any $(n,h)$-universal tree has size at least $g(n,h)$, where
$$ g(n,h) \ge \binom{\lfloor \log(n) \rfloor + h - 1}{\lfloor \log(n) \rfloor}$$

We show by induction that

$$g(n,h) = \sum_{d = 1}^n g(\lfloor n / d \rfloor,h-1)$$

$$g(n,1) = n ;\qquad g(h,1) = 1$$

Let $T$ be a $(n,h)$-universal tree, and $\delta \in [1,n]$. 
We claim that the number of nodes at depth $h-1$ 
of degree greater to or larger than $\delta$ is at least $g(\lfloor n / \delta \rfloor,h-1)$.

Let $T_\delta$ be the subtree of $T$ obtained by removing all leaves and all nodes at depth $h-1$
of degree less than $\delta$: the leaves of the tree $T_\delta$ have height exactly $h-1$.

We argue that $T_\delta$ is $(\lfloor n / \delta \rfloor,h-1)$-universal.
Indeed, let $t$ be a tree with $\lfloor n / \delta \rfloor$ leaves all at depth $h-1$.
To each leaf of $t$ we append $\delta$ children, yielding the tree $t_+$ which has $\lfloor n / \delta \rfloor \cdot \delta \le n$ leaves 
all at depth $h$.
Since $T$ is $(n,h)$-universal, the tree $t_+$ embeds into $T$.
Observe that the embedding induces an embedding of $t$ into $T_\delta$,
since the leaves of $t$ have degree $\delta$ in $t_+$, hence are also in $T_\delta$.

So far we proved that the number of nodes at depth $h-1$ 
of degree greater to or larger than $\delta$ is at least $g(\lfloor n / \delta \rfloor,h-1)$.
Now, note that the sum over $\delta \in [1,n]$ of the number of nodes at depth $h-1$ 
of degree greater to or larger than $\delta$ is a lower bound on the number of leaves,
which concludes.

### Between lower and upper bounds?

One can show that 

$$\frac{f(n)}{g(n)} = O(nh)$$

so there is a (reasonably small but still) gap.
Which is the right formula, the lower or the upper bound? Turns out it is neither of them!

Here is what I know:
* the lower bound is not tight. For $h = 2$ and $n = 5$, there are no universal trees of size $10$, easily seen by necessary conditions (there must be a node with $5$ leaves
and two other nodes with $2$ leaves).
* the upper bound is tight for $h = 2$ (there is a bit of work to check this)
* the upper bound is **not** tight for $h = 3$. Take $h = 3$ and $n = 10$. The upper bound construction uses as $T_\text{middle}$ a $(10,2)$-universal tree, which is actually wasteful. 
To see this, we do a case distinction, and find necessary conditions on the tree $T$ to use instead of $T_\text{middle}$.
Consider a tree of height $3$ with $10$ leaves we want to embed and follow the construction of the proof, using the induction we embed a subtree $t$ rooted in some child of the root into $T$.
Either the root of $t$ has degree at most $5$, and then it is enough to require that $T$ is $(5,2)$-universal,
or it has degree at least $6$. 
A case analysis reveals that the following tree $T$ satisfies the two conditions above, meaning it is $(5,2)$-universal and it embeds all trees with height $2$ and whose root has degree at least $6$.
Interestingly, it is smaller than $T_\text{middle}$.

<figure>
	<img src="{{ '/images/counter_example.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Replace $T_\text{middle}$ by this tree to get a smaller $(10,3)$-universal tree</figcaption>
</figure>

> **Open question:** what is the exact size of the smallest $(n,h)$-universal tree?


