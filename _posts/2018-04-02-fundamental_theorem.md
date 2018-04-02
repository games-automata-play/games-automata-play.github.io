---
layout:     post
title:      The fundamental theorem of statistical learning 
date:       2018-04-02 9:00:00
author:     Nathana&euml;l Fijalkow
---

<p class="intro"><span class="dropcap">W</span>e state and prove the fundamental theorem of statistical learning, which says that a class of functions is learnable,
if and only if it has finite VC dimension.</p>

This post uses all the notations of the [previous post]({{ '/blog/VC' | prepend: site.baseurl }}).

> **Theorem:**
* If $H$ has infinite VC dimension, then it is not PAC-learnable.
* If $H$ has finite VC dimension, then it is PAC-learnable.

## First implication: no free lunch

We prove that if $H$ has infinite VC dimension, then it is not PAC-learnable, which is sometimes called the "no free lunch theorem".

Fix $m$. Assume that $H$ has infinite VC dimension, in particular it has dimension at least $2m$, so there exists $Y$ of size $2m$ which is shattered by $H$.
Recall that to be PAC-learnable, there must exist an algorithm working against any distributions of the inputs. 
Here we will restrict ourselves with the uniform distribution over $Y$, so in particular we can only consider hypotheses $$h : Y \to \left\{0,1\right\}$$.

We will show the following: if we are given $m$ samples, then there is no way that with high probability an algorithm can correctly pick an almost correct function.
More precisely, we show that any algorithm fails on a proportion at least $\frac{1}{8}$ of the inputs with probability at least $\frac{1}{7}$.

These strange constants come from the following lemma.

> **Lemma:**
Let $X$ be a random variable taking values in $[0,1]$ such that $$E[X] \ge \frac{1}{4}$$.
Then $$P(X \ge \frac{1}{8}) \ge \frac{1}{7}$$.

This is a simple application of Markov's inequality: $$P(X \ge a) \le \frac{E[X]}{a}$$, with double complementation.

We write $U$ for the uniform distribution over $Y$.

An algorithm is a function $A$ outputting $$A(S,f) : Y \to \left\{0,1\right\}$$.

> **Lemma:**
In the following quantity, we draw a function $$f : Y \to \left\{0,1\right\}$$ uniformly at random.
$$E_f \left[ E_{S \sim U^m} \left[ L_{U,f}(A(S,f)) \right] \right] \ge \frac{1}{4}$$.

**Proof:**
Recall that $$L_{U,f}(A(S,f))$$ is defined by
$$E_{x \sim U} [L_f(A(S,f),x)]$$.

We lower bound it by 

$$\underbrace{P_{x \sim U} (x \notin S)}_{\ge \frac{1}{2}} \cdot E_{x \sim U} \left[ A(S,f)(x) \neq f(x) \mid x \notin S \right]$$.

Remark that 

$$E_f \left[ E_{S \sim U^m} \left[ E_{x \sim U} \left[ A(S,f)(x) \neq f(x) \mid x \notin S \right] \right] \right] = \frac{1}{2}$$,

which concludes the proof of this lemma.


We now wrap up the proof of the no free lunch theorem.
Fix an algorithm $A$.
It follows from the second lemma that there exists a function $f$ such that 
$$E_{S \sim U^m} \left[ L_{U,f}(A(S,f)) \right] \ge \frac{1}{4}$$.
Thanks to the first lemma, we obtain
$$P_{S \sim U^m} \left( L_{U,f}(A(S,f)) \ge \frac{1}{8} \right) \ge \frac{1}{7}$$.

## Converse implication: finite VC dimension implies PAC-learnability

Let $d$ be the VC dimension of $H$. 
We combine a few ingredients.

The first ingredient is Sauer's lemma.

> **Sauer's Lemma:**
$$\tau_H(m) \le \sum_{i = 0}^d \binom{m}{i} = O(m^d)$$.

There are different proofs of Sauer's lemma, some of them are very beautiful.
Note that it is a purely combinatorial statement.

The main technical piece of work to be done is to obtain generalization bounds using Rademacher complexity.
What is a generalization bound? It's an inequality relating the empirical loss, i.e. $L_{S,f}(h)$, and the actual loss, i.e. $L_{D,f}(h)$.
We say that an hypothesis $h$ generalises if $$|L_{D,f}(h) - L_{S,f}(h) |$$ is small.

> **Lemma:**
$$E_{S \sim D^m} \left[ |L_{D,f}(h) - L_{S,f}(h) | \right] \le R_H(2m)$$.

**Proof:**
COMING SOON


We now combine this with Massart's Theorem (see the [previous post]({{ '/blog/VC' | prepend: site.baseurl }})),
yielding 
$$E_{S \sim D^m} \left[ |L_{D,f}(h) - L_{S,f}(h) | \right] \le \sqrt{\frac{\log(\pi_H(2m))}{m}}$$.

It follows using Markov's inequality that for $\delta > 0$,
$$P_{S \sim D^m} \left( |L_{D,f}(h) - L_{S,f}(h) | \le \frac{1}{\delta} \sqrt{\frac{\log(\pi_H(2m))}{m}} \right) \le 1 - \delta$$.

Thanks to Sauer's lemma, $\pi_H(2m)$ grows polynomially in $m$ (since $d$ is constant), so given $\varepsilon > 0$ and $\delta > 0$, one can choose $m$
such that $$\frac{1}{\delta} \sqrt{\frac{\log(\pi_H(2m))}{m}} \le \varepsilon$$.

An Empirical Risk Minimisation (ERM) algorithm is one that outputs an hypothesis exactly matching the training set, 
i.e. such that $L_{S,f}(A(S,f)) = 0$.

Hence what we proved is that any ERM algorithm ensures 
$$P_{S \sim D^m} \left( L_{D,f}(h) \le \varepsilon \right) \le 1 - \delta$$
for $m$ chosen as above.
Hence $H$ is PAC-learnable.
Note that we did not say anything about how to construct an ERM algorithm.
