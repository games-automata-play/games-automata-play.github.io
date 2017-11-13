---
layout:     post
title:      Separation for parity games
date:       2017-11-10 9:00:00
author:     Nathana&euml;l Fijalkow
---

<p class="intro"><span class="dropcap">T</span>he postulate of this post is that two algorithms for parity games, 
namely the small progress measure algorithm of Jurdzi&#324;ski<!--, the succint progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;, -->
and the power counting algorithm of Calude et al, are solutions of a separation problem.</p>

Let $V$ be a set of $n$ vertices and $d$ a number of priorities (without loss of generality $d$ is even), we consider a priority function $V \to \set{1,\ldots,d}$.
A cycle is a word $v \cdots v$. It is even is the maximal priority is even, and odd otherwise. 
We define three languages:
* $\text{Parity} \subseteq V^\omega$: the maximal priority appearing infinitely many times is even,
* $\text{AllEvenCycles} \subseteq V^\omega$: all cycles are even,
* $\text{AllOddCycles} \subseteq V^\omega$: all cycles are odd.

A language $L$ over finite words is **(topologically) closed** if for all words $u$ not in $L$, for all words $v$, we have that $uv$ is not in $L$.
They are sometimes called *safe*, because they are recognised by *safe* automata: all states are accepting (with possibly undefined transitions).
A closed language $L$ over finite words induces a language over infinite words, also written $L$, which is the set of infinite words such that all prefixes belong to $L$.
 
The separation problem reads: find a closed regular language $L \subseteq V^*$ such that

> 1. $\text{AllEvenCycles} \subseteq L$
> 2. $L \cap \text{AllOddCycles} = \emptyset$

<figure>
	<img src="{{ '/images/separation2.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The separation problem</figcaption>
</figure>

> **Theorem:** The winning regions of $\text{Parity}$ and $L$ coincide.

**Proof:** This relies on the positional determinacy of parity games.
A positional strategy ensuring $\text{Parity}$ also ensures $\text{AllEvenCycles}$, hence $L$.
Conversely, a positional strategy ensuring the complement of $\text{Parity}$ also ensures $\text{AllOddCycles}$, hence the complement of $L$.

It follows that solving the parity game is equivalent to solving a safety game of size $n \times |L|$, where $|L|$
is the number of states of a **deterministic** automaton recognising $L$.
Since solving a safety done can be done in linear time, this gives an algorithm whose running time is polynomial in $n$ and $\|L\|$

#### In the remainder of this note we explain that:
1. the small progress measure algorithm of Jurdzi&#324;ski is a solution of the separation problem with $\|L\| = O(n^{\frac{d}{2}})$,
<!--2. the succint progress measure algorithm of Jurdzi&#324;ski and Lazi&#263; is a solution of the separation problem. -->
2. the power counting algorithm of Calude et al is a solution of the separation problem with $\|L\| = O(n^{\log(d)})$,




### The small progress measure algorithm 
The fact that the small progress measures form a solution of the separation problem is due to Julien Bernet, David Janin, and Igor Walukiewicz, in the paper 
[Permissive strategies: from parity games to safety games](www.labri.fr/perso/igw/Papers/igw-perm.ps).
They indeed hint at the separation problem in the conclusion.

We give here a different proof; it is not fair to say that it is simpler, nor better. It is simply *different*. Also it gives some intuitions for the next algorithm.

We construct a (deterministic) safe automaton recognising a language $L$ solving the separation problem.
The states of the automaton are $d/2$-tuples of integers in the range $0,\ldots,n$,
they are numbered $1,3,\ldots,d-1$.
For a tuple $x$ and a priority $p$, we let $x(p)$ denote the $p$ component of $x$.
The initial state is the tuple containing only $0$, written $x_0$.
The transition function is as follows: from the tuple $x$, reading a vertex $v$ of priority $p$:
* if $p$ is even, then the new tuple is the same as $x$, but all priorities smaller than $p$ are reset to $0$,
* if $p$ is odd, then the new tuple is the same as $x$, but the $p$ component is incremented by $1$ and all priorities smaller than $p$ are reset to $0$.

