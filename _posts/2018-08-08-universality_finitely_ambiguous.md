---
layout:     post
title:      The universality problem for automata with bounded ambiguity 
date:       2018-08-08 10:00:00
author:     Ritam Raha and Nathana&euml;l Fijalkow
category:   Automata
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      N: "{\\mathbb{N}}",
      R: "{\\mathbb{R}}",
      M: "{\\mathcal{M}}",
      A: "{\\mathcal{A}}",
      rank: "{\\text{rank}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e prove that the universality problem for automata with fixed ambiguity is decidable in polynomial time.</p>

Given an automaton $\A$ on alphabet $A$ recognising the language $L(\A)$, the universality problem asks whether

$$\forall w \in A^*, w \in L(\A)$$

The universality problem is PSPACE-complete for the class of all non-deterministic automata.
In this note, we will prove that the problem is decidable in polynomial time if the automaton has fixed ambiguity.
This result was proved in [this paper](https://epubs.siam.org/doi/pdf/10.1137/0214044). 
Our proof is very similar.
Most of the credit goes to Ritam Raha for rediscovering the proof, with the support and supervision of Nathan Lhote, Filip Mazowiecki, and Vincent Penelle.

### Background on linear recurrence sequences

Recall that an LRS of order $k$ is a sequence $(u_\ell)_{\ell \in \N}$ such that

$$u_\ell = X \cdot A^{\ell} \cdot Y$$

where $A \in \R^{k \times k}$ and $X,Y \in \R^k$.
For instance, the Fibonacci sequence $(0,1,1,2,3,5,8,13,\ldots)$ is

$$F_\ell = (1\qquad 0) \cdot \left( \begin{array}{cc} 1 & 1 \\ 1 & 0 \end{array} \right)^{\ell} \cdot (0\qquad 1)$$

The two properties of LRS we rely on are:
> **Theorem:**
* The $\ell$-th term of an LRS of order $k$ can be computed in time $O(\log(\ell) \cdot k^3)$
* Two LRS of order at most $k$ are equal if and only if they agree on the first $k$ terms

These two properties are well known and best proved using an algebraic point of view.

The following lemma shows how LRS naturally appear in the study of automata.
We let $n$ be the number of states of the automaton $\A$.

> **Lemma:**
For **any** non-deterministic automaton, the sequence 
$(ACC(\ell))_{\ell \in \N}$ calculating the number of accepting runs of length $\ell$
is an LRS of order $n$

The proof is one line long
$$ACC(\ell) = I \cdot \Delta^{\ell} \cdot F$$

### The universality problem for unambiguous automata

Recall that an automaton is $k$-ambiguous if for every word, there are **at most** $k$ **accepting** runs for $w$.
Note that we consider only accepting runs; there might be more (non-accepting) runs.

> **Theorem:**
The universality problem for unambiguous automata is in PTIME

The crux is to show that the sequence

$$\alpha(\ell) = \text{ number of words of length } \ell \text{ accepted by } \A$$

is a linear recurrence sequence (LRS) of order $n$.

We prove the theorem: observe that for an unambiguous automaton $\A$, 
we have $\alpha = ACC$ since counting accepted words is the same as counting accepting runs.
Hence $$(\alpha(\ell))_{\ell \in \N}$$ is an LRS of order $n$.
Note that $$(|A|^{\ell})_{\ell \in \N}$$ is an LRS of order $1$.
It follows that to check universality of $\A$, it is enough to check whether 

$$\alpha(\ell) = |A|^{\ell}$$

for $\ell \le n$, which can be done in polynomial time.

### The universality problem for finitely ambiguous automata

We fix $k \in \N$.

> **Theorem:** 
The universality problem for $k$-ambiguous automata is in PTIME

We will get a complexity in $n^{O(k)}$, hence the algorithm is polynomial only when $k$ is constant.
One can indeed show that for non-constant $k$ the universality problem is PSPACE-complete again.

The main difficulty in extending the previous idea is that counting accepted words cannot be easily reduced to counting accepting runs, because to an accepted word corresponds 
to a number of accepting runs between $1$ and $k$.
Nevertheless, we will show that the sequence

$$\alpha(\ell) = \text{ number of words of length } \ell \text{ accepted by } \A$$

is a linear recurrence sequence (LRS) of order $n^{O(k)}$.

For $p \le k$, denote
$$\alpha_p(\ell) = \text{ number of words of length } \ell \text{ having exactly } p \text{ accepting runs over } \A$$

We have $\alpha(\ell) = \sum_{p = 1}^k \alpha_p(\ell)$
hence it suffices to show that each $(\alpha_p(\ell))_{\ell \in \N}$ is an LRS of order $n^{O(k)}$.

We construct an automaton $\A_p$ as follows. 
On a first approximation, we use the powerset construction capped to sets of size at most $p$.
It is technically convenient for what follows to identify two runs of $\A_p$ which contain exactly the same runs of $\A$.
To this end, we fix a linear order on $Q$ and choose as set of states for $\A_p$ non-decreasing tuples of $Q$ of size at most $p$.
A state is final if it is a tuple of size exactly $p$ containing only accepting states of $\A$.
Hence a word $w$ is accepted by $\A_p$ if and only if $w$ has **at least** $p$ accepting runs over $\A$.
The automaton $\A_p$ has $O(n^p)$ states.

We let $ACC_p(\ell)$ be the number of accepting runs of length $\ell$ of $\A_p$.
We observe that

$$ACC_p(\ell) = \sum_{j = p}^k {j \choose p} \alpha_j(\ell)$$

Indeed:
* each word having exactly $p$ runs induce one run of $\A_p$
* each word having exactly $p+1$ runs induce $p+1$ runs of $\A_p$, obtained by choosing $p$ runs among $p+1$
* more generally, each word having exactly $j$ runs induce $${j \choose p}$$ runs of $\A_p$, obtained by choosing $p$ runs among $j$

In other words, writing $ACC$ for the vector of length $k$ with $(ACC_1,\ldots,ACC_k)$ and $\alpha$ for $(\alpha_1,\ldots,\alpha_k)$
we obtain

$$ACC = M \cdot \alpha$$

with $M$ upper triangular and invertible. Hence

$$\alpha = M^{-1} ACC$$

Thanks to the lemma above each $ACC_p$ is an LRS of order $n^{O(k)}$, which implies that each $\alpha_p$ as well.

This yields a polynomial time algorithm similarly as for unambiguous automata.
Note that this also implies that if $\A$ is $k$-ambiguous and not universal, 
then there exists a word $w$ that is not accepted by $\A$ of length $n^{O(k)}$.

