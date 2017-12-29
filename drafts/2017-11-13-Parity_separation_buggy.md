---
layout:     post
title:      Separation for parity games
date:       2017-11-10 9:00:00
author:     Nathana&euml;l Fijalkow
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      enc: "{\\text{enc}}",
      deltasucc: "{\\delta_{\\text{succ}}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>he postulate is that three algorithms for parity games, 
namely the small progress measure algorithm of Jurdzi&#324;ski, the succinct progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;,
and the power counting algorithm of Calude et al, can be seen as solutions of a separation problem.</p>

Let $V$ be a set of $n$ vertices and $d$ a number of priorities (without loss of generality $d$ is even), we consider a priority function $V \to \set{1,\ldots,d} = [1,d]$.
A cycle is a word $v \cdots v$. It is even is the maximal priority is even, and odd otherwise. 
We define three languages:
* $\text{Parity} \subseteq V^\omega$: the maximal priority appearing infinitely many times is even,
* $\text{AllEvenCycles} \subseteq V^\omega$: all cycles are even,
* $\text{AllOddCycles} \subseteq V^\omega$: all cycles are odd.

A language $L$ over finite words is **(topologically) closed** if for all words $u$ not in $L$, for all words $v$, we have that $uv$ is not in $L$.
They are sometimes called *safe*, because they are recognised by *safe* automata: all states are accepting (with possibly undefined transitions).
A closed language $L$ over finite words induces a language over infinite words, also written $L$, which is the set of infinite words such that all prefixes belong to $L$.
Since $\text{AllEvenCycles}$ and $\text{AllOddCycles}$ are both closed, we will see them either as languages over finite words or infinite words.
 
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

### <a name="automaton">Constructing a totally ordered deterministic safe automaton</a>
We can be a bit more precise: the three techniques can be seen as constructing a deterministic safe automaton with the following properties.
The automaton is $$(Q,q_0,\delta : Q \times [1,d] \to Q)$$.
(Note that the transitions only depend on the priorities, not on the vertices themselves.) 
The set $Q$ is equipped with a total order $\le$.

The transition function is as follows: from the state $q$, reading a priority $p$, the next state $q'$ is the largest satisfying: 
* if $p$ is even, then $q \ge_p q'$,
* if $p$ is odd, then $q >\_p q'$.