The automaton is safe: all states are accepting, and if a transition is undefined, *i.e.* a component with value $n$ is incremented, the automaton rejects.

**Note:**
The definitions are slightly different from the classical progress measures. 
In particular we do not define the lexicographic order,
and in case of an overflow, we do not increase the next component, but instead directly reject.
The same results hold for the classical progress measures, almost *mutatis mutandis*, but for our purposes here these definitions are more convenient.

For a word $w$ and a tuple $x$, we let $\delta(x,w)$ denote the tuple reached after reading $w$ from $x$, if defined.
Then, $\delta(w)$ is $\delta(x_0,w)$.

**Lemma:**
1. $\text{AllEvenCycles} \subseteq L$
2. $L \cap \text{AllOddCycles} = \emptyset$

**Proof:**
We start by proving the second item: it follows from the well known observation that if $w$ is an odd cycle with maximal priority $p$, then $x(p) < \delta(x,w)(p)$.
Indeed, along the cycle the $p$ component is either not touched, when a lower priority is visited, or incremented.
Consider now the largest priority appearing infinitely many times, and assume it is odd: there are infinitely many odd cycles with this priority, 
and from some point on they induce an increase in the corresponding tuple, which eventually results in the automaton rejecting.

We now prove the first item. 
It follows from the following invariant: for every $w \in V^*$ which is not rejected by the automaton, $w$ has a suffix of the form $w_{d-1} \cdots w_3 w_1$, where 
for $p \in \set{1,3,\ldots,d-1}$, the priority $p$ appears at least $\delta(w)(p)$ times, and no larger priority appears in $w_p$.

Assume the invariant holds for $w$, and we read a vertex $v$ of priority $p$, there are two cases:
* if $p$ is even, we construct the suffix $w_{d-1} \cdots (w_{p+1} v)$ of $w v$, *i.e.* the new word for $p+1$ is $w_{p+1} v$.
* if $p$ is odd, we construct the suffix $w_{d-1} \cdots (w_p v)$ of $w v$, *i.e.* the new word for $p$ is $w_{p+1} v$. 
The priority $p$ appears at least $\delta(wv)(p) = \delta(w)(p) + 1$ times.

This completes the proof of the invariant. Now, we argue that if a word is rejected by the automaton, then it contains an odd cycle,
which is the contrapositive of the first item. Let $wv$ such that $w$ is accepted and $wv$ is rejected.
Assume that $v$ has priority $p$, this implies that $\delta(w)(p) = n$. It follows that in $w_p w_{p-1} \cdots w_1 v$ the priority $p$ appears at least $n+1$ times,
and no larger priority appears. Since there are $n$ vertices, some vertex repeats twice, forming an odd cycle.


**Remark.**
The original proof of correctness of the small progress measure algorithm differs for the first item. It shows the following **global** property:
for all graphs with $n$ vertices and $d$ priorities, if all cycles are even then there exists a progress measure, which is a function $\mu : V \to \set{0,\ldots,n}^{\frac{d}{2}}$
satisfying some local proprties. More precisely, the local properties say that the progress measure (*somehow*) respects the transition function of the automaton defined above.
We see this as a **global** property, as opposed to the **local** (and dynamic) property $\text{AllEvenCycles} \subseteq L$.
We do not know of any non-convoluted way to prove one property using the other.

