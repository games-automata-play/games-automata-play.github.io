---
layout:     post
title:      Value iteration for parity games
date:       2018-08-10 9:00:00
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

<p class="intro"><span class="dropcap">T</span>his post presents a generic value iteration algorithm for parity games
parametrised by universal trees. As special cases this extends the small progress measure of Jurdzi&#324;ski 
and the succinct progress measure of Jurdzi&#324;ski and Lazi&#263;.
The complexity of the algorithm is bounded by the size of the universal tree.</p>

We use the notion of universal trees introduced in [this post]({{ '/blog/universal_trees' | prepend: site.baseurl }}).
We refer to [this paper](https://arxiv.org/abs/1801.09618) for full details.

#### Tree labeling as winning witnesses

Consider a parity game with $n$ vertices and using priorities in $[1,d]$ (without loss of generality $d$ is even).
Let $h = d/2$.
As in [this post]({{ '/blog/universal_trees' | prepend: site.baseurl }}), 
we look at totally ordered trees with unbounded degree, meaning that each node may have arbitrarily many children and the children of a node are totally ordered.
We index the levels by odd priorities from bottom to top (see picture below).

We consider labelings of trees by vertices. The labels are on the leaves of the tree, and a leaf may be labeled with several vertices.
Hence a labeling is a function $\mu : V \to L(T) \cup \\{\bot\\}$, where $L(T)$ is the set of leaves of $T$.

A labelling $\mu$ induces a set of orders on vertices: 
* for $p$ odd and $v,v'$ two vertices, we have $v \vartriangleleft_p v'$ if 
the ancestor at level $p$ of $v$ is **strictly** to the left of the ancestor at the same level of $v'$.
* for $p$ even and $v,v'$ two vertices, we have $v \vartriangleleft_p v'$ if 
the ancestor at level $p$ of $v$ is **to the left of or equal to** the ancestor at the same level of $v'$.

The element $\bot$ is the smallest element for all orders.
A picture is worth a thousand words:

<figure>
	<img src="{{ '/images/example_tree.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Example of relations: $v_3 >_1 v_7 >_1 v_1$, $v_5 \ge_2 v_3 \ge_2 v_6$, and $v_4 >_3 v_2$.</figcaption>
</figure>

Note that $\vartriangleleft_p$ is a total order, which is strict for $p$ odd and reflexive for $p$ even.

> **Theorem:** 
For all $(n,d/2)$-universal tree $T$, there exists a labeling $\mu : V \to L(T) \cup \\{\bot\\}$ such that
* $\mu(v) \neq \bot$ if and only if Eve wins from $v$
* if $v \in V_E$ has priority $p$, then there exists $(v,v') \in E$ such that $v \vartriangleleft_p v'$
* if $v \in V_A$ has priority $p$, then for all $(v,v') \in E$ we have $v \vartriangleleft_p v'$

<!--
**Proof:**
This is a rephrasing of Jurdzi&#324;ski's original proof for the small progress measure algorithm.

Indeed, the algorithm yields a labeling $\mu : V \to [1,n]^{d/2}$ satisfying the properties above.
We see $[1,n]^{d/2}$ as a leaf in the naive universal tree above, given by its path from the root.
 
Removing all leaves that are not labeled by some vertex, we obtain a tree with at most $n$ leaves and height $d/2$.
By universality it embeds into $T$, implying that it also satisfies the properties above.

The fact that such a tree witnesses winning relies on the observation that the last two properties rule out odd cycles in the underlying positional strategy.
-->

#### Generic value iteration algorithm

We fix a $(n,h)$-universal tree $T$.
The value iteration algorithm constructs a labeling $\mu : V \to L(T) \cup \\{\bot\\}$ as in the theorem above.
The first labeling assigns all vertices to the maximal (i.e., leftmost) leaf in $T$.
At each step, the value iteration algorithm picks a vertex $v$, and checks whether the local conditions defined in conditions 2. and 3. are satisfied by $\mu$ for $v$.
If they are not, the value of $\mu(v)$ is decreased by the minimal possible amount to satisfy them, giving rise to $\lift_v(\mu)$.
If this is not possible, $\mu(v)$ is assigned $\bot$ (losing).

<figure>
	<img src="{{ '/images/example_lifting.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Example of lifting $v_5$: it is pushed to the right in order to satisfy $v_5 \vartriangleleft_3 v_7$ and $v_5 \vartriangleleft_2 v_1$.</figcaption>
</figure>

The correctness follows from the two observations: let $\mu_*$ be a labeling as in the theorem above, for the order $\vartriangleleft_1$:
* the initial labeling is pointwise larger than $\mu_*$,
* if $\mu$ is pointwise larger than $\mu_*$, then so is $\lift_v(\mu)$.

#### Complexity

The algorithm given above lifts a vertex $v$ at most $\|T\|$ many times, which is the number of leaves of $T$.
Hence the total number of lifts is $O(n |T|)$, hence so is (roughly) the complexity of the algorithm, provided lifts can be performed in reasonable time 
(which is the case when the universal tree is reasonably described).

> **Theorem:** (Jurdzi&#324;ski and Lazi&#263;)
There exists a $(n,h)$-universal tree with $n^{\log(h)}$ leaves

This yields the succinct progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;.

> **Corollary:**
There exists a quasipolynomial time algorithm solving parity games
