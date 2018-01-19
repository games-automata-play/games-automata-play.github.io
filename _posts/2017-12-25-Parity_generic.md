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
parametrised by universal trees. As special cases this extends the small progress measure of Jurdzi&#324;ski 
and the succinct progress measure of Jurdzi&#324;ski and Lazi&#263;.
The complexity of the algorithm is the size of the universal tree, motivating constructing small universal trees.</p>

#### Universal trees

<p>
We fix two parameters: $n$, which will be the number of leaves, and $h$, which will be the height.
All trees we consider have height $h$, and more precisely all the leaves have depth exactly $h$.
</p>

<p>
We look at totally ordered trees with unbounded degree, meaning that each node may have arbitrarily many children and the children of a node are totally ordered.
</p>

<p>
We say that a tree embeds into another if the first one can be obtained by removing nodes from the second.
Note that since all trees we consider have height $h$, the root of the first tree is mapped to the root of the second tree.
We say that a tree $T$ is $(n,h)$-universal if all trees with at most $n$ leaves each of depth exactly $h$ embed into $T$.
</p>

<p>
An example of a $(n,h$)-universal tree is the complete tree of height $h$ with each node of degree $n$. It has $n^h$ leaves.
</p>

<figure>
	<img src="{{ '/images/tree.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The naive $(5,2)$-universal tree has 25 branches.</figcaption>
</figure>

<figure>
	<img src="{{ '/images/embedding_example.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>A tree with $5$ leaves and height $2$, and one possible embedding into the universal tree above.</figcaption>
</figure>

#### Tree labeling as winning witnesses

Consider a parity game with $n$ vertices and using priorities in $[1,d]$ (without loss of generality $d$ is even).
Let $h = d/2$.

We consider labelings of trees by vertices. The labels are on the leaves of the tree, and a leaf may be labeled with several vertices.
Hence a labeling is a function $\mu : V \to L(T) \cup \\{\bot\\}$, where $L(T)$ is the set of leaves of $T$.

It induces a set of orders on vertices: for $p \in [1,d]$ and $v,v'$ two vertices, we have $v \ge_p v'$ if
the ancestor at level $p/2$ of $v$ is to the left of the ancestor at the same level of $v'$ (levels are counted from the bottom up).
The element $\bot$ is the smallest element for all orders $\ge_p$.
The strict version is $>\_p$. 
A picture is worth a thousand words:

<figure>
	<img src="{{ '/images/example_tree.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>$u >_1 v >_1 w$ and $u =_3 v >_3 w$.</figcaption>
</figure>

Note that $\ge_{2p}$ and $\ge_{2p+1}$ coincide.

