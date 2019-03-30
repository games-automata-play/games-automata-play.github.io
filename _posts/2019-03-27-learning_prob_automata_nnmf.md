---
layout:     post
title:      Weighted automata and matrix factorisations
date:       2019-03-26 9:00:00
author:     Nathana&euml;l Fijalkow
category: 
- Weighted automata
- Learning theory
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      A: "{\\mathcal{A}}",
      rk: "{\\text{rank}}",
      NNrk: "{\\text{rank}_+}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e discuss extensions of Fliess' theorem which says that the smallest weighted automaton for a function is exactly the rank of its Hankel matrix.
The goal is to extend this theorem to subclasses of weighted automata such as probabilistic automata.</p>

Many thanks to the Bellairs Barbados 2019 workshop on Learning and Verification for many discussions on these questions,
and in particular Borja Balle, Alexander Clark, Gerco van Heerdt, Pierre Ohlmann, and Joshua Moermans.

We use the notations from [this post]({{ '/blog/fliess_theorem' | prepend: site.baseurl }}) about weighted automata and the Hankel matrix.

Recall that for a formal series $$f : \Sigma^* \to \R$$, the following theorem is the basis for [weighted automata minimisation]({{ '/blog/fliess_theorem' | prepend: site.baseurl }}) as well as [learning]({{ '/blog/angluin_learning' | prepend: site.baseurl }}).

