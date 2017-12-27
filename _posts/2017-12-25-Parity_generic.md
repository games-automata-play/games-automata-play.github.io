---
layout:     post
title:      Value iteration for parity games
date:       2017-12-25 9:00:00
author:     Nathana&euml;l Fijalkow
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
parametrised by "universal trees". As special cases this extends the small progress measure of Jurdzi&#324;ski 
and the succinct progress measure of Jurdzi&#324;ski and Lazi&#263;.
The complexity of the algorithm is the size of the universal tree, motivating constructing small universal trees.</p>

#### Universal trees

We fix two parameters: $n$, which will be the number of leaves, and $h$, which will be the height.

The trees we consider have unbounded degree and the branching is totally ordered, meaning that each node may have arbitrarily many children and the children of a node are totally ordered.
A tree is just the graphical representation of a lexicographic order on the leaves. For a tree $T$, $x,x'$ two leaves of $T$ and $p \in [1,h]$, 
we write $x \ge_p x'$ for the lexicographic order up to depth $p$, and $x >_p x'$ for the strict version.

We say that a tree embeds into another if the first one can be obtained by removing nodes from the second.
We say that a tree $T$ is $(n,h)$-universal if all trees with at most $n$ leaves each of depth exactly $h$ embed into $T$.

An example of a $(n,h$)-universal tree is the complete tree of height $h$ with each node of degree $n$. It has $n^h$ leaves.

<figure>
	<img src="{{ '/images/tree.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The naive $(5,2)$-universal tree has 25 branches.</figcaption>
</figure>

#### Tree labeling as winning witnesses

Consider a parity game $G$ with $n$ vertices and using priorities in $[1,d]$ (without loss of generality $d$ is even).
Let $h = d/2$.

We consider labelings of trees by vertices. The labels are on the leaves of the tree, and a leaf may be labeled with several vertices.
Hence a labeling is a partial function $\mu : V \to L(T)$, where $L(T)$ is the set of leaves of $T$.

> **Theorem:** 
Let $T$ be a $(n,d/2)$-universal tree, there exists a labeling $\mu : V \to L(T)$ such that: 
* $\mu(v)$ is defined if, and only if, Eve wins from $v$,
* if $v \in V_E$ has priority $p$, then there exists $(v,v') \in E$ such that $\mu(v) \ge_p \mu(v')$, strict if $p$ odd,
* if $v \in V_A$ has priority $p$, then for all $(v,v') \in E$ we have $\mu(v) \ge_p \mu(v')$, strict if $p$ odd.

**Proof:**
This is a rephrasing of Jurdzi&#324;ski's original proof for the small progress measure algorithm.
The existence is proved by analysing Zielonka's algorithm, which will be described in a later post.
Indeed, the algorithm yields a labeling $\mu : V \to [1,n]^{d/2}$ satisfying the properties above. 
This induces a tree with at most $n$ leaves and height $d/2$, hence it embeds into $T$, implying that it also satisfies the properties above.
The fact that such a tree witnesses winning relies on the observation that the last two properties rule out odd cycles in the underlying positional strategy.


**Remark:**
Miko&#322;aj Boja&#324;czyk observed that in this theorem, it is not necessary to talk about linear preorders, i.e. trees. They could be replaced by general preorders.
I do not know yet how this will help, but it *sounds* like a smart observation.

#### Generic value iteration algorithm

We fix a $(n,h)$-universal tree $T$.
The value iteration algorithm constructs a labeling $\mu : V \to L(T)$ as in the theorem above.
The first labeling assigns all vertices to the maximal (i.e., leftmost) leaf in $T$.
At each step, the value iteration algorithm picks a vertex $v$, and checks whether the local conditions defined in conditions 2. and 3. are satisfied by $\mu$ for $v$.
If they are not, the value of $\mu(v)$ is decreased by the minimal possible amount to satisfy them, giving rise to $\lift_v(\mu)$.
If this is not possible, $\mu(v)$ is left undefined (losing).

The correctness follows from the two observations: let $\mu_*$ be a labeling as in the theorem above,
* the initial labeling is pointwise larger than $\mu_*$,
* if $\mu$ is pointwise larger than $\mu_*$, then so is $\lift_v(\mu)$.

#### Complexity

The algorithm given above lifts a vertex $v$ at most $\|T\|$ many times, which is the number of leaves of $T$.
Hence the total number of lifts is $O(n |T|)$, hence so is (roughly) the complexity of the algorithm, provided lifts can be performed in reasonable time.

> **Theorem:** (Jurdzi&#324;ski) 
There exists a $(n,h)$-universal tree with $n^h$ leaves.

> **Theorem:** (Jurdzi&#324;ski and Lazi&#263;)
There exists a $(n,h)$-universal tree with $n^{\log(h)}$ leaves.

<figure>
	<img src="{{ '/images/tree_succinct.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The succinct $(5,2)$-universal tree of Jurdzi&#324;ski and Lazi&#263; has 17 branches.</figcaption>
</figure>

**Question:** What is the size of the smallest $(n,h)$-universal tree? If it is polynomial in $n$ and $h$ and such a tree is constructible in polynomial time, 
then this gives rise to a polynomial time algorithm for parity games.

Below we show the smallest $(5,2)$-universal tree. It has 11 leaves, which is less than the naive one (25 branches) and the succinct one (17 branches).

<figure>
	<img src="{{ '/images/tree_optimal.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The smallest $(5,2)$-universal tree has 11 leaves.</figcaption>
</figure>

