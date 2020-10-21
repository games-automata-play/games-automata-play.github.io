---
layout:     post
title:      The fundamental theorem of statistical learning 
date:       2018-04-02 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Learning theory
---

<p class="intro"><span class="dropcap">W</span>e state and prove the fundamental theorem of statistical learning, which says that a class of functions is learnable
if and only if it has finite VC dimension.</p>

This post uses all the notations of the [previous post]({{ '/blog/VC' | prepend: site.baseurl }}).

> **Theorem:**
* If $H$ has infinite VC dimension, then it is not PAC-learnable.
* If $H$ has finite VC dimension, then it is PAC-learnable.

## First implication: no free lunch

We prove that if $H$ has infinite VC dimension, then it is not PAC-learnable, which is sometimes called the "no free lunch theorem".

Fix $m$. Assume that $H$ has infinite VC dimension, in particular it has dimension at least $2m$, so there exists $Y$ of size $2m$ which is shattered by $H$.
Recall that to be PAC-learnable, there must exist an algorithm working against any distributions of the inputs. 
Here we will restrict ourselves to the uniform distribution over $Y$, so in particular we can only consider hypotheses $$h : Y \to \left\{0,1\right\}$$.

We will show the following: if we are given $m$ samples, then there is no way that with high probability an algorithm can correctly pick an almost correct function.
The intuitive reason is that the correct function can be any function $$h : Y \to \left\{0,1\right\}$$ and $Y$ has size $2m$: using $m$ samples we only get (at most) half of the inputs.

We show that for any algorithm there exists a target function for which the algorithm fails on a proportion at least $\frac{1}{8}$ of the inputs with probability at least $\frac{1}{7}$.
These strange constants come from the following lemma.

> **Lemma:**
Let $X$ be a random variable taking values in $[0,1]$ such that $$E[X] \ge \frac{1}{4}$$.
Then $$P(X \ge \frac{1}{8}) \ge \frac{1}{7}$$.

This is a simple application of Markov's inequality: $$P(X \ge a) \le \frac{E[X]}{a}$$, with double complementation:

$$P(X \ge \frac{1}{8}) = 1 - P(X \le \frac{1}{8})
= 1 - P(1 - X \ge \frac{7}{8})
\ge 1 - \frac{E[1 - X]}{\frac{7}{8}}
= 1 - \frac{8}{7} \left(1 - E[X] \right)
= \frac{8}{7} E[X] - \frac{1}{7}
\ge \frac{8}{7} \frac{1}{4} - \frac{1}{7}
= \frac{1}{7}$$

We write $U$ for the uniform distribution over $Y$.

An algorithm is a function $A$ taking as input the labelled samples $S_f$ and outputting $$A(S_f) : Y \to \left\{0,1\right\}$$.
Let us say that the algorithm **fails** on $f$ if 

$$P_{S \sim U^m} \left( L_{U,f}(A(S_f)) \ge \frac{1}{8} \right) \ge \frac{1}{7}$$.

Thanks to the lemma above, it is enough to show that 

$$E_{S \sim U^m} \left[ L_{U,f}(A(S_f)) \right] \right] \ge \frac{1}{4}$$.

We show that any algorithm fails on at least one target function $f$.
To this end we draw a function $f$ uniformly at random, and show that the expectation of failing is at least $\frac{1}{4}$,
implying that there indeed exists a function $f$ on which the algorithm fails.

> **Lemma:**
In the following quantity, we draw a function $$f : Y \to \left\{0,1\right\}$$ uniformly at random.
$$E_f \left[ E_{S \sim U^m} \left[ L_{U,f}(A(S_f)) \right] \right] \ge \frac{1}{4}$$.

**Proof:**
Recall that $$L_{U,f}(A(S_f))$$ is defined by
$$E_{x \sim U} [L_f(A(S_f),x)]$$.

We write the following case distinction: either $x \in S$ or $x \notin S$.

$$E_{x \sim U} [L_f(A(S_f),x)] = P_{x \sim U} (x \notin S) \cdot E_{x \sim U} \left[ A(S_f)(x) \neq f(x) \mid x \notin S \right] + P_{x \sim U} (x \in S) \cdot E_{x \sim U} \left[ A(S_f)(x) \neq f(x) \mid x \in S \right]$$.

Intuitively, in the case where $x \in S$ the algorithm should not err, it is reasonable to think that $A(S_f)(x) = f(x)$, because the algorithm has access to the value $f(x)$ since $x \in S$.
However if $x \notin S$, then the algorithm can only guess the value of $f(x)$. Hence we focus on the first term, lower bounding the second by $0$:

$$E_{x \sim U} [L_f(A(S_f),x)]
\ge P_{x \sim U} (x \notin S) \cdot E_{x \sim U} \left[ A(S_f)(x) \neq f(x) \mid x \notin S \right]
$$.

We make two observations:
* $P_{x \sim U} (x \notin S) \ge \frac{1}{2}$: indeed $U$ is the uniform distribution over $2m$ elements, and $S$ is an iid sample of $m$ elements, hence with probability at least half $x$ does not belong to the samples
* $E_f \left[ E_{S \sim U^m} \left[ E_{x \sim U} \left[ A(S_f)(x) \neq f(x) \mid x \notin S \right] \right] \right] = \frac{1}{2}$$: 
indeed for $x \notin S$ the value of $f(x)$ is either $1$ or $0$ each with probability $\frac{1}{2}$ since $f$ is drawn uniformly at random.