> **Theorem:** (Fliess '74)
* Any weighted automaton recognising $f$ has at least $\rk(H_f)$ many states,
* There exists a weighted automaton recognising $f$ with $\rk(H_f)$ many states.


### Different matrix factorisations

We will take a slightly different point of view than in the post about [minimisation]({{ '/blog/fliess_theorem' | prepend: site.baseurl }})
since here the central notion is matrix factorisation.

Let $M$ be a matrix, say in $\R^{n \times m}$.
A factorisation of $M$ of dimension $d$ is $M = AW$ where $A \in \R^{n \times d}$ and $W \in \R^{d \times m}$.
One way to understand this is by looking at the rows of $M$: the factorisation $M = AW$ says that the rows of $M$ can be expressed as linear combinations of the rows of $W$.
In other words, $W$ yields a basis for the vector space spanned by the rows of $W$.

We say that a factorisation is
* **non-negative** if the entries of $A$ and $W$ are non-negative
* **residual** if the rows of $W$ are rows of $M$

This yields different notions of ranks: for instance the non-negative rank of $M$ is the smallest $d$ such that there exists a non-negative factorisation of $M$ of dimension $d$.
The usual notion of ranks correspond to general factorisations.
Note that the residual rank is just the rank: indeed, in choosing a basis for the vector space spanned by the rows of $W$, we can always choose rows of $W$.
This is not true anymore for non-negative factorisations, so we have three interesting notions of factorisations: 
general, non-negative, and residual non-negative.

There's another interesting property of factorisation, called **restricted**, which requires that the vector space spanned by the rows of $W$ is equal to the vector space spanned by the rows of $M$.
It does not seem to play a role for weighted automata?

Unfortunately, computing the non-negative factorisation of a matrix is hard (NP-hard by Vavasis, see [here](https://arxiv.org/abs/0708.4149)), 
which contrasts with the usual definition of rank since a factorisation can be found in polynomial time.

The residual condition was introduced by Donoho and Stodden in 2003.
Later, Arora, Ge, Kannan, and Moitra showed in [this paper](https://arxiv.org/abs/1111.0952) that there is a polynomial-time algorithm for computing the non-negative residual factorisation of a matrix.

The algorithm is actually very simple: 
* the first step is to construct $W$, which means selecting the rows of $M$ (which will be rows of $W$) to be those which are not convex combinations of other rows of $M$. 
This can easily be done in polynomial time since checking whether a vector belongs to a convex combination of other vectors can be reduced to linear programming.
* the second step is to construct $A$, which is easily done knowing $M$ and $W$.

Although the first step of the algorithm can indeed be implemented in polynomial time, in practice the following greedy algorithm is often used: 
the set of rows is picked online, adding at each step the row which is the furthest away from the current convex combination of rows.
This algorithm gives an overapproximation of the smallest set of rows, but it is much faster.

### Subclasses of weighted automata

In this post we are interested in the following subclasses of weighted automata:
* **non-negative** automata: all weights are non-negative
* **probabilistic (predictive)** automata: $$\sum_{a \in \Sigma} \Delta(a)$$ is a row stochastic matrix, $$\alpha$$ is a distribution over states, and $$\beta$$ is the constant vector equal to $1$,
which means that $f$ induces for each $n$ a distribution over words of length $n$
* **probabilistic (generative)** automata: non-negative and for all states $q$, we have $$\sum_{a \in \Sigma, q' \in Q} \Delta(a)(q,q') + \beta(q) = 1$$ and $$\alpha$$ is a distribution over states
which means that $f$ induces a distribution over words
* **residual** automata: for all states $q$, there exists a word $w$ such that $$\alpha \Delta(w)$$ is $0$ everywhere and non-zero in $q$.
We sometimes say that $w$ is an anchor for $q$

The notion of anchored appears in [this paper](http://www.aclweb.org/anthology/Q16-1018) by Stratos, Collins and Hsu, called "Unsupervised Part-Of-Speech Tagging with Anchor Hidden Markov Models".

### A closer look at the proof of Fliess' theorem

Let us get back to the proof of Fliess' theorem and see how to relate the type of factorisation of $H_f$ we are using to the type of weighted automaton we consider.

#### The lower bound 

Let $\A$ be a weighted automaton recognising $f$.

Define $P_\A(\cdot,q) \in \R^{\Sigma^* }$ the formal series recognised by the weighted automaton $\A$ with $e_q$ as final vector,
and $S_\A(\cdot,q) \in \R^{\Sigma^* }$ the formal series recognised by the weighted automaton $\A$ with $e_q$ as initial vector.

We have

$$H_f = P_\A \cdot S_\A$$

A few remarks are in order
* without further assumptions, this implies that the rank of $H_f$ is at least $\|Q\|$, the number of states of $\A$
* if $\A$ is non-negative, then this is a non-negative factorisation
* if $\A$ is residual, then this is a residual factorisation

#### The upper bound 

We write $H_f = A W$.
We let $Q$ index the inner dimension, i.e. the rows of $W$.
Let $w$ a finite word, we have

$$H_f(w) = \sum_{q \in Q} A(w,q) W(q),$$

where $H_f(w)$ is the row corresponding to $w$ in $H_f$ and $W(q)$ the row corresponding to $q$ in $W$.

Let us assume that the factorisation $H_f = AW$ is residual (and recall that any factorisation can be chosen residual, but not at the same time residual and non-negative).
This means that $Q$ is a subset of words!

We construct a weighted automaton $$\A$$ as follows
* the set of states is $$Q$$ (did you see that coming?)
* the initial vector is defined by $\alpha(q) = A(\varepsilon,q)$
* the transition is defined $\Delta(a)(q,q') = A(q a,q')$
* the final vector is **$$1$$**, the constant vector equal to $1$

To see that $\A$ recognises $f$ we do a simple induction on words.

Note that if the factorisation is non-negative, then the automaton $\A$ is non-negative.
Also, if $H_f$ is row stochastic, then $\A$ is (predictive) probabilistic,
and with just a bit more work 
if $H_f$ induces a distribution for each length, then $\A$ is (generative) probabilistic.

Let us get back to the general case where the factorisation is not assumed to be residual.
It is not clear what can be said or how to construct an automaton, because rows of $W$ cannot be interpreted in $H_f$.

### The residual variant of Fliess' theorem

We have proved the following.

> **Theorem:**
The non-negative residual rank of $H_f$ is equal to the size of the smallest non-negative residual weighted automaton recognising $f$.

If furthermore $H_f$ is row stochastic, then this is equal to the size of the smallest probabilistic (predictive) residual automaton recognising $f$,
and if $H_f$ induces a distribution for each length, then this is equal to the size of the smallest probabilistic (generative) residual automaton recognising $f$.

### Open questions

Can we drop the residual assumption in the previous theorem?
Meaning: is the non-negative rank of $H_f$ is equal to the size of the smallest non-negative weighted automaton recognising $f$?
For the lower bound, the proof goes through, but I strongly suspect that the upper bound does not hold.
Does the restricted assumption for factorisation play a role here? I also suspect that the answer is no.

Another line of inquiry is how much can the residual theorem be used for learning?
It is not yet clear to me how to adapt the Angluin's style learning algorithm from [this post]({{ '/blog/angluin_learning' | prepend: site.baseurl }}).
Note that it is always possible to learn a weighted automaton using the previous algorithm, the goal here would be to directly learn a probabilistic automaton.
One solution is to learn a weighted automaton (using polynomially many membership and equivalence queries) and then to turn it into a probabilistic one, 
but can we do this efficiently (in terms of complexity)?
We know (thanks to Borja Balle and Joshua Moermans) that there are examples where the anchor words have exponential length.