<!--
### The succint progress measure algorithm
The fact that the succinct progress measures form a solution of the separation problem was not known before this post, to the best of the author's knowledge.
It does not seem to follow directly from the [original paper](https://arxiv.org/abs/1702.05051).
We prove it by extending the proof above.

We construct a (deterministic) safe automaton recognising a language $L$ solving the separation problem.
The states of the automaton are $d/2$-tuples of binary words such that the total length is at most $\log(n)$,
they are numbered $1,3,\ldots,d-1$.
For a tuple $x$ and a priority $p$, we let $x(p)$ denote the $p$ component of $x$.
The initial state is the tuple containing only $\varepsilon$, written $x_0$.
We order binary strings as follows: 
$$ 
0 s < \varepsilon \qquad ; \qquad \varepsilon <\ 1 s \qquad ; \qquad b s <\ b s' \Longleftrightarrow s <\ s'.
$$

We extend this order into a lexicographical order for tuples, written $\le$ and $<$ for the strict version.
For a tuple $x$ and a priority $p$, we define $x_{| \ge p}$ the tuple restricted to the components larger than $p$.

The transition function is as follows: from the tuple $x$, reading a vertex $v$ of priority $p$:
* if $p$ is even, then the new tuple is the smallest $x'$ such that $x_{\| \ge p} \le x'_{\| \ge p}$,
* if $p$ is odd, then the new tuple is the smallest $x'$ such that $x_{\| \ge p} < x'_{\| \ge p}$.

The automaton is safe: all states are accepting, and if a transition is undefined the automaton rejects.

For a word $w$ and a tuple $x$, we let $\delta(x,w)$ denote the tuple reached after reading $w$ from $x$, if defined.
Then, $\delta(w)$ is $\delta(x_0,w)$.

**Lemma:**
1. $\text{AllEvenCycles} \subseteq L$
2. $L \cap \text{AllOddCycles} = \emptyset$

**Proof:**
We start by proving the second item: it follows from the observation that if $w$ is an odd cycle with maximal priority $p$, then $x_{| \ge p} < \delta(x,w)_{| \ge p}$.
This is stated as such in the [original paper](https://arxiv.org/abs/1702.05051).
Indeed, consider a cycle $w = v_1 v_2 \cdots v_k v_1$, and assume that $v_1$ has priority $p$, odd and maximal in the cycle.
Then we have
$$
x_{| \ge p} < \delta(x, v_1)_{| \ge p} \le \cdots \le \delta(x, v_1 \cdots v_k)_{| \ge p} \le \delta(x, w)_{| \ge p}.
$$

Consider now the largest priority appearing infinitely many times, and assume it is odd: there are infinitely many odd cycles with this priority, 
and from some point on they induce an increase in the corresponding tuple, which eventually results in the automaton rejecting.

We now prove the first item. 
It follows from the following invariant: for every $w \in V^*$ which is not rejected by the automaton, $w$ has a suffix of the form $w_{d-1} \cdots w_3 w_1$, where 
for $p \in \set{1,3,\ldots,d-1}$, the priority $p$ appears at least $V[\delta(w)(p)]$ times, and no larger priority appears in $w_p$.
The function $V$ is defined as follows: $V[\varepsilon] = 0$, then $V[s]$ is the value of $s$ in binary plus one. For instance, $V[10] = 2 + 1 = 3$.

Assume the invariant holds for $w$, and we read a vertex $v$ of priority $p$, there are two cases:
* if $p$ is even, we construct the suffix $w_{d-1} \cdots (w_{p+1} v)$ of $w v$, *i.e.* the new word for $p+1$ is $w_{p+1} v$.
* if $p$ is odd, we construct the suffix $w_{d-1} \cdots (w_p v)$ of $w v$, *i.e.* the new word for $p$ is $w_{p+1} v$. 
The priority $p$ appears at least $\delta(wv)(p) = \delta(w)(p) + 1$ times.

This completes the proof of the invariant. Now, we argue that if a word is rejected by the automaton, then it contains an odd cycle,
which is the contrapositive of the first item. Let $wv$ such that $w$ is accepted and $wv$ is rejected.
Assume that $v$ has priority $p$, this implies that $\delta(w)(p) = n$. It follows that in $w_p w_{p-1} \cdots w_1 v$ the priority $p$ appears at least $n+1$ times,
and no larger priority appears. Since there are $n$ vertices, some vertex repeats twice, forming an odd cycle.
-->


### The power counting algorithm 
The second point has been worked out by Miko&#322;aj Boja&#324;czyk and Wojtek Czerwi&#324;ski in their lecture notes
[An automata toolbox](https://www.mimuw.edu.pl/~bojan/20172018-2/advanced-topics-in-automata-20172018-jezyki-automaty-i-obliczenia-2).
Unfortunately, they make the assumption that $n = d$. 
We will explain soon how to adapt their proof.

### The succint progress measure algorithm
The fact that the succinct progress measures form a solution of the separation problem is not known, to the best of the author's knowledge.
It does not seem to follow directly from the [original paper](https://arxiv.org/abs/1702.05051).


