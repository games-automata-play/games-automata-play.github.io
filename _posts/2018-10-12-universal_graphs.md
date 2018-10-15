---
layout:     post
title:      Parity games and universal graphs 
date:       2018-10-12 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Parity games
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

<p class="intro"><span class="dropcap">T</span>his post introduces the notion of universal graphs and their applications to parity games.</p>

The goal of this post is to prove the main result of [this paper](https://arxiv.org/abs/1807.10546),
meaning that separating automata have at least quasipolynomial size.

This quasipolynomial barrier subjects all three quasipolynomial time algorithms for parity games as explained in the post about [separating automata]({{ '/blog/parity_separation' | prepend: site.baseurl }}).

This post relies on the previous post about the [fundamental theorem for parity games]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}).
All details can be found in this [technical report](https://arxiv.org/abs/1810.05106) which is a joint work with Thomas Colcombet.

### Universal graphs
We fix $n,d$ two parameters.

> **Definition:** A graph is **universal** if it satisfies parity and 
any graph of size $n$ satisfying parity can be mapped homomorphically into it.


We show that universal graphs can be used to construct a conceptually simple algorithm for parity games.
Consider a game $\G$ and a universal graph $\U$, we construct a safety game $\G \times \U$ 
where Eve chooses which edge to follow on the second component,
and she wins if she manages to play forever.

> **Theorem:**
Let $\G$ be a parity game and $\U$ a universal graph.
Then Eve wins in $\G$ if and only if she wins in $\G \times \U$.

**Proof:**
Let us assume that Eve wins in $\G$, let $\sigma$ be a positional strategy in $\G$. 
We consider the graph $\G[\sigma]$, by definition there exists a homomorphism from $\G[\sigma]$ to $\U$. 
We construct a winning strategy in $\G \times \U$ by playing as in $\sigma$ in the first component
and following the homomorphism on the second component.
Conversely, a winning strategy in $\G \times \U$ induces a winning strategy in $\G$ since $\U$ satisfies parity.

It follows that solving the parity game is equivalent to solving a safety game of size $n \times \|\U\|$, where $\|\U\|$ is the size of the universal graph.
Since solving a safety game can be done in linear time, this gives an algorithm whose running time is linear in $n$ and $\|\U\|$.

If you have just read the post about [separating automata]({{ '/blog/parity_separation' | prepend: site.baseurl }}) what is above should sound very familiar.

#### In the remainder of this post we prove the following theorem:

> **Theorem:**
The following quantities are equal
* The size of the smallest universal tree
* The size of the smallest separating automaton
* The size of the smallest universal graph

We refer to [this post]({{ '/blog/universal_tree' | prepend: site.baseurl }}) for the definition of universal trees
and [this post]({{ '/blog/parity_separation' | prepend: site.baseurl }}) for the definition of separating automata.

Thanks to the upper and lower bounds for universal trees (see [this post]({{ '/blog/universal_tree' | prepend: site.baseurl }})) we know that the above quantity
is quasi-polynomial.

The equivalence between universal trees and separating automata was originally proved in [this paper](https://arxiv.org/abs/1807.10546).
The innovations of the [new paper](https://arxiv.org/abs/1810.05106) is to use a different route, using universal graphs
and the [fundamental theorem]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}).

The outline is as follows.
1. construction [from a universal graph to a universal tree](#graph_to_tree)
2. construction [from a universal tree to a (deterministic) separating automaton](#tree_to_automaton)
3. construction [from a (non-deterministic) separating automaton to a universal graph](#automaton_to_graph)

### <a name="graph_to_tree">Universal graph to universal tree</a>
> **Lemma**
A maximal universal graph is a universal tree.

This is an easy corollary of the lemma proved in this [post]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }})
which says that a maximal graph is a tree.
It follows that from a universal graph, one constructs a universal tree by taking a saturation of the universal graph,
which preserves the size.

### <a name="tree_to_automaton">Universal tree to separating automaton</a>
Let $T$ be a universal tree. 
We construct an automaton $\A$ as follows.
The set of states is the set of leaves of $T$. 
The initial state is the minimum element for the total order $E_0$.
The transition function is defined as follows.

$$
\delta(v,i) = \min_{E_0} \set{v' : (v,i,v') \in E}
$$

Note that $\A$ is deterministic.

> **Lemma:**
The automaton $\A$ is separating.

