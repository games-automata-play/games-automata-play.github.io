---
layout:     post
title:      Strong and weak separating automata 
date:       2019-03-25 9:00:00
author:     Nathana&euml;l Fijalkow
category:   
- Parity games
- research
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

<p class="intro"><span class="dropcap">T</span>his post discusses the difference between strong and weak separating automata.</p>

The goal of this post is to discuss the notion of separating automata, which already appear in many previous posts, see 
[this post]({{ '/blog/parity_separation' | prepend: site.baseurl }}) and [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}) for parity games,
[this post]({{ '/blog/mean_payoff' | prepend: site.baseurl }}) for mean payoff games,
and [this post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}) for general games.

We consider graphs and games whose edges are labelled by $C$, a set of labels.
The size of a graph is its number of vertices.
We fix $$W \subseteq C^\omega$$ a winning condition, and we make one assumption, called positionality of $W$:

> **Assumption:** In any finite game, if Eve has a strategy ensuring $W$, then she has a positional strategy ensuring $W$,
and the same holds for Adam.

We fix $n$ a parameter.

### Separating automata

The automata we consider are deterministic safety automata over infinite words on the alphabet $C$, 
where safety means that all states are accepting: a word is rejected if there exist no run for it.
The size of an automaton is its number of states.

In the following we say that a path in a graph is accepted or rejected by an automaton; this is an abuse of language
since what the automaton reads is only the labels of the corresponding path.
We say that a graph satisfies $W$ if all paths in the graph satisfy $W$.

> **Definition:** An automaton is **$$n$$-weak-separating** if the two following properties hold.
* For all graphs of size $n$ satisfying $W$, the automaton accepts all paths in the graph
* All words accepted by the automaton are in $W$

The weak definition is asymmetric; it only relies on the fact that Eve has positional strategies.
In [this post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}) it is explained how this yields a reduction to safety games.

> **Definition:** An automaton is **$$n$$-strong-separating** if the two following properties hold.
* For all graphs of size $n$ satisfying $W$, the automaton accepts all paths in the graph
* For all graphs of size $n$ satisfying the complement of $W$, the automaton rejects all paths in the graph

Clearly, if an automaton is $n$-weak-separating, then it is $n$-strong-separating.
Hence a priori, strong-separating automata may be smaller than weak-separating ones.

The strong definition restores symmetry. The same idea of reduction to safety games works for this definition, this time relying on both players having memoryless strategies. 

> **Lemma:**
Let $L$ be the language recognised by a $n$-strong-separating automaton $$\A$$.
Then for all games $G$ of size $n$, we have
that Eve has a strategy ensuring $W$ if and only she has a strategy ensuring $L$.

**Proof:**
Let us first assume that Eve has a strategy $\sigma$ ensuring $W$.
It can be chosen positional thanks to our assumption on $W$.
Then $G[\sigma]$, the graph obtained by restricting the game $G$ to the moves prescribed by $\sigma$, is a graph of size $n$ satisfying $W$, so $$\A$$ accepts all paths in $G[\sigma]$.
In other words, all paths consistent with $\sigma$ are in $L$, or equivalently $\sigma$ ensures $L$.

Conversely, assume that Adam has a strategy $\tau$ ensuring the complement of $W$.
It can be chosen positional thanks to our assumption on $W$.
Then $G[\tau]$, the graph obtained by restricting the game $G$ to the moves prescribed by $\tau$, is a graph of size $n$ satisfying the complement of $W$, 
so $$\A$$ rejects all paths in $G[\tau]$.
In other words, all paths consistent with $\tau$ are not in $L$, or equivalently $\tau$ ensures the complement of $L$.

### Why is this a good definition?

The definition may seem a bit arbitrary. In particular, in the early results on separating automata, there were different definitions.
Boja&#324;czyk and Czerwi&#324;ski talk about separating automata in a different way (in the context of parity games): 
they say that the automaton must accept all words which contain only even cycles.

The point of our definition is that it is minimal, in the following sense.

> **Lemma:**
Let $\A$ be an automaton over the alphabet $C$ recognising a language $L$, then the following are equivalent 
* for all games $G$ of size $n$, Eve has a strategy ensuring $W$ if and only she has a strategy ensuring $L$
* $\A$ is $n$-strongly separating

### Strong VS weak

Why did we focus on the weak notion in all the previous posts? Well, simply because all the constructions we have yield weak-separating automata.
Weak-separating automata are also strong-separating. Strong-separating automata may be smaller than weak-separating ones, but this never materialised: 
we do not know of any (non-trivial) example of a strong-separating automaton which is not a weak-separating automaton.

What can be said about strong-separating automata? So far, not much. The correspondence with universal graph does not hold anymore, since universal graphs are indeed very asymmetric,
and so far we could not find the right symmetric definition to match strong-separating automata.
[This paper](https://arxiv.org/abs/1902.07175) proves some lower bounds, but not for the full class of strong-separating automata.