We define the following properties.
1. $\le_1\ \subseteq\ \le_2\ \subseteq\ \le_3\ \cdots$,
2. for $p$ even we have $\delta(q,p) \ge q_0$,
3. $\delta$ is monotonic, *i.e.* if $q \le q'$ then $\delta(q,p) \le \delta(q',p)$,
4. for $q \in Q$, if $p,p'$ are odd priorities and $p <\ p'$, then $\delta(q,p') \le \delta(q,p)$,
5. the automaton accepts $(d-1)[n]$, where $(d-1)[n]$ is a sequence of length $n$ of the maximal odd priority $d-1$,
6. if $w$ is a finite word whose first letter is the largest priority in $w$ and is odd, then $q > \delta(q,w)$.

We used the following notations:
* the initial state and the transition function induce $\delta : [1,d]^* \to Q$,
* for a word $w$ and a state $q$, we let $\delta(q,w)$ denote the state reached after reading $w$ from $q$, if defined.

Let $L$ be the language recognised by such an automaton.

**Lemma:**
* Property 1 to 5 imply that $\text{AllEvenCycles} \subseteq L$.
* Property 6 implies that $L \cap \text{AllOddCycles} = \emptyset$, and even that $L \subseteq \text{Parity}$.

**Proof:**
To obtain the first item we show by induction that for all priorities $p$ and $w \in \text{AllEvenCycles}$:
* if $p$ is even, $\delta(w)$ is defined and $$\delta(w) \ge \delta((p-1)^{n-1})$$. 
* if $p$ is odd, $\delta(w)$ is defined and $$\delta(w) \ge \delta(p^{n-1})$$. 

We distinguish two cases:
* If the maximal priority $p$ is even, then $w$ can be factorised into $w = w_1 v_1 w_2 v_2 \cdots v_{k-1} w_k$ such that the $v_i$'s have priority $p$ 
and the $w_i$'s use priorities less than $p$.
The $w_i$'s are in $\text{AllEvenCycles}$. By induction hypothesis $\delta(w_1)$ is defined and $\delta(w_1) \ge \delta((p-1)^{n-1}$.
Thanks to property 1 we have $\delta(w_1v_1) = q_0$, so we can iterate this reasoning, and obtain that $\delta(q,w)$ is defined $\delta(w) = \delta(w_k) \le \delta((p-1)^{n-1})$.
* If the maximal priority $p$ is odd, then $w$ can be factorised into $w = w_1 v_1 w_2 v_2 \cdots$ such that the $v_i$'s have priority $d$ 
and the $w_i$'s use priorities less than $d$. In this case the vertices in $w_i$'s are pairwise disjoint, and the $v_i$'s do not repeat.
The $w_i$'s are in $\text{AllEvenCycles}$.
By induction hypothesis $\delta(q,w_1)$ is defined and $\delta(q,w_1) \ge \delta(q_0_{p-1},(p-2)^{n_1})$, with $n_1$ the number of different vertices in $w_1$.
Thanks to property 2 we have $\delta(q,w_i) \ge \delta(q,d^{n_i})$.
The sum of the number of $v_i$'s and the cumulated number of vertices in the $w_i$'s is at most $n$, so $\delta(w) \le \delta(d^n)$.

Conclude :!!!!!!

We show the second item. Let $w$ be an infinite word not satisfying the parity condition: the largest priority appearing infinitely many times is odd, 
inducing infinitely many consecutive words which start by an odd largest priority, 
which results in a decreasing chain of states thanks to property 4, hence the automaton eventually rejects.

### <a name="small_progress">The small progress measure algorithm</a>
The fact that the small progress measures form a solution of the separation problem was essentially shown by Julien Bernet, David Janin, and Igor Walukiewicz, in the paper 
[Permissive strategies: from parity games to safety games](www.labri.fr/perso/igw/Papers/igw-perm.ps).
By essentially I mean that the separation problem is hinted at in the conclusion, but the definitions of $\text{AllEvenCycles}$ and $\text{AllOddCycles}$ do not appear.
The result we give here is not a direct consequence of theirs, and the proof is also different.

We construct a deterministic safe automaton recognising a language $L$ solving the separation problem.
The states of the automaton are $d/2$-tuples of integers in $[0,n] = \set{0,\ldots,n}$,
they are numbered $1,3,5,\ldots,d-1$.
For a tuple $q$ and a priority $p$, we let $q(p)$ denote the $p$ component of $q$.
We define the $p$-orders: for a priority $p$, we let $$\le_p$$ denote the lexicographic order restricted to the components larger than or equal to $p$,
and $<\_p$ the strict version.
The initial state is the maximal element for $\le = \le_d$, *i.e.* the tuple containing only $n$, written $q_0$.

<!--
To define the transition function, we define the $p$-orders: for a priority $p$, we let $$\le_p$$ denote the lexicographic order restricted to the components larger than or equal to $p$,
and $<\_p$ the strict version.
The transition function is as follows: from the state $q$, reading a priority $p$, the next state $q'$ is the largest satisfying: 
* if $p$ is even, then $q \ge_p q'$,
* if $p$ is odd, then $q >\_p q'$.
-->

A mechanistic point of view for the transition function can be derived from the generic definition: from the state $q$, reading a priority $p$, the next state $q'$ is such that: 
* if $p$ is even, $q'$ is the same as $q$, but all components smaller than $p$ are reset to $n$,
* if $p$ is odd, $q'$ is the same as $q$, but the $p$ component is decremented by $1$ and all priorities smaller than $p$ are reset to $n$.
If the $p$ component of $q$ is $0$, then the component just above is decremented, and so on. If no component can be decremented, the automaton rejects.

**Lemma:**
The small progress measure automaton satisfies the properties 1 to 4 defined [above](#automaton).

Each property is easily checked.

**Remark.**
We give another simple and instructive proof that $\text{AllEvenCycles} \subseteq L$, which is also different than the technical developments of 
[the original paper](www.labri.fr/perso/igw/Papers/igw-perm.ps).
This follows from the following invariant: for every $w \in V^*$ which is not rejected by the automaton, $w$ has a suffix of the form $w_{d-1} \cdots w_3 w_1$, where 
for $p \in \set{1,3,\ldots,d-1}$, the priority $p$ appears at least $\delta(w)(p)$ times, and no larger priority appears in $w_p$.

Assume the invariant holds for $w$, and we read a vertex $v$ of priority $p$, there are two cases:
* if $p$ is even, we construct the suffix $w_{d-1} \cdots (w_{p+1} v)$ of $w v$, *i.e.* the new word for $p+1$ is $w_{p+1} v$.
* if $p$ is odd, we construct the suffix $w_{d-1} \cdots (w_p v)$ of $w v$, *i.e.* the new word for $p$ is $w_{p+1} v$. 
The priority $p$ appears at least $\delta(wv)(p) = \delta(w)(p) + 1$ times.

This completes the proof of the invariant. Now, we argue that if a word is rejected by the automaton, then it contains an odd cycle,
which is the contrapositive of $\text{AllEvenCycles} \subseteq L$. 
Let $wv$ such that $w$ is accepted and $wv$ is rejected.
Assume that $v$ has priority $p$, this implies that $\delta(w)(p) = n$. It follows that in $w_p w_{p-1} \cdots w_1 v$ the priority $p$ appears at least $n+1$ times,
and no larger priority appears. Since there are $n$ vertices, some vertex repeats twice, forming an odd cycle.






### <a name="succinct_progress">The succinct progress measure algorithm</a>
The fact that the succinct progress measures form a solution of the separation problem was not known before this post, to the best of the author's knowledge.

We construct a deterministic safe automaton recognising a language $L$ solving the separation problem.
The set states of the automaton is $S_{n,d}$, the set of $\frac{d}{2}$-tuples of binary strings whose total length is at most $\lceil \log(n) \rceil$.
The components are numbered $d-1,d-3,\ldots,3,1$.
For a state $q$ and a priority $p$, we let $q(p)$ denote the $p$ component of $q$.

Fix an (arbitrary) total order on binary strings; in the [original paper](https://arxiv.org/abs/1702.05051) the choice is somehow imposed
by the succinct encoding lemma, here it does not matter.
Without loss of generality, let us assume that $\varepsilon$ is the maximal element.
We extend this order into a lexicographic order for states.
We also let $$\le_p$$ denote the lexicographic order restricted to the components larger than or equal to $p$,
and $<\_p$ the strict version. The order $\le_p$ is called the $p$-order.

The initial state is the tuple containing only $\varepsilon$, written $q_0$.

The transition function is defined exactly as for small progress measures: from the state $q$, reading a priority $p$, the next state $q'$
is the smallest satisfying: 
* if $p$ is even, then $q \ge_p q'$,
* if $p$ is odd, then $q >\_p q'$.

A more mechanistic point of view can be derived from this definition. Because of the condition that the total number of bits is $\lceil \log(n) \rceil$
and of the choice of the total order on binary strings, this is a bit tedious, see the [original paper](https://arxiv.org/abs/1702.05051).

<!--
Formally, the transition function is $$\deltasucc : S_{n,d} \times V \to S_{n,d}$$, inducing $$\deltasucc : V^* \to S_{n,d}$$.

The automaton is safe: all states are accepting, and if a transition is undefined the automaton rejects.

For a word $w$ and a tuple $x$, we let $\deltasucc(x,w)$ denote the tuple reached after reading $w$ from $x$, if defined.
Then, $\deltasucc(w)$ is $\deltasucc(x_0,w)$.
-->

**Lemma:**
The small progress measure automaton satisfies the properties 1 to 4 defined [above](#automaton).

Each property is easily checked.










### <a name="power_counting">The power counting algorithm</a>
The fact that the power counting technique gives rise to a solution of the separation problem has been worked out by Miko&#322;aj Boja&#324;czyk and Wojtek Czerwi&#324;ski in their lecture notes
[An automata toolbox](https://www.mimuw.edu.pl/~bojan/20172018-2/advanced-topics-in-automata-20172018-jezyki-automaty-i-obliczenia-2).
They make the assumption that $n = d$. 
We explain how to adapt the construction of the automaton, and show that it satisfies the properties 1 to 4  defined [above](#automaton), giving an alternative proof.
The automaton is sligthly simplified, as we do not use undefined components.

We construct a deterministic safe automaton recognising a language $L$ solving the separation problem.
Let $k$ such that $2^k > n$, *i.e.* $k = \lfloor \log(n) \rfloor + 1$. 
The states of the automaton are non-increasing $k$-tuples whose values are priorities.
Here by non-increasing we mean for the natural order.
The components of a $k$-tuple are numbered $k-1,\ldots,1,0$.
Note that there are $d^k = O(d^{\log(n)}) = O(n^{\log(d)})$ states. 
For a state $q$ and a component $i$, we let $q(i)$ denote the component $i$ in $q$.

We use the order $d \succ d-2 \succ \cdots \succ 2 \succ 1 \succ 3 \succ \cdots \succ d-1$,
which is extended to a lexicographic order on states.

The initial state is the tuple containing only $d$, written $q_0$.
The transition function is as follows: from the state $q$, reading a priority $p$:
* if $p$ is even, let $i$ be the largest component that stores a value $<\ p$, the new state is obtaining by inserting $p$ in $i$ and resetting all smaller registers to $d$
(if there is no such $i$ then do nothing),
* if $p$ is odd, let $i$ be the largest component that stores a value $<\ p$ and $j$ be the smallest component that stores an even number or is empty, then
	* if either $i$ or $j$ is defined, the new state is obtained by inserting $p$ in $\max(i,j)$ and resetting all smaller registers to $d$,
	* if neither $i$ nor $j$ are defined, then reject.

**Lemma:**
The power counting automaton satisfies the properties 1 to 4 defined [above](#automaton).

Each property is easily checked. Properties 2 and 4 require rolling up the sleeves but there is no technical difficulty.