**Proof:**
Consider a graph $G$ of size $n$ satisfying parity, we show that $\A$ accepts all paths in $G$.
Thanks to the [fundamental theorem]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}), there exists a tree and a homomorphism $\phi : G \to t$.

We show by induction that for a finite path $\rho$ in $G$ ending in $v$, 
we have $(\delta(\pi(\rho)), 0, \phi(v)) \in E$.
To initialise we recall that the initial state is the minimal element for $E_0$.
For the inductive case let $\rho' = \rho \cdot (v,i,v')$, we are looking at $\delta(\pi(\rho')) = \delta(\delta(\pi(\rho)),i)$.
Since $(v,i,v') \in E$ we have $(\phi(v),i,\phi(v')) \in E$,
and by induction hypothesis we have $(\delta(\pi(\rho)), 0, \phi(v)) \in E$,
the combination of this implies that $(\delta(\pi(\rho)), i, \phi(v')) \in E$.
It follows by definition of $\delta$ that $(\delta(\pi(\rho')), 0, \phi(v')) \in E$.

To see that the automaton rejects all paths not satisfying parity, consider a path $\rho$ with odd maximal priority $i$.
We note that for any state $v$ we have $(v,i,\delta(v,\pi(\rho))) \in E$, which is easily shown by induction.
Since a path not satisfying parity contains a suffix consisting of infinitely many paths 
each of which has odd maximal priority $i$, it follows that the automaton eventually rejects such a path.
Indeed, $i$ being odd $E_i$ is a non-reflexive preorder, so the sequence of states at the beginning of each loop 
would be an infinite increasing sequence of states.


### <a name="automaton_to_graph">Separating automaton to universal graph</a>
Let $\A$ be a separating automaton. 
We will do the proof assuming that $\A$ is deterministic, and later explain why it straightforwardly extend to non-deterministic automata.
We write $\delta(u)$ for the state reached after reading the word $u$ in $\A$ from the initial state; 
note that $\delta(u)$ might be undefined.
Without loss of generality all states in $\A$ are reachable.
We construct a graph $G$ as follows.
The set of vertices is the set of states of $\A$, and $(v,i,v') \in E$ if $v' = \delta(v,i)$.

> **Lemma:**
The following two properties hold.
* $G$ satisfies parity
* Any saturation of $G$ is universal

**Proof:**
To see that $G$ satisfies parity, we observe that all cycles of $G$ are even, since otherwise $\A$ would accept a word not satisfying parity.
We consider a saturation of $G$ which for the sake of simplicity we also call $G$.
Recall that thanks to a lemma proved in this [post]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}),
we know that the relation $E_0$ is a total order.

Let $H$ be a graph satisfying parity of size $n$, we construct a homomorphism $\phi$ from $H$ to $G$.
We need to associate to any vertex $v$ of $H$ a vertex $\phi(v)$ of $G$.
Define

$$
\phi(v) = \max_{E_0} \set{\delta(\pi(\rho)) : \rho \text{ path of } H \text{ ending in } v},
$$

where $\pi(\rho)$ projects the path $\rho$ onto a sequence of priorities.
Note that since $H$ satisfies parity, for each path $\rho$ of $H$ we have that $\delta(\pi(\rho))$ is well defined.

We verify that $\phi$ is a homomorphism.
Let $(v,i,v') \in E$, we show that $(\phi(v),i,\phi(v')) \in E$.
By definition $\phi(v) = \delta(\pi(\rho))$ for $\rho$ some path ending in $v$.
Note that $\rho' = \rho \cdot (v,i,v')$ is a path ending in $v'$, so by definition $(\delta(\pi(\rho')),0,\phi(v')) \in E$.
Since $\delta(\pi(\rho')) = \delta(\delta(\pi(\rho)),i)$ and $\phi(v) = \delta(\pi(\rho))$ we have $(\phi(v),i,\delta(\pi(\rho'))) \in E$.
It follows thanks to the first property of trees that $(\phi(v),i,\phi(v')) \in E$.

The proof goes through if $\A$ is non-deterministic with two adjustments
* $\phi(v)$ is the maximum for the $E_0$ order over all states reachable through some path of $H$ ending in $v$
* we do not have that $\phi(v) = \delta(\pi(\rho))$, but only that $\phi(v) \in \delta(\pi(\rho))$


