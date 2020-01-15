---
layout:     post
title:      Value iteration for parity games
date:       2018-08-10 9:00:00
author:     Nathana&euml;l Fijalkow
category:   
- Parity games
- Research
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      lift: "{\\text{lift}}",
      G: "{\\mathcal{G}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post presents a generic value iteration algorithm for parity games
parametrised by universal trees. As special cases this extends the small progress measure of Jurdzi&#324;ski 
and the succinct progress measure of Jurdzi&#324;ski and Lazi&#263;.
The complexity of the algorithm is bounded by the size of the universal tree.</p>

We use the notion of universal trees introduced in [this post]({{ '/blog/universal_trees' | prepend: site.baseurl }})
and the fundamental theorem proved in [this post]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}).

We refer to [this paper](https://arxiv.org/abs/1801.09618) for full details.

### Progress measures and universal tree

We fix two parameters $n$ and $d$, and $T$ a universal tree.

As in [this post]({{ '/blog/universal_trees' | prepend: site.baseurl }}), 
we look at totally ordered trees with unbounded degree, meaning that each node may have arbitrarily many children and the children of a node are totally ordered.
We index the levels by odd priorities from bottom to top (see picture below).

<figure>
	<img src="{{ '/images/example_tree.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Example of relations: $u \vartriangleleft_0 v \vartriangleleft_0 w$, $v \vartriangleleft_2 w \vartriangleleft_2 v$, and $u \vartriangleleft_3 v$.</figcaption>
</figure>

Let $T$ be a tree. We use $T$ for the set of leaves of the tree $T$.
We define the relations $\vartriangleleft_p$ for each $p \in [0,d-1]$
* for $p$ even and $\ell,\ell'$ two leaves, we have $\ell \vartriangleleft_p \ell'$ if 
the ancestor at level $p$ of $\ell$ is **to the left of or equal to** the (node) ancestor at the same level of $\ell'$
* for $p$ odd and $\ell,\ell'$ two vertices, we have $\ell \vartriangleleft_p \ell'$ if 
$\ell \vartriangleleft_{p-1} \ell'$ and not $\ell' \vartriangleleft_{p-1} \ell$

Equivalently, for $p$ odd we have $\ell \vartriangleleft_p \ell'$ if and only if
the (edge) ancestor at level $p$ of $v$ is **strictly to the left of** the (edge) ancestor at the same level of $v'$

> **Definition:**
Consider a graph $G$ with $n$ vertices and priorities in $[0,d-1]$.
A progress measure for $G$ is a function $\mu : V \to T$ such that
for $(v,p,v') \in E$, we have $\mu(v) \vartriangleleft_p \mu(v')$

We can reformulate the [fundamental theorem]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}) as follows:
a graph $G$ satisfies parity if and only if there exists a progress measure for $G$.
Indeed, a progress measure is exactly a homomorphism from $G$ to $G(T)$.

We extend this to games.
We add a bottom element $\bot$.

> **Definition:**
Consider a parity game $\G$ with $n$ vertices and priorities in $[0,d-1]$.
A progress measure $\G$ is a function $\mu : V \to T \cup \\{\bot\\}$ such that
* if $v \in V_E$, then there exists $(v,p,v') \in E$ such that $\mu(v) \vartriangleleft_p \mu(v')$
* if $v \in V_A$, then for all $(v,p,v') \in E$ we have $\mu(v) \vartriangleleft_p \mu(v')$

We can now state the main theorem underlying the value iteration algorithm.

> **Theorem:** 
For all parity games with $n$ vertices and priorities in $[0,d-1]$, 
there exists a progress measure such that $\mu(v) \neq \bot$ if and only if Eve has a winning strategy from $v$

**Proof:**
This is an easy corollary of the [fundamental theorem]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}) combined with positional determinacy.

First, observe that for any progress measure $\mu$, we can define a positional strategy $\sigma$ by defining $\sigma(v) = (v,p,v')$ such that $\mu(v) \vartriangleleft_p \mu(v')$.
Now reducing to the vertices such that $\mu(v) \neq \bot$ and the moves prescribed by $\sigma$ we obtain a graph together with a progress measure for this graph.
It follows that $\sigma$ is a winning strategy from all vertices such that $\mu(v) \neq \bot$. Note that this observation holds for any progress measure $\mu$.

Conversely, assume that Eve has a winning strategy from $v$, and let $\sigma$ be a positional winning strategy. Consider the graph $\G[\sigma]$, it satisfies parity,
hence by the fundamental theorem there exists a progress measure. Combined with the strategy $\sigma$ this yields a progress measure for the game $\G$.

### The algorithm

The value iteration algorithm constructs the **largest** progress measure.
It manipulates functions $\mu : V \to T \cup \\{\bot\\}$.
The initial function assigns all vertices to the maximal (i.e., rightmost) leaf in $T$.
At each step, the value iteration algorithm picks a vertex $v$, and checks whether the local conditions are satisfied by $\mu$ for $v$.
If they are not, the value of $\mu(v)$ is decreased by the minimal possible amount to satisfy them, giving rise to $\lift_v(\mu)$.
If this is not possible, $\mu(v)$ is assigned $\bot$ (losing).

<figure>
	<img src="{{ '/images/example_lifting.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Example of lifting $v_5$: it is pushed to the left in order to satisfy $v_5 \vartriangleleft_3 v_7$ and $v_5 \vartriangleleft_2 v_1$.</figcaption>
</figure>

The correctness follows from the two observations: let $\mu_*$ be a labeling as in the theorem above, for the order $\vartriangleleft_0$:
* the initial labeling is pointwise larger than $\mu_*$,
* if $\mu$ is pointwise larger than $\mu_*$, then so is $\lift_v(\mu)$.

### Complexity

The algorithm given above lifts a vertex $v$ at most $\|T\|$ many times, which is the number of leaves of $T$.
Hence the total number of lifts is $O(n |T|)$, hence so is (roughly) the complexity of the algorithm, provided lifts can be performed in reasonable time 
(which is the case when the universal tree is reasonably described).

> **Theorem:** (Jurdzi&#324;ski and Lazi&#263;)
There exists a $(n,h)$-universal tree with $n^{\log(h)}$ leaves

This yields the succinct progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;.

> **Corollary:**
There exists a quasipolynomial time algorithm solving parity games

