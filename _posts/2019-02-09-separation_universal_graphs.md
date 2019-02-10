---
layout:     post
title:      Separating automata and universal graphs 
date:       2019-02-09 9:00:00
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

<p class="intro"><span class="dropcap">T</span>his post discusses the equivalence between separating automata and universal graphs in a general framework.</p>

The goal of this post is to present **again** the result of [this paper](https://arxiv.org/abs/1810.05106), which is a joint work with Thomas Colcombet, but this time in more generality.
The paper talks about parity games, and shows the equivalence between universal graphs, universal trees, and separating automata, as presented in 
[this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).
The result is actually much more general, and the point of this post is to explain how.
In particular this will be useful for studying mean payoff games, see [this post]({{ '/blog/mean_payoff' | prepend: site.baseurl }}).

An important remark is that for parity games, a third notion comes into play: universal trees.
This notion is very specific to the parity condition, and does not extend to other winning conditions.
The other two notions, separating automata and universal graphs, are naturally extended in the following way.

We fix $$W \subseteq C^\omega$$ a winning condition, and we make one assumption, called (half-)positionality of $W$:

> **Assumption:** In any finite game with condition $W$, if Eve has a winning strategy, then she has a positional winning strategy.

We consider graphs whose edges are labelled by $C$.
We say that a graph satisfies $W$ if all paths in the graph satisfy $W$.

We fix $n$ a parameter.

### Separating automata

The automata we consider are deterministic safety automata over infinite words on the alphabet $C$, 
where safety means that all states are accepting: a word is rejected if there exist no run for it.

In the following we say that a path in a graph is accepted or rejected by an automaton; this is an abuse of language
since what the automaton reads is only the labels of the corresponding path.

> **Definition:** An automaton is **separating** if the two following properties hold.
* For all graphs of size $n$ satisfying $W$, the automaton accepts all paths in the graph
* The automaton rejects all paths not satisfying $W$

The case of separating parity automata is discussed in more details in [this post]({{ '/blog/parity_separation' | prepend: site.baseurl }}).
The use of separating automata is to solve games with condition $W$ by reduction to safety games, in exactly the same way as for parity games.

> **Lemma:**
Let $L$ be the language recognised by a separating automaton $$\A$$.
Then for all games $G$ with $n$ vertices, we have
that Eve has a winning strategy for the condition $W$ if and only she has a winning strategy for the condition $L$.

**Proof:**
Let us first assume that Eve has a strategy $\sigma$ ensuring $W$.
It can be chosen positional thanks to our assumption on $W$.
Then $G[\sigma]$ satisfies $W$.
So $$\A$$ accepts all paths in $G[\sigma]$.
In other words, all paths consistent with $\sigma$ are in $L$, or equivalently $\sigma$ ensures $L$.

Conversely, assume that Eve has a strategy $\sigma$ ensuring $L$.
Since $L \subseteq W$ thanks to the second item of the definition of separating automata, the strategy $\sigma$ also ensures $W$.

It follows that solving the game $W$ is equivalent to solving a safety game of size $n \times \|A\|$, where $\|A\|$ is the number of states of the separating automaton.
Since solving a safety game can be done in linear time, this gives an algorithm whose running time is linear in $n$ and $\|A\|$.

### Universal graphs

> **Definition:** A graph is **universal** if it satisfies $$W$$ and 
any graph of size $n$ satisfying $$W$$ can be mapped homomorphically into it.

The case of universal parity graphs is discussed in more details in [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).

## Equivalence result

> **Theorem:**
* For every separating automaton one can construct a universal graph of the same size
* For every universal graph one can construct a separating automaton of the same size

The proof is in essence the same as for parity games, as presented in [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).
For instance, for the sake of completeness it is spelled out in [this paper](https://arxiv.org/abs/1812.07072) for the case of mean payoff games.

<!--
The outline is as follows.
1. construction [from a universal graph to a separating automaton](#graph_to_automaton)
2. construction [from a separating automaton to a universal graph](#automaton_to_graph)

### <a name="graph_to_automaton">Universal tree to separating automaton</a>
Let $G$ be a universal graph.
We construct an automaton $\A$ as follows.
The set of states is the set of vertuces of $G$. 
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
-->

