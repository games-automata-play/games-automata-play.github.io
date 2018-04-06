---
layout:     post
title:      Resolution algorithm for parity games
date:       2018-03-15 9:00:00
author:     Nathana&euml;l Fijalkow and Filip Mazowiecki
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      enc: "{\\text{enc}}",
      deltasucc: "{\\delta_{\\text{succ}}}",
      last: "{\\text{last}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e reduce parity games to satisfiability of an SMT formula. 

postulate is that three algorithms for parity games, 
namely the small progress measure algorithm of Jurdzi&#324;ski, the succinct progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;,
and the power counting algorithm of Calude et al, can be seen as solutions of a separation problem.</p>

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

#### In the remainder of this post we explain that:
1. the [small progress measure](#small_progress) of Jurdzi&#324;ski is a solution of the separation problem with $\|L\| = O(n^{\frac{d}{2}})$,
2. the [succinct progress measure](#succinct_progress) of Jurdzi&#324;ski and Lazi&#263; is a solution of the separation problem with $\|L\| = O(n^{\log(d)})$,
3. the [power counting](#power_counting) of Calude et al is a solution of the separation problem with $\|L\| = O(n^{\log(d)})$.



### <a name="small_progress">The small progress measure algorithm</a>
The fact that the small progress measures form a solution of the separation problem was essentially shown by Julien Bernet, David Janin, and Igor Walukiewicz, in the paper 
[Permissive strategies: from parity games to safety games](www.labri.fr/perso/igw/Papers/igw-perm.ps).
By essentially I mean that the separation problem is hinted at in the conclusion, but the definitions of $\text{AllEvenCycles}$ and $\text{AllOddCycles}$ do not appear.

The proof we give here is not a direct adaptation of their proof. It is not fair to say that it is simpler, nor better. It is simply *different*.

We construct a (deterministic) safe automaton recognising a language $L$ solving the separation problem.
The states of the automaton are $d/2$-tuples of integers in $[n] = \set{0,\ldots,n}$,
they are numbered $1,3,\ldots,d-1$.
For a tuple $x$ and a priority $p$, we let $x(p)$ denote the $p$ component of $x$.
The initial state is the tuple containing only $0$, written $x_0$.
We order tuples lexicographically; more precisely, we let $$\le_p$$ denote the lexicographic order restricted to the components larger than or equal to $p$,
and $<\_p$ the strict version. The order $\le_p$ is called the $p$-order.
The transition function is as follows: from the tuple $x$, reading a vertex $v$ of priority $p$, the next tuple $x'$
is the smallest satisfying: 
* if $p$ is even, then $x \le_p x'$,
* if $p$ is odd, then $x <\_p x'$.

A more mechanistic definition is as follows: 
* if $p$ is even, then the new tuple is the same as $x$, but all priorities smaller than $p$ are reset to $0$,
* if $p$ is odd, then the new tuple is the same as $x$, but the $p$ component is incremented by $1$ and all priorities smaller than $p$ are reset to $0$.
If a component cannot be incremented, it is reset, and the component just above is incremented. If no components can be incremented, the automaton rejects.

Formally, the transition function is $$\delta : [n]^{\frac{d}{2}} \times V \to [n]^{\frac{d}{2}}$$, inducing $$\delta : V^* \to [n]^{\frac{d}{2}}$$.
For a word $w$ and a tuple $x$, we let $\delta(x,w)$ denote the tuple reached after reading $w$ from $x$, if defined.
Then, $\delta(w)$ is $\delta(x_0,w)$.

The automaton is safe: all states are accepting, and if a transition is undefined, the automaton rejects.

**Lemma:**
1. $\text{AllEvenCycles} \subseteq L$
2. $L \cap \text{AllOddCycles} = \emptyset$

**Proof:**
We start by proving the second item: it follows from the well known observation that if $w$ is an odd cycle, then $x <\_p \delta(x,w)$.
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

For all games with $n$ vertices and $d$ priorities, 
Eve wins, if, and only if, 
there exists a progress measure, which is a (partial) function $\mu : V \to [n]^{\frac{d}{2}}$ such that for all $v$ of priority $p$:
* if $v$ belongs to Eve, there exists $(v,v') \in E$ such that 
$$\mu(v) \le_p \mu(v'),$$
* if $v$ belongs to Adam, for all $(v,v') \in E$ we have 
$$\mu(v) \le_p \mu(v').$$

We see this as a **global** property, as opposed to the **local** (and dynamic) property $\text{AllEvenCycles} \subseteq L$.
We do not know of any non-convoluted way to prove one property using the other.







### <a name="succinct_progress">The succinct progress measure algorithm</a>
The fact that the succinct progress measures form a solution of the separation problem was not explicitely stated before this post, to the best of the author's knowledge.

Before constructing the automaton, we give some intuitions.
The small progress measure algorithm manipulates functions $\mu : V \to [n]^{\frac{d}{2}}$, 
which can be seen as trees of depth $\frac{d}{2}$ and branching directions $[n]$, where the leaves are labeled by vertices.
The *key* observation of Jurdzi&#324;ski and Lazi&#263; is that these trees are *only* used for lexicographic comparisons.
Hence we can encode them in a more succinct way supporting *only* this feature.

Let $k = \lceil \log(n) \rceil$ and $S_{n,d}$ the set of $\frac{d}{2}$-tuples of binary strings whose total length is at most $k$.
The components are numbered $1,3,\ldots,d-1$.
For a tuple $x$ and a priority $p$, we let $x(p)$ denote the $p$ component of $x$.
We order binary strings as follows: 
$$ 
0 s < \varepsilon \qquad ; \qquad \varepsilon <\ 1 s \qquad ; \qquad b s <\ b s' \Longleftrightarrow s <\ s'.
$$

We extend this order into a lexicographical order for tuples; more precisely we let $$\le_p$$ denote the lexicographic order restricted to the components larger than or equal to $p$,
and $<\_p$ the strict version. The order $\le_p$ is called the $p$-order.

The following lemma is the key technical tool of the [original paper](https://arxiv.org/abs/1702.05051).

**Lemma (succinct tree coding).**
There exists a function $\enc$ taking as input a function $\mu : V \to [n]^{\frac{d}{2}}$
and outputting a function $\enc(\mu) : V \to S_{n,d}$ such that all $p$-orders are preserved:
for $\mu$, $v, v'$ and a priority $p$, we have
$$\mu(v) \le_p \mu(v')$$ if, anf only if, $$\enc(\mu)(v) \le_p \enc(\mu)(v').$$

**Proof:**
This is the [original proof](https://arxiv.org/abs/1702.05051) by induction on $n$ and $d$. 
In this proof it is more convenient to talk about trees than functions.

Let $\mu$ be a tree with $n$ leaves and depth $d$.
Choose the smallest branching direction $i \in [n]$ such that:
* the set $V_{<}$ of vertices $v$ such that $\mu(v)(d-1) <\ i$ has size at most $\frac{n}{2}$, and
* the set $V_{>}$ of vertices $v$ such that $\mu(v)(d-1) >\ i$ has size at most $\frac{n}{2}$.

Let $V_{=}$ the set of vertices such that $\mu(v)(d-1) = i$.
We consider three subtrees:
* the subtree with leaves $V_{<}$,
* the subtree with leaves $V_{>}$,
* the subtree with leaves $V_{=}$ rooted in the node $i$.

By induction hypothesis, we obtain encodings for these tree subtrees.
The encoding for the original tree is obtained by prefixing the encoding for $V_{<}$ by $0$ and the encoding for $V_{>}$ by $1$, 
encoding $i$ by $\varepsilon$ and keeping the encoding for $V_{=}$ unchanged.
We say that $t$ is (succinctly) encoded by cutting along direction $i$.


The extra technical bit in this post is the following lemma, showing the interplay between succinct tree coding and updating.
Recall that $$\delta : [n]^{\frac{d}{2}} \times V \to [n]^{\frac{d}{2}}.$$

We define $\delta^* : (V \to [n]^{\frac{d}{2}}) \times V \to (V \to [n]^{\frac{d}{2}})$ as follows:
$\delta(\mu,v)(v') = \mu(v')$ if $v' \neq v$, and $\delta(\mu(v),v')$ otherwise.
This induces a function $\delta^* : V^* \to (V \to [n]^{\frac{d}{2}})$.

**Lemma (succinct tree coding and update).**
For $\mu$, $v,v'$, with $v$ of priority $p$,
We have $$\enc(\mu)(v') \le_p \enc(\delta(\mu,v))(v').$$

<!--
* if $p$ is even, then $$\enc(\mu)(v') \le_p \enc(\delta(\mu,v))(v'),$$
* if $p$ is odd, then $\enc(\mu)(v') <\_{p}  \enc(\delta(\mu,v))(v').$
-->

**Proof:**
We succinctly encode in parallel $\mu$ and $\delta(\mu,v)$, and reason by induction on the depth, starting from $d-1$ down to $p$.

We first observe that if $\mu$ is cut along direction $i$ and $\delta(\mu,v)$ along direction $j$, then $i \le j$,
since the only difference between $\mu$ and $\delta(\mu,v)$ is that $v$ has been pushed to the right in $\delta(\mu,v)$.
Now there are two cases:
* either $v'$ belongs to the same subtrees in both encodings, *i.e.* for instance in the subtree with leaves $V_{<}$ in both cases, then we continue by induction in this subtree.
* or $v'$ belongs to $V_{<}$ for $\mu$ and $V_{=}$ or $V_{>}$ for $\delta(\mu,v)$ (similar case: $v'$ belongs to $V_{=}$ for $\mu$ and $V_{>}$ for $\delta(\mu,v)$), 
which implies that $\enc(\mu)(v') <\_{p}  \enc(\delta(\mu,v))(v').$

If we go past depth $p$, then $\enc(\mu)(v') =\_{p}  \enc(\delta(\mu,v))(v').$


We construct a (deterministic) safe automaton recognising a language $L$ solving the separation problem.
The set states of the automaton is $S_{n,d}$.
The initial state is the tuple containing only $\varepsilon$, written $x_0$.
The transition function is defined exactly as for small progress measures: from the tuple $x$, reading a vertex $v$ of priority $p$, the next tuple $x'$
is the smallest satisfying: 
* if $p$ is even, then $x \le_p x'$,
* if $p$ is odd, then $x <\_p x'$.

A more mechanistic point of view can be derived from this definition. Because of the condition that the total number of bits is $\log(n)$,
this is a bit tedious, see the [original paper](https://arxiv.org/abs/1702.05051).

Formally, the transition function is $$\deltasucc : S_{n,d} \times V \to S_{n,d}$$, inducing $$\deltasucc : V^* \to S_{n,d}$$.

The automaton is safe: all states are accepting, and if a transition is undefined the automaton rejects.

For a word $w$ and a tuple $x$, we let $\deltasucc(x,w)$ denote the tuple reached after reading $w$ from $x$, if defined.
Then, $\deltasucc(w)$ is $\deltasucc(x_0,w)$.

**Lemma:**
1. $\text{AllEvenCycles} \subseteq L$
2. $L \cap \text{AllOddCycles} = \emptyset$

**Proof:**
We start by proving the second item: the proof is exactly as in the case of small progress measures, 
it follows from the observation that if $w$ is an odd cycle, then $x <\_p \deltasucc(x,w)$.
Consider now the largest priority appearing infinitely many times, and assume it is odd: there are infinitely many odd cycles with this priority, 
and from some point on they induce an increase in the corresponding tuple, which eventually results in the automaton rejecting.

We now prove the first item.
We claim that for a function $\mu : V \to [n]^{\frac{d}{2}}$ and two vertices $v,v'$, we have
$$\deltasucc(\enc(\mu)(v'),v) \le \enc(\delta^*(\mu,v))(v').$$

Indeed, let us say that $v$ has priority $p$.
We first assume that $p$ is even.
By definition $$\deltasucc(\enc(\mu)(v'),v)$$ is the smallest $x$ in $S_{n,d}$ such that
$$\enc(\mu)(v') \le_p x.$$
It follows that to prove the inequality above it is enough to show that
$$\enc(\mu)(v') \le_p \enc(\delta^*(\mu,v))(v'),$$
which is precisely the statement of the lemma above.

We now assume that $p$ is odd.
By definition $$\deltasucc(\enc(\mu)(v'),v)$$ is the smallest $x$ in $S_{n,d}$ such that
$$\enc(\mu)(v') <\_{p}\ x.$$
The lemma above states that
$$\enc(\mu)(v') \le_p \enc(\delta^*(\mu,v))(v').$$
AND????

This implies that $\deltasucc(w) \le \enc(\delta^*(w))(\last(w))$, where $\last(w)$ is the last vertex in $w$.
Hence if $w$ is accepted by the automaton using small progress measures, then it is accepted by the automaton using succinct progress measures,
which concludes the proof.










### <a name="power_counting">The power counting algorithm</a>
The last point has been worked out by Miko&#322;aj Boja&#324;czyk and Wojtek Czerwi&#324;ski in their lecture notes
[An automata toolbox](https://www.mimuw.edu.pl/~bojan/20172018-2/advanced-topics-in-automata-20172018-jezyki-automaty-i-obliczenia-2).
They make the assumption that $n = d$. We explain how to adapt the construction of the automaton. The proof applies *mutatis mutandis*.

We construct a (deterministic) safe automaton recognising a language $L$ solving the separation problem.
Let $k$ such that $2^k > n$, *i.e.* $k = \lceil \log(n) \rceil + 1$. 
The states of the automaton are $k$-tuples whose values are either priorities or undefined, written $\bot$.
The components of a $k$-tuple are numbered $0,1,\ldots,k-1$ and called registers. 
Note that there are $(d + 1)^k = O(d^{\log(n)}) = O(n^{\log(d)})$ states. 
For a tuple $x$ and a register $i$, we let $x(i)$ denote the content of the register $i$ in $x$.
The initial state is the tuple containing only $\bot$, written $x_0$.
The transition function is as follows: from the tuple $x$, reading a vertex $v$ of priority $p$:
* if $p$ is even, let $i$ be the largest nonempty register that stores a value $<\ p$, the new tuple is obtaining by inserting $p$ in $i$ and emptying all smaller registers
(if there is no such $i$ then do nothing),
* if $p$ is odd, let $i$ be the largest nonempty register that stores a value $<\ p$ and $j$ be the smallest register than is empty or stores an even number, then
	* if either $i$ or $j$ is defined, the new tuple is obtaining by inserting $p$ in $\max(i,j)$ and emptying all smaller registers,
	* if neither $i$ nor $j$ are defined, then reject.

