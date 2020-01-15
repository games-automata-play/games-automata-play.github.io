---
layout:     post
title:      The complexity of solving games with combination of objectives
date:       2019-10-06 10:00:00
author:     Ashwani Anand and Nathana&euml;l Fijalkow
category:   Automata
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      Parity: "{\\texttt{Parity}}",
      MP: "{\\texttt{MP}}",
      SupMP: "{\\overline{\\texttt{MP}}}",
      InfMP: "{\\underline{\\texttt{MP}}}",
      M: "{\\mathcal{M}}",
      A: "{\\mathcal{A}}",
      Q: "{\\mathbb{Q}}",
      R: "{\\mathbb{R}}",
      C: "{\\mathbb{C}}",
      Z: "{\\mathbb{Z}}",
      N: "{\\mathbb{N}}",
      Na: "{\\mathcal{A}}",
      B: "{\\mathcal{B}}",
      Nw: "{\\mathcal{W}}",
      Lna: "{\\mathcal{L}(\\mathcal{A})}",
      Cc: "{\\mathcal{C}}",
      Bo: "{\\mathcal{O}}",
      Game: "{\\mathcal{G}}",
      Ap: "{\\Na_{\\texttt{P}}}",
      Amp: "{\\Na_{\\texttt{MP}}}",
      Ampp: "{\\Na_{\\texttt{MP,P}}}",
      Lap: "{\\mathcal{L}}",
      Lamp: "{\\mathcal{L}(\\Na_{\\texttt{MP}})}",
      Lampp: "{\\mathcal{L}(\\Na_{\\texttt{MP,P}})}",
      Mult: "{W_1\\vee W_2}",
      Multmpp: "{\\texttt{MP}\\vee\\texttt{P}}",
      Multmpmps: "{\\Supmp\\vee\\Supmp}",
      Multmpmpi: "{\\Infmp\\vee\\Infmp}",
      Multlang: "{W_1\\vee W_2\\vee\\ldots\\vee W_k}",
      Multkmp: "{\\Infmp_1\\vee\\Infmp_2\\vee\\ldots\\vee\\Infmp_d}",
      Multkp: "{\\Parity_1\\vee\\Parity_2\\vee\\ldots\\vee\\Parity_d}",
      Multkw: "{W_1\\vee W_2\\vee\\ldots\\vee W_k}",
      bo: "{\\mathcal{O}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e investigate the use of separation automata for combination of objectives.
The general idea is to find ways of combining the solutions for each of the objective into one for the combination, as much as possible treating the solutions for each as black-boxes.
In this post we sketch how to construct separating automata for disjunctions of parity and mean payoff objectives (both, with $\limsup$ and $\liminf$).</p>

This is joint work with Jérôme Leroux, obtained during the internship of Ashwani Anand.

We consider games whose objectives are given as disjunctions of objectives: each edge is labelled by a pair of colours, 
and $W_1 \vee W_2$ is the objective satisfied if either the projection on the first coordinate satisfies $W_1$ or the projection on the second coordinate satisfies $W_2$.

We consider combinations of the following objectives: $\Parity$, and the two variants of mean payoff: $\SupMP$ and $\InfMP$.
The first says that the supremum limit of the averages is non-negative, and the second uses the infimum limit.
In isolation the two mean payoff objectives are equivalent, but this is no longer the case when combining them for instance with a parity objective.

Our goal is to develop tools using separating automata for solving these questions.
We refer to this [blog post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}) for the basic definitions.
The typical result we would like to obtain is: assume that $A_1$ is a separating automaton for $W_1$ and $A_2$ a separating automaton for $W_2$,
we construct a new automaton $A$ based on $A_1$ and $A_2$ (if possible, as black-boxes) which is a separating automaton for $W_1 \vee W_2$.

#### A general reduction to strongly connected graphs
Let $W$ be a (positional) prefix-independent objective.
We first show that if we know universal graphs for strongly connected graphs, we can use them to construct universal graphs for general graphs.
Let us make the definitions more precise. We recall the definition of universal graphs.

> **Definition:** A (deterministic) automaton over $C^\omega$ is **$(n,W)$-separating** if all accepting words satisfy $W$ and 
it accepts all paths of graphs of size $n$ satisfying $W$.

A (deterministic) automaton over $C^\omega$ is **$(n,W)$-SC-separating** if all accepting words satisfy $W$ and 
it accepts all paths of strongly connected graphs of size $n$ satisfying $W$.

The key remark is that any path in a graph ends up in a strongly connected component, and goes through at most $n$ such components.
Hence a first naive construction is to construct a sequence of $n$ copies of a $(n,W)$-SC-separating automaton.
This can be refined by observing that the total size of the components is $n$ and using a "universal sequence" approach.

#### A separating automaton for disjunctions of $\InfMP$
> **Lemma:**
Given a strongly connected graph $G$, 
if $G$ satisfies a disjunction of objectives $\InfMP$, then it satisfies one of them.

The proof is that if the graph does not satisfy any of the individual $\InfMP$, then we find a sequence of cycles in the graph, with one bad cycle for each objective.
Iterating these cycles yields a path which does not satisfy any of the objective, hence the goal does not satisfy the disjunction.

Note that this lemma does not hold for $\SupMP$.

Now, assume we have $A_i$ an $(n,\InfMP_i)$-SC-separating automaton for each $i$.
Thanks to the lemma above, the disjoint union of the $A_i$'s yield an $(n,\bigvee_i \InfMP_i)$-SC-separating automaton.
The construction above turns it into an $(n,\bigvee_i \InfMP_i)$-separating automaton.

