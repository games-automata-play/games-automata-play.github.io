---
layout:     post
title:      PAC-learning and compression schemes 
date:       2019-03-26 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Learning theory
---

<p class="intro"><span class="dropcap">W</span>e state and prove the equivalence between PAC-learnability, finite VC dimension, and the existence of compression schemes.</p>

Many thanks to Borja Balle, Pascale Gourdeau, and Pierre Ohlmann, for discussions on compression schemes and for the joint effort to understand the proof below.

This post uses all the notations of [this post]({{ '/blog/VC' | prepend: site.baseurl }}) and is greatly inspired by the [lecture notes](https://www.mimuw.edu.pl/~fopss18/PDF/Worrell-lecture-notes.pdf)
of James Worrell for his Logic and Learning lectures last summer in Oxford.

> **Theorem:**
The following are equivalent
* $H$ has finite VC dimension
* $H$ is PAC-learnable
* $H$ has finite compression schemes

The equivalence with compression schemes was an open problem for a long time,
and only proved very recently by [Moran and Yehudayoff](https://arxiv.org/abs/1503.06960).
Since we already proved the equivalence between finite VC dimension and PAC-learnability in [this post]({{ '/blog/fundamental_theorem' | prepend: site.baseurl }}),
we will focus here on the two implications related to compression schemes.

### Compression schemes

A sample compression scheme of size $k$ consists of a compression function and a reconstruction function. 
Given a finite set of samples the compression function returns a subsamples of size at most $k$ and some side information from a finite set.
The reconstruction function uses the subsample to construct a hypothesis for the concept to be learned. 
The reconstructed hypothesis is required to predict the correct label for all examples in the original sample set.

Formally, a sample compression scheme is given by
* A compression map $K : S^* \to S^k \times I$ mapping a sample in $S^*$ to a subsample of size $k$ and side information from a finite set $I$,
* A reconstruction map $\rho : S^* \times I \to (X \to \{0,1\})$ mapping a small sample and side information to a hypothesis.

The requirement is that $\rho (K (S))$ is consistent with $S$.
Two remarks
* using a sample of size $k$ and finite additional information, we construct a hypothesis valid for a sample or arbitrary size, which seems to be very strong,
* but we do not require the reconstruction map to construct a hypothesis in $H$

### A tool: dual class

The dual class $H^*$ is the set of functions $f_x : H \to \{0,1\}$ for $x \in X$ defined by $f_x(h) = h(x)$.

> **Fact:**
If the VC dimension of $H$ is $d$, then the VC dimension of $H^*$ is at most $2^d$

Pascale Gourdeau gave an example showing that this bound is tight.

We write $d^*$ for the VC dimension of $H^*$.

### The easy implication

This consists in saying that a compression scheme induces a PAC-learning algorithm: 
given a sample $S$, the algorithm chooses the hypothesis $\rho( K (S))$ given by the compression scheme.
To prove the PAC bounds involves some (simple) calculation.

### The converse implication

Let $H$ of VC dimension $d$, we construct a compression scheme for $H$.

We need two lemmas.

> **Lemma:**
Let $H$ of VC dimension $d$. 
For all $\varepsilon, \delta > 0$, for any $D$ distribution on $X$ and hypothesis $f$,
there exists $m = O(d)$ such that
$$P_{S \sim D^m}( \exists h \in H, S \text{ consistent with } h \text{ and } \text{err}(D,f,h) \ge \varepsilon) < \delta$$
where
$\text{err}(D,f,h) = P_{x \sim D}(h(x) \neq f(x))$

This says that for $m$ large enough, with high probability any hypothesis consistent with the whole sample has small error.
This is just a reformulation of the fact that finite VC dimension implies PAC-learnability.

> **Lemma:**
Let $H$ of VC dimension $d$. 
Let $D$ be a distribution on $X$ and $\varepsilon > 0$. 
Then there exists a multiset $S \subseteq X$ of cardinality $O(d / \varepsilon^2)$ 
such that for all $h \in H$,
$$| P_{x \sim D}(h(x) = 1) - \frac{| \{x \in S\ :\ h(x) = 1 \}|}{|S|} | < \varepsilon$$

This says that there exists a multiset $S$ which can be used to estimate $P_{x \sim D}(h(x) = 1)$ for any $h \in H$.
We showed this in the proof that a class with finite VC dimension is PAC-learnable, and actually something even stronger:
picking $S$ according to $D$ yields the following bounds with high probability.

Let $L : S^* \to H$ be a learning algorithm, mapping samples to hypothesis.
Thanks to the first lemma with $\varepsilon = 1/3$, we get $m$ such that
for all distributions $D$ on $X$ and hypothesis $f$ there exists $h \in H_m$ such that
$\text{err}(D,f,h) \le 1/3$
where 

$$
H_m = \{ L(S) : S \text{ sample of size } m\}
$$

Given a set of samples $S$, 
we let $H_m(S)$ denote 
$\{ L(S') : S' \text{ subsample of size } m \text{ of } S\}$

> **Claim:**
For all $f \in H$, for all samples $S$, there exists $f_1,\dots,f_k \in H_m(S) \subseteq H_m$ with $k = O(d^*)$ such that for all $x \in S$
$$| \{ i\ :\ f_i(x) = f(x) \}| > k/2$$

The interpretation is that to determine $f(x)$ it is enough to know $f_1(x),\dots,f_k(x)$, and to choose to majority answer.
This will be the key to the reconstruction function in the compression scheme.

We prove the claim.
Consider the following concurrent game, which depends on $f$ and $S$: 
the row player picks $x \in X$, the column player picks $h \in H_m(S)$, and the outcome is $1$ if $h(x) = f(x)$, and $0$ otherwise.

The sentence above the claim exactly says that the column player can ensure a payoff at least $2/3$:
for all distributions $D$ on $X$ there exists $h \in H_m(S)$ such that
$P_{x \sim D}(f(x) = h(x)) \ge 2/3$.

By the minmax theorem, this implies that the column player has a strategy to ensure $2/3$:
there exists a distribution $D^*$ on $H_m(S)$ such that for every $x \in X$,

$$P_{h \sim D^*}(f(x) = h(x)) \ge 2/3$$

We now apply the second lemma to $H^*$ restricted to the elements of $X$ appearing in $S$ with $\varepsilon = 1/8$ (note that $2/3 - 1/8 > 1/2$).
This yields the existence of a set $f_1,\dots,f_k \in H_m(S)$ such that for every $x \in X$,

$$| \{ i\ :\ f_i(x) = f(x) \}| > k/2$$

which proves our claim.

We can now define our compression scheme.

**Compression**: given a sample $S$, we first use $L$ to find a consistent hypothesis $f = L(S)$. 
Thanks to the claim above, there exists $f_1,\dots,f_k \in H_m(S)$, hence there exists subsamples $S_i$ of $S$ of size $m$ 
such that $f_i = L(S_i)$. The compression scheme returns the union of the $S_i$, using the extra information to tell for each element of the union to each $S_i$ it belongs to.
The total number of elements is $O(k m)$, which is $O(d d^*)$.

**Reconstruction**: given the $S_i$, we first obtain the $f_i$ using $L$, 
and then construct a hypothesis $f$ by majority vote: $f(x)$ is the majority answer among $f_1(x),\dots,f_k(x)$.

The compression scheme is correct: $\rho (K (S))$ is consistent with $S$, a direct consequence of the claim.

Note that we do not have a guarantee that $f \in H$.

