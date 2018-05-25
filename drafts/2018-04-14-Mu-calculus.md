---
layout:     post
title:      A mu-calculus algorithm for parity games
date:       2018-04-14 9:00:00
author:     Nathana&euml;l Fijalkow
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      G: "{\\mathcal{G}}",
      VE: "{V_E}",
      Parity: "{\\mathrm{Parity}}",
      Pre: "{\\mathrm{Pre}}",
      Win: "{\\mathrm{Win}}",
      Lose: "{\\mathrm{Lose}}",
      Reach: "{\\mathrm{Reach}}",
      Safe: "{\\mathrm{Safe}}",
      LFP: "{\\mathrm{LFP}}",
      GFP: "{\\mathrm{GFP}}",
      N: "{\\mathbb{N}}",
      nN: "{_{n \\in \\mathbb{N}}}",
      priority: "{\\{1,\\ldots,d\\}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post constructs a recursive (exponential) algorithm for parity games.
Evidence that it is good, it has been invented many times.
One may recognise it in the works of Emerson and Jutla (91) using modal mu-calculus, and also more explicitely in the works of Kupferman and Vardi (98).
</p>

<p>
This algorithm is different from Zielonka's algorithm.
One of the point of this post is to extract from this algorithm the notion of signatures, which is the key to the correctness proof for 
the small progress measure algorithm of Jurdzi&#324;ski.
</p>

<p>
The analysis of the algorithm will give us as a by-product determinacy, positional determinacy and the existence of signatures.
Note that we do not assume anything here, the proofs are built from (almost) first principles.</p>

<p>We fix some notations. Consider a finite parity game with $n$ vertices and priorities in $[1,d]$.
The parity condition is satisfied by a play if the **maximal** priority seen infinitely often is even,
we write $\Parity_d$ to mean that the priorities range in $[1,d]$.
The two players are Eve and Adam, she controls the vertices depicted by circles and he controls the vertices depicted by squares.
We let $W_E(\Parity_d)$ denote the set of vertices from which Eve has a winning strategy, and $W_A(\Parity_d)$ for Adam.</p>

<p>For a set of vertices $U \subseteq V$, we let $\Pre(U) \subseteq V$ be the set of vertices from which Eve can ensure to reach $U$ in one step:
$$\Pre(U) = \{u \in V_E \mid \exists (u,v) \in E,\ v \in U\} \cup \{u \in V_A \mid \forall (u,v) \in E,\ v \in U\}.$$
We use $\overline{\Pre(U)}$ for the complement of $\Pre(U)$.
The condition $\Reach(U)$ is satisfied by plays visiting $U$ at least once, and $\Safe(U)$ by plays never visiting $U$.
We write $d$ for the set of vertices of priority $d$.</p>

### Two recursive lemmas yield a proof of positionality

> **Lemma:** 
Assume that $d$ is even.
$W_E(\Parity_d)$ is the greatest fixed point of the function $Y \mapsto W_E(\Parity_{d-1}\ \cup\ \Reach(d \cap \Pre(Y)))$

In words: $W_E(\Parity_d)$ is the largest set of vertices $Y$ such that from $Y$ Eve has a strategy ensuring that 
* either the priority $d$ is never seen, in which case the parity condition is satisfied with lower priorities,
* or the priority $d$ is seen, in which case Eve can ensure to reach $Y$ in one step.

**Proof:**
The fact that $W_E(\Parity_d)$ contains the greatest fixed point follows from the observation that any fixed point $Y$ is contained in $W_E(\Parity_d)$.
Indeed, if $Y$ is a fixed point, the strategy described above ensures parity: either it visits finitely many times $d$,
and then from some point onwards the parity condition is satisfied with lower priorities, or it visits infinitely many times $d$,
and then the parity condition is satisfied because $d$ is maximal and even.

Note that we are constructing positional strategies by taking the disjoint union of two positional strategies,
one for $W_E(\Parity_{d-1})$ for vertices of priorities less than $d$ and the other for $d \cap \Pre(W_E(\Parity_{d-1}))$.

The fact that $W_E(\Parity_d)$ is included in the greatest fixed point follows from the fact that it is itself a fixed point,
which is easy to check.

> **Lemma:** 
Assume that $d$ is odd.
$W_E(\Parity_d)$ is the least fixed point of the function $X \mapsto W_E(\Parity_{d-1} \cap \Safe(d \cap \overline{\Pre(X)}))$

In words: $W_E(\Parity_d)$ is the smallest set of vertices $X$ such that from $X$ Eve has a strategy ensuring that 
* either the priority $d$ is never seen, in which case the parity condition is satisfied with lower priorities,
* or the priority $d$ is seen, in which case Eve can ensure to reach $X$ in one step.

**Proof:**
The fact that $W_E(\Parity_d)$ contains the least fixed point follows from the fact that it is itself a fixed point,
which is easy to check.