> **Theorem:** 
For all $(n,d/2)$-universal tree $T$, there exists a labeling $\mu : V \to L(T) \cup \\{\bot\\}$ such that: 
* $\mu(v) \neq \bot$ if, and only if, Eve wins from $v$,
* if $v \in V_E$ has priority $p$, then there exists $(v,v') \in E$ such that $v \ge_p v'$, strict if $p$ odd,
* if $v \in V_A$ has priority $p$, then for all $(v,v') \in E$ we have $v \ge_p v'$, strict if $p$ odd.

**Proof:**
This is a rephrasing of Jurdzi&#324;ski's original proof for the small progress measure algorithm.
The existence is proved by analysing Zielonka's algorithm, which is explained in [this later post]({{ '/blog/Zielonka' | prepend: site.baseurl }}).

Indeed, the algorithm yields a labeling $\mu : V \to [1,n]^{d/2}$ satisfying the properties above.
We see $[1,n]^{d/2}$ as a leaf in the naive universal tree above, given by its path from the root.
 
Removing all leaves that are not labeled by some vertex, we obtain a tree with at most $n$ leaves and height $d/2$.
By universality it embeds into $T$, implying that it also satisfies the properties above.

The fact that such a tree witnesses winning relies on the observation that the last two properties rule out odd cycles in the underlying positional strategy.


**Remark:**
Miko&#322;aj Boja&#324;czyk observed that in this theorem, it is not necessary to talk about linear preorders, i.e. trees. They could be replaced by general preorders.
I do not know yet how this will help, but it *sounds* like a smart observation.

#### Generic value iteration algorithm

We fix a $(n,h)$-universal tree $T$.
The value iteration algorithm constructs a labeling $\mu : V \to L(T) \cup \\{\bot\\}$ as in the theorem above.
The first labeling assigns all vertices to the maximal (i.e., leftmost) leaf in $T$.
At each step, the value iteration algorithm picks a vertex $v$, and checks whether the local conditions defined in conditions 2. and 3. are satisfied by $\mu$ for $v$.
If they are not, the value of $\mu(v)$ is decreased by the minimal possible amount to satisfy them, giving rise to $\lift_v(\mu)$.
If this is not possible, $\mu(v)$ is assigned $\bot$ (losing).

The correctness follows from the two observations: let $\mu_*$ be a labeling as in the theorem above, for the order $\ge_d$:
* the initial labeling is pointwise larger than $\mu_*$,
* if $\mu$ is pointwise larger than $\mu_*$, then so is $\lift_v(\mu)$.

#### Complexity

The algorithm given above lifts a vertex $v$ at most $\|T\|$ many times, which is the number of leaves of $T$.
Hence the total number of lifts is $O(n |T|)$, hence so is (roughly) the complexity of the algorithm, provided lifts can be performed in reasonable time.

> **Theorem:** (Jurdzi&#324;ski) 
There exists a $(n,h)$-universal tree with $n^h$ leaves.

> **Theorem:** (Jurdzi&#324;ski and Lazi&#263;)
There exists a $(n,h)$-universal tree with $n^{\log(h)}$ leaves.

**Question:** What is the size of the smallest $(n,h)$-universal tree? If it is polynomial in $n$ and $h$ and such a tree is constructible in polynomial time, 
then this gives rise to a polynomial time algorithm for parity games.

Below we show the smallest $(5,2)$-universal tree. It has 11 leaves, which is less than the naive one (25 branches).

<figure>
	<img src="{{ '/images/tree_optimal.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The smallest $(5,2)$-universal tree has 11 leaves.</figcaption>
</figure>

**Upper bound:** 
The construction we present here is a streamlined version of Jurdzi&#324;ski's and Lazi&#263;'s.
It is very marginally smaller.

We construct an $(n,h)$-universal tree with $f(n,h)$ leaves by induction, 
where

$$f(n,h) = f(n,h-1) + f(\lfloor n/2 \rfloor,h) + f(n - 1 - \lfloor n/2 \rfloor,h)$$

$$f(n,1) = n ;\qquad f(h,1) = 1$$

<figure>
	<img src="{{ '/images/smallest_tree_construction.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The inductive construction.</figcaption>
</figure>

To construct the $(n,h)$-universal tree $T$, let:
* $T_\text{middle}$ a $(n,h-1)$-universal tree;
* $T_\text{left}$ a $(\lfloor n/2 \rfloor,h)$-universal tree;
* $T_\text{right}$ a $(n - 1 - \lfloor n/2 \rfloor,h)$-universal tree.

Now construct $T$ as in the drawing above.

We argue that it is $(n,h)$-universal.
Consider a $(n,h)$-tree $t$. 
The question is where to cut in the middle. 
Let $v_1,\ldots,v_m$ be the children of the root of $t$, and let $n(v_i)$ be the number of leaves below $v_i$. 
Choose the unique $v_p$ such that 

$$n(v_1) + \cdots + n(v_{p-1}) \le \lfloor n/2 \rfloor$$

and

$$n(v_{p+1}) + \cdots + n(v_m) \le n - 1 - \lfloor n/2 \rfloor.$$

To embed $t$ into $T$, map $v_p$ to the root of $T_\text{middle}$, and then proceed by induction.


**Lower bound:** We can show that any $(n,h)$-universal tree has at least $g(n,h)$ leaves by induction, where

$$g(n,h) = \sum_{d = 1}^n g(\lfloor n / d \rfloor,h-1)$$

$$g(n,1) = n ;\qquad g(h,1) = 1$$

This will be posted here someday...
