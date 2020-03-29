---
layout:     post
title:      A polynomial time algorithm for the equivalence problem of weighted automata over a field
date:       2019-04-06 9:00:00
author:     Nathana&euml;l Fijalkow
category:   
- Weighted automata
- Research
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      Q: "{\\mathbb{Q}}",
      N: "{\\mathbb{N}}",
      Z: "{\\mathbb{Z}}",
      A: "{\\mathcal{A}}",
      B: "{\\mathcal{B}}",
      rk: "{\\text{rank}}",
      NNrk: "{\\text{rank}_+}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e present an algebraic algorithm solving the equivalence problem for weighted automata over a field (in particular probabilistic automata) in polynomial time.</p>

#### Edits
29/03/2020: Clarified the model to avoid a terminology clash

We consider weighted automata over an arbitrary semiring. The key assmuption in this post is that the semiring is a field. For simplicity we will use the reals $$\R$$, but everything readily extends to any field.
The algorithm does not extend to semirings which are not fields, for instance it is well known that for the semiring $$(\N,\min,+)$$, the equivalence problem is undecidable.


A formal series (here over the reals) is a function $$\Sigma^* \to \R$$.
For recognising formal series we use weighted automata: 

$$\A = (Q, \alpha \in \R^Q, (\Delta(a) \in \R^{Q \times Q})_{a \in A}, \eta \in \R^Q)$$.

Such an automaton recognises the function 

$$\A(a_1 \cdots a_n) = \alpha \cdot \underbrace{\Delta(a_1) \cdots \Delta(a_n)}_{\Delta(a_1 \cdots a_n)} \cdot \eta$$

Probabilistic automata are a special case of weighted automata.

> **Theorem:** (Schützenberger '61)
The following problem can be solved in polynomial time: given two weighted automata $\A$ and $\B$ **over a field**, 
determine whether $\A = \B$, meaning for all words $w \in \Sigma^*$, we have $\A(w) = \B(w)$.

As evidence that this is an important theorem, it has been rediscovered a number of times. 
The techniques described here are close to the original ones, which where about minimising weighted automata.

We simplify the problem by constructing a weighted automaton recognising the formal series $\A - \B$: the automaton has size the sum of the two automata.
The question reduces to whether $\A = 0$, where $\A$ is given by a weighted automaton.

We let $\Delta(a)(s)$ denote the vector in $\R^Q$ whose entry for $s'$ is $\Delta(a)(s,s')$.

We define the following sequence of sets in $\R^Q$:
$$
\begin{array}{lll}
  u \in X_0 & \Longleftrightarrow & \alpha \cdot u = 0 \\
  u \in X_{k+1} & \Longleftrightarrow & u \in X_k \text{ and for all } a \in \Sigma,\ u \cdot \Delta(a) \in X_k.
\end{array}
$$

> **Proposition:**
* $X_0 \supseteq X_1 \supseteq X_2 \supseteq \cdots$,
* $X_k$ is a vector space in $\R^Q$,
* if $X_{k+1} = X_k$, then $X_{k+2} = X_{k+1}$,
* $X = X_{Q - 1} = X_{Q} = \cdots$,
* for all $k$, we have the equivalence between for all $w \in \Sigma^{\le k}, \A(w) = 0$ and $\eta \in X_k$.

Before going into the proof, we explain how this implies an algorithm.
The algorithm iteratively compute bases, starting from $X_0$, then $X_1$, and so on until it stabilises,
which happens in at most $Q$ steps. 
We let $X$ denote the limit.
The final step is to check whether $\eta$ belongs to $X$.	

The core of the algorithm relies on polynomially calls (specifically, at most $Q$) to the following task: 
given a matrix $M$ in $\R^{Q \times Q}$ and a basis for a vector space $V$, 
compute a basis for the vector space $\set{u \mid u \cdot M \in V}$,
which can be performed in polynomial time.

Indeed, let $M$ be the matrix with $\Sigma + 1$ blocks each of dimension $Q \times Q$:
the first one is the identity, and for each $a \in A$ a block equal to $\Delta(a)$.
By construction 
$X_{k+1} = \set{u : u \cdot M \in X_k^{1 + \Sigma}}$.

We proceed to the proof of the proposition. The first two items are easy to see, so we focus on the remaining three.

* This is a symbolic argument, better understood using the remark that $X_{k+1} = \set{u \mid u \cdot M \in X_k^p}$, for some matrix $M$ and some $p$ in $\N$.
Assume that $X_{k+1} = X_k$. We already know that $X_{k+1} \supseteq X_{k+2}$, so we prove the converse implication.
Let $u$ in $X_{k+1}$, since $X_{k+1} = X_k$ we have that $u$ is in $X_k$, so by definition $u \cdot M$ is in $X_{k+1}^p$, which means that $u$ is in $X_{k+2}$.
* This is a dimension argument: the sequence $(\text{dim}(X_k))_{k \in \N}$ is non-increasing, takes values in $[0,Q-1]$, and stabilises as soon as it hits twice the same value.
Note that we **do** need the property above saying that once the same value is hit twice then the sequence stabilises. 
There is a special place in hell for people who forget to state (and prove) that property.
* We first prove by induction on $k$ that $X_k$ is the set of $u \in \R^Q$ such that for all $w \in \Sigma^{\le k}$, we have $\alpha \cdot \Delta(w) \cdot u = 0$.
Both the base and the inductive cases are easy.
This readily implies that for all $w \in \Sigma^{\le k}, \A(w) = 0$ if and only if $\eta \in X_k$.