The fact that $W_E(\Parity_d)$ is included in the least fixed point is the interesting non-trivial bit.
It follows from the observation that any fixed point $X$ contains $W_E(\Parity_d)$.
To prove this, we show that $V \setminus X \subseteq W_A(\Parity_d) \subseteq V \setminus W_E(\Parity_d)$.
Note that here we are not relying on the determinacy of parity games: the second inclusion is very simple and always true,
it only says that Eve and Adam cannot win from the same vertex.

Indeed, if $X$ is a fixed point, from $V \setminus X$ Adam has a strategy ensuring that
* either the priority $d$ is never seen, in which case the parity condition is violated with lower priorities,
* or the priority $d$ is seen, in which case Adam can ensure to reach $V \setminus X$ in one step.

This strategy violates parity: either it visits finitely many times $d$,
and then from some point onwards the parity condition is violated with lower priorities, 
or it visites infinitely many times $d$, and then the parity condition is violated because $d$ is maximal and odd.

**Remark:**
We could have said that the odd case is symmetric to the even case, swapping the role of the two players.
Indeed, the two claims are in some sense dual to each other (more precisely, this amount to negate the modal mu-calculus formula).
We do not take this road, because it requires assuming determinacy of parity games, which we avoided in this presentation,
and obtained as a corollary.

### The algorithm

To be continued!

<!--

<p>We construct two recursive procedures, which take as input a parity game with priorities in $[1,p]$ 
and two additional colours: $\set{\Win,\Lose}$, and output the winning set for Eve.
The vertices with colours $\Win$ or $\Lose$ are terminal: when reaching a terminal vertex, 
the game stops and one of the players is declared the winner.
Formally, the objective is
$$\left( \Parity_p \cup \Reach(\Win) \right) \cap \Safe(\Lose)$$.
</p>

Algorithmically, we compute a non-increasing sequence of sets $Y_0 \supseteq Y_1 \supseteq \cdots$,
such that $Y_{k+1} = W_E(\Parity_{d-1}\ \cup\ \Reach(d \cap \Pre(Y_k)))$ until reaching the greatest fixed point.

For each $k$ this is indeed a recursive call: in the new game, vertices with priorities $d$ are marked terminal, and declared
winning if in $d \cap \Pre(Y_k)$, losing otherwise. So in this game the priorities are in $[1,d-1]$.

<figure>
	<img src="{{ '/images/parity_even.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The even case</figcaption>
</figure>

Algorithmically, we compute a non-increasing sequence of sets $X_0 \subseteq X_1 \subseteq \cdots$,
such that $X_{k+1} = W_E(\Parity_{d-1} \cap \Safe(d \cap \overline{\Pre(X_k)}))$ until reaching the least fixed point.

<figure>
	<img src="{{ '/images/parity_odd.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The odd case</figcaption>
</figure>

**Complexity:**
The algorithm above alternates greatest and least fixed point computations, in total $d$ of them.
Each of them computes subsets of the vertices, hence stabilise within at most $n$ steps.
Each step can be carried out in linear time, so we obtain a naive but good enough time complexity bound of $O(n^d)$.

### The construction of signatures

Let us fix some notations. 
* For an even priority $p$, let $Y(p)$ be the greatest fixed point computed at step $p$.
* For an odd priority $p$, let $X_0(p) \subseteq X_1(p) \subseteq \cdots \subseteq X_n(p)$ be the non-decreasing sequence of sets of vertices computed at step $p$.

We define a function $\mu : V \to [1,n]^{d/2} \cup \\{\bot\\}$.
The tuples in $[1,n]^{d/2}$ are indexed by odd priorities in $[1,d]$.
For $p$ an odd priority, let $\mu(p)(v)$ be the smallest $k$ such that $v$ is in $X_k(p)$, and $\bot$ if it belongs to none.

The function $\mu$ induces a set of orders on vertices: for $p$ an odd priority and $v,v'$ two vertices, we have $v \ge_p v'$ if
$\mu(p)(v) \ge \mu(p)(v')$. For technical convenience we also define $\ge_p$ for $p$ an even priority by $\ge_p = \ge_{p-1}$.

**Theorem:**
* $\mu(v) \neq \bot$ if, and only if, Eve wins from $v$,
* if $v \in V_E$ has priority $p$, then there exists $(v,v') \in E$ such that $v \ge_p v'$, strict if $p$ odd,
* if $v \in V_A$ has priority $p$, then for all $(v,v') \in E$ we have $v \ge_p v'$, strict if $p$ odd.

**Proof:**
The first item has already been argued in the description of the algorithm.
The other two items are easily proved, relying on two arguments:
* for $v$ of priority $p$ in $X_k(p')$ with $p' > p$, the positional winning strategy ensures to remain in $X_k(p')$,
* for $v$ of odd priority $p$ in $X_k(p)$, the positional winning strategy ensures to reach $X_{k-1}(p)$.


-->
