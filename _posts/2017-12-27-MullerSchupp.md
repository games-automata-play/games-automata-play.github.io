---
layout:     post
title:      The backward approach of Muller and Schupp for positional determinacy
date:       2017-12-27 9:00:00
author:     Nathana&euml;l Fijalkow
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      G: "{\\mathcal{G}}",
      VE: "{V_E}",
      Parity: "{\\mathrm{Parity}}",
      N: "{\\mathbb{N}}",
      nN: "{_{n \\in \\mathbb{N}}}",
      priority: "{\\{0,\\ldots,d\\}}",
      lex: "{\\le_{\\mathrm{lex}}}",
      lexstrict: "{<_{\\mathrm{lex}}}",
      tr: "{\\mathrm{tr}}",
      P: "{\\mathcal{P}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post revisits Muller and Schupp's backward approach to prove the positionality of parity games.
The argument is simple and beautiful; credits go to Thomas Colcombet for this presentation.</p>

<p>
We discussed the technical details of the construction informally together, and Thomas gave me a draft he wrote. 
Unhappy that I didn't write the draft myself, I wrote down my own version of the construction.
It turned out to be technically [identical](https://en.wikipedia.org/wiki/Pierre_Menard,_Author_of_the_Quixote). 
This post features additional explanations and unhelpful drawings.</p>

<p>
The crux in a few sentences: assume we have a winning strategy for parity, we want to construct a positional one. 
The difficulty is that the strategy we start from may perform stupid moves at various times during the game. 
We define a preference order on plays, which induces the notion of the worst possible history when reaching a vertex. 
The positional strategy plays assuming this worst case; if the preference order is well chosen, this forces the strategy to play good moves,
ensuring the parity condition.
</p>

We fix $\G$ a game with finite outdegree, $v_0$ a starting vertex, $\Omega : V \to \priority$ a priority function,
and $\sigma$ a winning strategy for Eve, i.e. ensuring $\Parity(\Omega)$.

#### Ordering histories

A history is a finite or infinite word over $\priority$,
we let 
$$\priority^\infty = \priority^* \cup \priority^\omega$$ 
be the set of histories.
It is equipped with the prefix distance, inducing a notion of limit for sequences of histories.

The parity condition is satisfied by infinite histories such that the *maximal* priority appearing infinitely
often is even.

For a history $p$, define $\tr_c(p)$ the suffix of $p$ that starts after the last occurrence of $c$.
The infix from position $n$ to position $m$ is written $p[n,\ldots,m]$.
For a history $p$, define $\xi_c(p)$ the set of positions for which $p$ has value $c$.

We will consider sets of natural numbers, ordered by $\lex$ defined as follows:
for two sets $X,Y \subseteq \N$, 
we say that $X \lexstrict Y$ if there exists $n \in \N$ such that
$n \in Y \setminus X$ and $X \cap \set{0,\ldots,n-1} = Y \cap \set{0,\ldots,n-1}$.

We define the order $\preceq$ over for finite histories;
intuitively speaking, $p \preceq q$ reads: $p$ is worse than $q$ with respect to $\Parity(\Omega)$.
We first define $\preceq_d$ by induction on the maximal priority $d$:
* If $d = 0$, then $p \preceq_d\ q$ if $\|p\| \le \|q\|$.
* If $d$ is odd, then $p \preceq_d\ q$ if 
$$\left\{
\begin{array}{ll}
		  & \xi_d(q) \lexstrict \xi_d(p)\\
\text{or}\quad & \xi_d(q) = \xi_d(p) \text{ and } \tr_d(p) \preceq_{d-1}\ \tr_d(q)
\end{array}
\right.$$
* If $d$ is even and different from $0$, then $p \preceq_d\ q$ if 
$$\left\{
\begin{array}{ll}
		  & \max(\xi_d(p)) < \max(\xi_d(q))\\
\text{or}\quad & \max(\xi_d(p)) = \max(\xi_d(q)) \text{ and } \tr_d(p) \preceq_{d-1}\ \tr_d(q)
\end{array}
\right.$$

Now define $\preceq$ as $\preceq_d$.

<figure>
	<img src="{{ '/images/order.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>In both cases $p \preceq q$</figcaption>
</figure>

> **Fact:**
If $p \preceq q$, then for all $r$ we have $p \cdot r \preceq q \cdot r$.

#### Defining a positional strategy for Eve

A play is a finite or infinite word over $V$, which naturally induces a history.
We assume that plays start in $v_0$.
We identify a play and its induced history: 
for instance, for $\pi_1,\pi_2$ two finite plays, we say that $\pi_1 \preceq \pi_2$ if $c(\pi_1) \preceq c(\pi_2)$ holds,
where $c(\pi_1)$ and $c(\pi_2)$ are the histories induced by $\pi_1$ and $\pi_2$.

In the following lemma, we make use of the finite-degree assumption.

> **Lemma:**
Let $(p_n)\nN$ be a sequence of finite plays.
Then it contains a converging subsequence.

We define a positional strategy $\sigma'$ on all vertices reachable by $\sigma$.
The definition is by induction on the rank, which is the length of the smallest path from $v_0$.
For the definition to make sense we need to ensure the following property:
for all vertices $v$ reachable by $\sigma'$, there exists a play ending in $v$
consistent with both $\sigma$ and $\sigma'$.

Assume that $\sigma'$ has been defined on all vertices of rank $k$,
then for all vertices $v \in \VE$ of rank $k+1$ reachable by $\sigma'$,
define $\sigma'(v) = \sigma(\pi_v)$,
where $\pi_v$ is a finite play ending in $v$ and consistent with both $\sigma$ and $\sigma'$,
minimal with respect to $\preceq$ among all such plays.
The property is clearly preserved by this construction.

> **Theorem:**
The strategy $\sigma'$ ensures $\Parity(\Omega)$.

**Proof:**
Consider a play $\pi = v_0 v_1 \ldots$ consistent with $\sigma'$.

For the sake of readability, write $\pi_n = \pi_{v_n}$.
We first observe that $\pi_{n+1} \preceq \pi_n \cdot v_{n+1}$ for all $n$.
Indeed, by definition of $\sigma'$, the play $\pi_n \cdot v_{n+1}$ is consistent with both $\sigma$ and $\sigma'$.
Since the play $\pi_{n+1}$ is the worst play ending in $v_{n+1}$ consistent with both $\sigma$
and $\sigma'$, it is smaller than $\pi_n \cdot v_{n+1}$ with respect to $\preceq$.

Using this inequality, an easy induction shows that $\pi_m \preceq \pi_n \cdot \pi\[n+1,\ldots,m\]$
for all $n < m$, and also $\pi_n \preceq \pi\[0,\ldots,n\]$.
Here we rely on the fact above.

Thanks to the lemma above, the sequence $(\pi_n)\nN$ contains a subsequence indexed by $I \subseteq \N$
which converges to an infinite play, written $\pi_\infty$.
Observe that since for all $n \in I$, the finite play $\pi_n$ is consistent with $\sigma$,
the limit $\pi_\infty$ is consistent with $\sigma$ hence satisfies $\Parity(\Omega)$.

<figure>
	<img src="{{ '/images/construction.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Illustration of the construction</figcaption>
</figure>

We conclude using the following lemma.

> **Lemma:**
Let $\pi = v_0 v_1 \ldots$ be an infinite play and $(\pi_n)\nN$ be a sequence of finite plays, such that:
* $(\pi_n)\nN$ converges to an infinite play $\pi_\infty$,
* $\pi_\infty$ satisfies $\Parity(\Omega)$,
* for all $n$, $\pi_{n+1} \preceq \pi_n \cdot \pi[\|\pi_n+1\|,\ldots,\|\pi_{n+1}\|]$.
<br/>
Then $\pi$ satisfies $\Parity(\Omega)$.

**Proof:**
We proceed by induction on the maximal priority $d$.
The case $d = 0$ is easy.

*First case:* $d$ is even and different from $0$.
There are two cases: 

* Either $d$ appears infinitely many times in $\pi_\infty$.
We show the following property: 

For every $k$ such that $\Omega(\pi_\infty)(k) = d$, there exists $k' \ge k$ such that $\Omega(\pi)(k') = d$.

Consider such a $k$ and $\pi_n$ that coincides with $\pi_\infty$ on the first $k$ positions.
Since $\pi_n \preceq \pi[0,\ldots,n]$, by definition
$\max(\xi_d(\pi_n)) \le \max(\xi_d(\pi[0,\ldots,n]))$.
Observe that $\max(\xi_d(\pi_n)) \ge k$ since $k \in \xi_d(\pi_n)$,
so $\max(\xi_d(\pi[0,\ldots,n])) \ge k$, and the conclusion follows.

It follows that $d$ appears infinitely many times in $\pi$,
so $\pi$ satisfies $\Parity(\Omega)$.

* Or $d$ appears finitely many times in $\pi_\infty$.
We conclude by induction hypothesis, considering the sequence $(\tr_d(\pi_n))\nN$.

*Second case:* $d$ is odd.
We show the following property: 

For every $n$, for every $k \ge n$ such that $\Omega(\pi(k)) = d$, there exists $k' \ge n$ such that $\Omega(\pi_\infty(k')) = d$.

