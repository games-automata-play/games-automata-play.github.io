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

<p class="intro"><span class="dropcap">T</span>his post discusses the equivalence between separating automata and universal graphs beyond parity games.</p>

The goal of this post is to present **again** the result of [this paper](https://arxiv.org/abs/1810.05106), which is a joint work with Thomas Colcombet, but this time in more generality.
The paper talks about parity games, and shows the equivalence between universal graphs, universal trees, and separating automata, as presented in 
[this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).
The result is actually much more general as it applies to any half-positional winning condition.
In particular this will be useful for studying mean payoff games, see [this post]({{ '/blog/mean_payoff' | prepend: site.baseurl }}).

An important remark is that for parity games, a third notion comes into play: universal trees.
This notion is very specific to the parity condition, and does not extend to other winning conditions.
The other two notions, separating automata and universal graphs, are naturally extended in the following way.

Why are these two notions interesting?
* **Separating automata** yield a family of algorithms for solving games by reduction to safety games.
Given the success story of solving parity games using separating automata, it is natural to ask whether one can use separating automata for constructing efficient algorithms
for other types of games.
* **Universal graphs** yield another family of algorithms for solving games by value iteration.

We consider graphs and games whose edges are labelled by $C$, a set of labels.
The size of a graph is its number of vertices.
We fix $$W \subseteq C^\omega$$ a winning condition, and we make one assumption, called (half-)positionality of $W$:
We say that a graph satisfies $W$ if all paths in the graph satisfy $W$.

> **Assumption:** In any finite game, if Eve has a strategy ensuring $W$, then she has a positional strategy ensuring $W$.

We fix $n$ a parameter.

### Separating automata

The automata we consider are deterministic safety automata over infinite words on the alphabet $C$, 
where safety means that all states are accepting: a word is rejected if there exist no run for it.
The size of an automaton is its number of states.

In the following we say that a path in a graph is accepted or rejected by an automaton; this is an abuse of language
since what the automaton reads is only the labels of the corresponding path.

> **Definition:** An automaton is **$$n$$-separating** if the two following properties hold.
* For all graphs of size $n$ satisfying $W$, the automaton accepts all paths in the graph
* All words accepted by the automaton are in $W$

The case of separating parity automata is discussed in more details in [this post]({{ '/blog/parity_separation' | prepend: site.baseurl }}).
The use of separating automata is to solve games with condition $W$ by reduction to safety games (in exactly the same way as was done for parity games).

> **Lemma:**
Let $L$ be the language recognised by a $n$-separating automaton $$\A$$.
Then for all games $G$ of size $n$, we have
that Eve has a strategy ensuring $W$ if and only she has a strategy ensuring $L$.

**Proof:**
Let us first assume that Eve has a strategy $\sigma$ ensuring $W$.
It can be chosen positional thanks to our assumption on $W$.
Then $G[\sigma]$, the graph obtained by restricting the game $G$ to the moves prescribed by $\sigma$, is a graph of size $n$ satisfying $W$, so $$\A$$ accepts all paths in $G[\sigma]$.
In other words, all paths consistent with $\sigma$ are in $L$, or equivalently $\sigma$ ensures $L$.

Conversely, assume that Eve has a strategy $\sigma$ ensuring $L$.
Since $L \subseteq W$ thanks to the second item of the definition of separating automata, the strategy $\sigma$ also ensures $W$.

It follows that solving the game $W$ is equivalent to solving a safety game with $m \times \|A\|$ edges, 
where $m$ is the number of edges of $G$ and $\|A\|$ the number of states of the separating automaton.
Since solving a safety game can be done in linear time in the number of edges, this gives an algorithm whose running time is linear in $m$ and $\|A\|$.

### Universal graphs

> **Definition:** A graph is **$$n$$-universal** if it satisfies $$W$$ and 
any graph of size $n$ satisfying $$W$$ can be mapped homomorphically into it.

The case of universal parity graphs is discussed in more details in [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).

## Equivalence result

> **Theorem:**
* For every $n$-separating automaton one can construct an $n$-universal graph of the same size
* For every $n$-universal graph one can construct an $n$-separating automaton of the same size

All the ideas of the proof are in the parity games case, as presented in [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).
For the sake of completeness the full proof is given in [this paper](https://arxiv.org/abs/1812.07072) for the case of mean payoff games.
In both cases, the **key observation** is that the set of vertices (or the set of states, depending on the direction) can be totally ordered.