This concludes the proof of this lemma.

We now wrap up the proof of the no free lunch theorem.
Fix an algorithm $A$.
It follows from the second lemma that there exists a function $f$ such that 
$$E_{S \sim U^m} \left[ L_{U,f}(A(S_f)) \right] \ge \frac{1}{4}$$.
Thanks to the first lemma, we obtain
$$P_{S \sim U^m} \left( L_{U,f}(A(S_f)) \ge \frac{1}{8} \right) \ge \frac{1}{7}$$.

## Converse implication: finite VC dimension implies PAC-learnability

Let $d$ be the VC dimension of $H$. 
We combine a few ingredients.

The first ingredient is Sauer's lemma.

> **Sauer's Lemma:**
$$\tau_H(m) \le \sum_{i = 0}^d \binom{m}{i} = O(m^d)$$.

There are different proofs of Sauer's lemma, some of them are very beautiful.
My favourite is [the second one in this lecture notes](https://www.cse.buffalo.edu/~hungngo/classes/2010/711/lectures/sauer.pdf).
Note that it is a purely combinatorial statement.

The following theorem relates the Rademacher complexity to the growth function:

> **Massart's Theorem:**
$$R_H(m) \le \sqrt{\frac{2 \log(\pi_H(m))}{m}}$$.

The proof of analytical and relies on Hoeffding's inequality.
See [p39/40 of the Foundations of Machine Learning book](https://cs.nyu.edu/~mohri/mlbook) for a proof.


Combining these two results yields

$$R_H(m) = O\left( \sqrt{ \frac{\log(m)}{m} } \right)$$

In particular it goes to $0$ as $m$ goes to infinity.



The main technical piece of work to be done is to obtain generalization bounds using Rademacher complexity.
What is a generalization bound? It's an inequality relating the empirical loss, i.e. $L_{S,f}(h)$, and the actual loss, i.e. $L_{D,f}(h)$.
We say that an hypothesis $h$ generalises if $$L_{D,f}(h) - L_{S,f}(h)$$ is small.

> **Lemma:**
$$E_{S \sim D^m} \left[ L_{D,f}(h) - L_{S,f}(h) \right] \le 2 R_H(2m)$$.

**Proof:**
By definition $$L_{D,f}(h) = E_{x \sim D} [L_f(h,x)]$$ and 
$$L_{S,f}(h) = \frac{1}{m} \sum_{i = 1}^m L_f(h,x_i)$$.

To compare these two quantities we first make them look similar:
observe that $$L_{D,f}(h) = E_{S' \sim D^m} [L_{S,f}(h)]$$.
We write $$S' = (x'_i)_{i \in [1,m]}$$.

We obtain
$$E_{S,S' \sim D^m} \left[ L_{S,f}(h) - L_{S',f}(h) \right] = E_{S,S' \sim D^m} \left[ \frac{1}{m} \sum_{i = 1}^m L_{f}(h,x_i) - L_{f}(h,x'_i) \right]$$.

Recall that the goal is to upper bound by the Rademacher complexity.
We claim that the quantity above is equal to

$$E_{\sigma,\sigma' \in \left\{-1,+1\right\}^m} E_{S,S' \sim D^m} \left[ \frac{1}{m} \sum_{i = 1}^m \sigma_i L_{f}(h,x_i) - \sigma'_i L_{f}(h,x'_i)) \right]$$

Indeed, fix $\sigma$ and $\sigma'$. For each $i$ there are four cases:
* either $\sigma_i = \sigma'_i = +1$, then this is the term as in the quantity above,
* or $\sigma_i = \sigma'_i = -1$, this is the term as in the quantity above once $x_i$ and $x'_i$ are swapped in $S$ and $S'$,
* the other two terms cancel out ($\sigma_i = +1$ and $\sigma'_i = -1$ with $\sigma_i = -1$ and $\sigma'_i = +1$). 

This quantity is smaller than $$2 R_H(2m)$$, which concludes the proof of this lemma.


#### Proof wrap up

It follows using Markov's inequality that for $\delta > 0$,

$$P_{S \sim D^m} \left( L_{D,f}(h) - L_{S,f}(h) \le \frac{2}{\delta} R_H(2m) \right) \le 1 - \delta$$.

Thanks to the discussion above (using Sauer's Lemma and Massart's Theorem) we know that $$R_H(2m)$$ converges to $0$ when $m$ goes to infinity.
So there exists some $m$ (which depend on $\varepsilon$ and $\delta$) such that 

$$P_{S \sim D^m} \left( L_{D,f}(h) - L_{S,f}(h) \le \varepsilon \right) \le 1 - \delta$$.

An Empirical Risk Minimisation (ERM) algorithm is one that outputs an hypothesis exactly matching the training set, 
i.e. such that $L_{S,f}(A(S_f)) = 0$.
Hence what we proved is that any ERM algorithm ensures 
$$P_{S \sim D^m} \left( L_{D,f}(h) \le \varepsilon \right) \le 1 - \delta$$
for $m$ chosen as above.
Hence $H$ is PAC-learnable.
Note that we did not say anything about how to construct an ERM algorithm.