Consider such $k$ and $k$ and $n_0,n_1$ such that $\pi_{n_0}$ coincides with $\pi_\infty$ on the first $n$ positions,
and $\pi_{n_1}$ coincides with $\pi_\infty$ on the first $k$ positions.
Without loss of generality, $n < n_0 < k < n_1$.
We have $\pi_{n_1} \preceq \pi_{n_0} \cdot \pi[n_0+1,\ldots,n_1]$,
so 
$$\xi_d(\pi_{n_0} \cdot \pi[n_0+1,\ldots,n_1]) \lex \xi_d(\pi_{n_1}).$$

Since $\Omega(\pi(k)) = d$, there exists $k' \le k$ such that $\Omega(\pi_{n_1}(k')) = d$.
Since $\pi_{n_0}$ and $\pi_{n_1}$ both coincide with $\pi_\infty$ on the first $n$ positions,
there are equal up to this position, so $k' > n$.
Since $\pi_{n_1}$ coincide with $\pi_\infty$ on the first $k$ positions, 
it follows that $\Omega(\pi_\infty(k')) = d$.

Since $d$ appears finitely many times in $\pi_\infty$,
it implies that $d$ appears finitely many times in $\pi$.
As above, we conclude by induction hypothesis, considering the sequence $(\tr_d(\pi_n))\nN$.

<figure>
	<img src="{{ '/images/odd.svg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Illustration of the odd case</figcaption>
</figure>
