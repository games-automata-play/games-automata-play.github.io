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
      mp: "{\\texttt{MP}}",
      Supmp: "{\\overline{\\texttt{MP}}}",
      Infmp: "{\\underline{\\texttt{MP}}}",
      Parity: "{\\texttt{P}}",
      Multlang: "{W_1\\vee W_2\\vee\\ldots\\vee W_k}",
      Multkmp: "{\\Infmp_1\\vee\\Infmp_2\\vee\\ldots\\vee\\Infmp_d}",
      Multkp: "{\\Parity_1\\vee\\Parity_2\\vee\\ldots\\vee\\Parity_d}",
      Multkw: "{W_1\\vee W_2\\vee\\ldots\\vee W_k}",
      bo: "{\\mathcal{O}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e solve 2-player games on graphs with disjunctive combinations of parity and mean payoff objectives(both, with $\limsup$ and $\liminf$), which we denote as $W_1\vee W_2\vee\ldots\vee W_d$, where $W_is$ are Parity or Mean payoff. </p>

### Notations
Given a game $\Game=(\A, d, c, W)$, where $\A$ is a 2-player(Adam and Eve) arena, d is the dimention of the game, c is the coloring function which assigns to each edge a d dimentional vector of a finite set of colors $\Cc$ and $W$ is the winning condition, i.e. $W\subseteq \Cc^d$, which Eve has to ensure. The question we want to ask is the following:
<center>Given a vertex $v_0$, can Eve ensure the winning condition $W$ from $v_0$?</center>
By $\Parity, \Supmp$ and $\Infmp$, we denote Parity, Mean payoff with $\limsup$ and Mean payoff with $\liminf$, respectively. A run $r$ satisfies $W_1\vee W_2\vee\ldots\vee W_d$ if $\exists 1\leq i\leq d$, such that $\pi_i(r)$ satisfies $W_i$, where $\pi_i$ is the projection funtion in the $i^{th}$ dimension.

We use the [universal graphs/separation]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}) approach to solve to problem.

> **Theorem:**
* There exists a universal graph of size $dn\log n$, for the winning objective $\Multkmp$.
* There exists a separating automata of size $\|\Amp\|\cdot \|\Ap\|\cdot p$, for the winning objective $\Multmpp$, where $p$ is the maximum color in $\Cc$, $\Ap$ and $\Amp$ are $(n,\Parity)$ and $(n,\mp)$ separating automata, respectively.
* There exists a separating automata of size $\|\Amp\|^{k_1}\cdot (\|\Ap\|\cdot p)^{k_2}$ for the winning condition $\Infmp_1\vee\Infmp_2\vee\ldots\Infmp_{k_1}\vee\Parity_1\vee\Parity_2\vee\ldots\Parity_{k_2}$.
* There exists a separating automata of size $(\|\Ap\|\cdot p)^d$ for the winning objective $\Multkp$.

#### Universal graph for $\Multkmp$
For this case, we use the following lemmas.
> **Lemma:**
For any winning objective $W$ , if we have a $(n, W )−$universal graphs $UG_{SCC}(n)$ for stongly connected graphs of size $n$, we can construct a $(n, W )−$universal graph $UG_{gen} (n)$ for general graphs.

The idea is to construct the universal graph for the general graphs recursively, using the property that any run from the graph will eventually end up in one of the strongly connected components.

> **Lemma:**
Given a strongly connected graph $G$, $G\models \Multkmp\implies G\models \mp_i$ for some $i$.

If for no component does the graph satisfy $\Infmp$, we can find a sequence of cycles in the graph, on iterating which we can get a path in $G$ which does not satisfy $\Infmp$.

$UG_{SCC}(n)$ consists of $k$ disjoint copies, $G_1 , G_2 ,\cdots G_d$ , of $(n, \mp)−$universal graph $UG_{\mp}$. In the $i^{th}$ copy, replace the labelling $a \in C$ on each edge by all the vectors in $C_1 \times C_2 \times C_{i−1} \times {a} \times C_{i+1} \cdots C_k.$ And now using the previous lemma we construct the universal graph for $\Multkmp$ for general graphs.

Note that this construction does not work when we have Mean Payoff objectives with $\limsup$, because the second lemma is not valid in this case.

#### Separating automata for $\Multmpp$
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
