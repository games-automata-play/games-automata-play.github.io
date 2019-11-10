---
layout:     post
title:      The complexity of solving games with combination of objectives
date:       2019-11-10 10:00:00
author:     Ashwani Anand and Nathana&euml;l Fijalkow
category:   Automata
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      Parity: "{\\texttt{Parity}}",
      SupMP: "{\\overline{\\texttt{MP}}}",
      InfMP: "{\\underline{\\texttt{MP}}}",
      MultMP: "{\\bigvee_i \\texttt{MP}_i}",
      M: "{\\mathcal{M}}",
      A: "{\\mathcal{A}}",
      Q: "{\\mathbb{Q}}",
      R: "{\\mathbb{R}}",
      C: "{\\mathbb{C}}",
      Z: "{\\mathbb{Z}}",
      N: "{\\mathbb{N}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e investigate the use of separation automata (or equivalently, universal graphs) for combination of objectives.
The general idea is to find ways of combining the solutions for each of the objective into one for the combination, as much as possible treating the solutions for each as black-boxes.
In this post we sketch how to construct separating automata for disjunctions of parity and mean payoff objectives (both, with $\limsup$ and $\liminf$).</p>

This is joint work with Jérôme Leroux, obtained during the internship of Ashwani Anand during the summer 2019.

We consider games whose objectives are given as disjunctions of objectives: each edge is labelled by a pair of colours, 
and $W_1 \vee W_2$ is the objective satisfied if either the projection on the first coordinate satisfies $W_1$ or the projection on the second coordinate satisfies $W_2$.

We consider combinations of the following objectives: $\Parity$, and the two variants of mean payoff: $\SupMP$ and $\InfMP$.
The first says that the supremum limit of the averages is non-negative, and the second uses the infimum limit.
In isolation the two mean payoff objectives are equivalent, but this is no longer the case when combining them for instance with a parity objective.

Our goal is to develop tools using separating automata for solving these questions.
We refer to this [blog post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}) for the basic definitions.
Typically, $n$ is the number of vertices, $m$ the number of edges, and in the context of a mean-payoff objective, $W$ is the largest weight in absolute value.
 
#### A general reduction to strongly connected graphs
Let $W$ be a (positional) objective.
We first show that if we know separating automata for strongly connected graphs, we can use them to construct separating automata for general graphs.

The idea is to construct the separating automata for the general graphs recursively, using the property that any run from the graph will eventually end up in one of the strongly connected components.
A first naive construction consists in taking a sequence of $n$ copies of the separating automata for strongly connected graphs: 
each copy serves as an "attempt", hoping to stabilise in the current strongle connected component. The property above ensures that $n$ attempts are enough.
A finer construction observes that the total size of the strongly connected components is $n$, the number of vertices. 
Hence we can use a "universal sequence" of separating automata for strongly connected graphs, yielding a smaller separating automaton.

#### Separating automata for $\MultMP$
For this case, we use the following lemma.

> **Lemma:**
Let $G$ be a strongly connected graph. 
If $G$ satisfies $\MultMP$ then there exists $i$ such that $G$ satisfies $\InfMP_i$.

If for all $i$ the graph does not satisfy $\InfMP_i$, we can find for each $i$ a bad cycle in the graph. The strongly connected assumption allows us to define a path using each cycle.
By iterating each cycle an increasing number of times, we construct a path in $G$ which does not satisfy $\MultMP$.

Note that this lemma does not hold for $\SupMP$.

This lemma implies that the disjoint union of separating automata for each $\InfMP_i$ yields a separating automaton for strongly connected graphs for $\MultMP$.
Using the previous lemma we construct a separating automaton for $\MultMP$ for general graphs.

> **Corollary:**
There exists a separating automaton for $\MultMP$ of size $O(n \cdot k \cdot nW)$, 
inducing an algorithm for solving these games with complexity $O(m \cdot k \cdot n^2 \cdot W)$.

#### Separating automata for $\Parity \vee \InfMP$
The objective $\Parity \vee \InfMP$ is over the colours $[1,d] \times [-W,W]$, it is satisfied is either the left component satisfies parity or the right component satisfies mean payoff.

Let $\A_{\Parity}$ be a separating automaton for $\Parity$ and $\A_{\InfMP}$ a separating automaton for $\MP$.
We construct a separating automaton $\A$ for $\Parity \vee \InfMP$, which is roughly speaking a **wreath product** of the two automata.

A set of states of $\A$ is a pair of one state of $A_{\Parity}$ and one state of $\A_{\InfMP}$, plus a priority, which is the maximal priority seen since the last reset (or since the beginning).
At each transition $\A$ simulates the automaton $\A_{\InfMP}$. 
If this simulation fails, i.e. the automaton $\A_{\InfMP}$ rejects, then the automaton resets, which means two things:
it simulates the automaton $\A_{\Parity}$ using the maximal priority seen since last reset, and resets the state of $\A_{\InfMP}$ to its initial state.

The automaton $\A$ rejects if at some point $\A_{\Parity}$ rejects. In any other case, it accepts.

The proof of correctness of this construction a slightly involved, but rather elegant and short (one page long).

> **Corollary:**
There exists a separating automaton for $\Parity \vee \InfMP$ of size $O(d \cdot |\A_{\Parity}| \cdot |\A_{\InfMP}|)$, 
inducing a pseudo-quasi-polynomial algorithm for solving these games.

### Open problem
We do not know how to construct separating automata for the objective $\SupMP_1 \vee \SupMp_2$, as studied in [this paper](https://arxiv.org/abs/1210.3141).