#### A separating automaton for $\InfMP \vee \Parity$

CONTINUE HERE!!!!!!!!!!!!!!!

The intuition is to run a word $w \in (C \times C)^{\omega}$ on both $\A_{\Parity}$ and $\A_{\mp}$ simulataneously, i.e. first co-ordinate on $\A_{\mp}$ and second on $\A_{\Parity}$. We store in a control state the maximum we priority we have seen till now in the second co-ordinate of the word. When $\A_{\mp}$ rejects the word, we take the transition with the stored max priority in $\A_{\Parity}$, and reset $\A_{\mp}$ to initial state, and continue like above. The word is rejected if $\A_{\Parity}$ can not make a move when $\A_{\mp}$ rejects. The word is accepted if the above mentioned procedure never stops. 

Now, to show that proof of correctness, we use the following lemma.

> **Lemma:**
Let G be a graph with $\|G\| = n$ and $\pi \in Path(G)$. For $W \in {\mp, \Parity}$, if $(n, W )−$separating automata rejects $\pi$ at position $i$, then the prefix $\pi_{\leq i}$ contains a corresponding negative cycle.

The proof of this lemma is simple. If not, then we consider a subgraph which contains only the edges in $\pi_{\leq i}$, and add a self loop on the last vertex with color 0. Any path in this graph should be accepted by the separating automata but the path $ \pi'=\pi_{\leq i}\cdot (0)^{\omega} $, is rejected at position $i$.

Now, if a graph $G$ satisfies $\Multmpp$, then any path will not be rejected by our separating automata, because if it is rejected, then, by previous lemma, we get a cycle in the graph which is negative for parity and which contains smaller cycles which are negative for mean payoff. We can iterate these cycles to obtain a a run of the graph which does not satisfy $\Multmpp$, which gives a contradiction. 

Given a word $w\notin \Multmpp$, $\A_{\mp}$ and $\A_{\Parity}$ reject $w$ infinitely often and hence our constructed separating automata will also reject $w$.

#### Separating automata for $\Infmp_1\vee\Infmp_2\vee\ldots\Infmp_{k_1}\vee\Parity_1\vee\Parity_2\vee\ldots\Parity_{k_2}$
The construction idea of the separating automata is exactly the same as the previous one. The separating automaton records the max priority seen in each parity co-ordinate and taking a move on the same when the separating automata for previous co-ordinate rejects, for all but first parity co-ordinate. In the first parity co-ordinate, the automaton takes a transistion everytime any of the mean payoff separating automata reject.

#### Separating automata for $\Multkp$
The construction has the similar idea as the previous two. We take $k$ copies of the $(n, \Parity)−$separating automata, and run the word on all the copies simultaneously as follows. Store the heighest priority seen till now in all the co-ordinates except the first one. Take transion with the max priority seen till now, on $i^{th}$ co-ordinate if $(i−1)^{th}$ separating automata rejects, and reset the $(i − 1)^{th}$ separating automata. In the first co-ordinate take transition on the current letter. Reject if $k^{th}$ copy rejects.

This concludes the proof of the theorem. As a corollary, we obtain the following complexity result.
> **Corollary:**
* A $ \Multkmp $ game of size $ n $ and $ m $ edges, can be solved in time $ \bo(mn\log n) $.
* There is an algorithm to solve an $ (n,\Multmpp)- $game in time complexity $ m \|\A\|\cdot \|\B\| $, where $ \|\A\| $ is the size of a $ (n,\mp)- $ separating automaton, i.e. $  O(nW) $, and $ \|\B\| $ the size of a $ (n,\Parity)- $separating automaton, i.e. $ {log(n) + d \choose d} $, where $ W $ is the maximum color.
* There is an algoritm solving an $ (n,\Infmp_1\vee\Infmp_2\vee\ldots\Infmp_{k_1}\vee\Parity_1\vee\Parity_2\vee\ldots\Parity_{k_2}) $ game in time $m \|\A\|^{k_1}\times (\|\B\|\times p)^{k_2}$, where $ \A $ and $ \B $ are $ (n,\mp) $ and $ (n,\Parity) $ separating automata, $ m $ is the number of edges in the underlying graph and $ p=max(C) $.
* There is an algoritm solving an $ (n,\Multkp) $ game in time $m \times (\|\A\|\times p)^{d}$, where $ \A $ is $ (n,\Parity)-$ separating automata, $ m $ is the number of edges in the underlying graph and $ p=max(C) $.

### Open problem
* However [this paper](https://arxiv.org/abs/1210.3141) gives an algorithm to solve games with the objective $ \Supmp_1\vee\Supmp_2\vee\ldots\Supmp_k $, we could not find a way to solve it using separating automata technique. We could not do it even for 2-dimensional case.


> **Theorem:**
* There exists a universal graph of size $dn\log n$, for the winning objective $\Multkmp$.
* There exists a separating automata of size $\|\Amp\|\cdot \|\Ap\|\cdot p$, for the winning objective $\Multmpp$, where $p$ is the maximum color in $\Cc$, $\Ap$ and $\Amp$ are $(n,\Parity)$ and $(n,\mp)$ separating automata, respectively.
* There exists a separating automata of size $\|\Amp\|^{k_1}\cdot (\|\Ap\|\cdot p)^{k_2}$ for the winning condition $\Infmp_1\vee\Infmp_2\vee\ldots\Infmp_{k_1}\vee\Parity_1\vee\Parity_2\vee\ldots\Parity_{k_2}$.
* There exists a separating automata of size $(\|\Ap\|\cdot p)^d$ for the winning objective $\Multkp$.

