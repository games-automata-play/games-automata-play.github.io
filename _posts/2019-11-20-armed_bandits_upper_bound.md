---
layout:     post
title:      The upper confidence bound (UCB) analysis for mutli-armed bandits 
date:       2019-11-20 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Learning theory
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
      E: "{\\mathbb{E}}",
      P: "{\\mathbb{P}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e present a proof that the upper confidence bound yields an (asymptotically) optimal algorithm for regret minimisation of muti-armed bandits.
This result was proved by Auer, Cesa-Bianchi, and Fischer in [this paper](http://homes.di.unimi.it/cesa-bianchi/Pubblicazioni/ml-02.pdf). The proof is the same, with some more context and explanations.</p>

The setting is the following: there are $K$ machines, which are each given by a distribution on rewards, which here are in $[0,1]$.
We write $i \in [1,K]$ for a bandit. Let $\mu_i$ denote the mean of the distribution for the machine $i$.
Initially, we do not know anything except for the number $K$ of machines.
At each time step, we can choose a machine $i$ and get a reward from $i$. 
The goal is to **maximise the expected total payoff**, as a function of the number of steps.

An algorithm selects at each time step a machine $i$ based on the rewards observed so far. 
For the sake of clarity we will not mark the dependence on the algorithm on each random variables.
We write:
* $i$ is typically a machine (in $[1,K]$)
* $t,T$ are number of steps
* $n(i,T)$ is the number of times the machine $i$ was played in the first $T$ steps
* $M(t)$ is the machine chosen at time step $t$
* $X(t)$ is the reward from the machine chosen at time step $t$
* $X(i,t)$ is the reward of playing the machine $i$ at time step $t$; if $i$ was not chosen at time $t$, then it is $0$
* $\widehat{X}(i,t)$ is the **empirical** reward of machine $i$ in the first $T$ steps, defined by

$$\widehat{X}(i,t) = \frac{1}{n(i,T)} \sum_{t \in [1,T]} X(i,t)$$

An optimal machine is one maximising the average reward $\mu$. We write $*$ for this machine.

The goal is to maximise

$$
\E[\sum_{t \in [1,T] X(t)]
$$

Equivalently, we can define the regret of an algorithm, as the difference between the optimal reward in $T$ steps,
which is simply $\mu_* \cdot T$, and the obtained reward:

$$
R = \mu_* \cdot T - \sum_{t \in [1,T] X(t)
$$

Maximising the expected total payoff is the same as minimising the regret.
Note that both are functions of $T$ the total number of steps, hence minimising / maximising is not very clearly defined.
A first formalisation is to consider the asymptotic behaviour, but of course it is more desirable to also give bounds for any $T$.

### A class of algorithms: index policies

A very natural class of algorithms is to assign at each time step $t$ and to each machine $i$ a number $U(i,t)$, 
and to choose at each step the machine maximising this number.
(To be a bit specific: we first choose each machine once, to meaningfully initialise these numbers.)

The first naive attempt is to use $U(i,t) = \widehat{X}(i,t)$.
The empirical reward converges towards the actual expected reward, i.e. $\lim_T \widehat{X}(i,T) = \mu_i$,
so this algorithm finds the optimal machine in the limit.
The problem is that it converges very slowly: one can show that the regret in linear in $T$.

Indeed, the missing information is whether the empirical reward for machine $i$ can be considered **accurate or not**, 
in other words whether the machine $i$ was played enough times to trust the empirical reward.
This leads to indices of the form $U(i,t) = \widehat{X}(i,t) + c(i,t)$, with $c(i,t)$ reflecting on the quality of the empirical reward.

It turns out that there is a choice of $c(i,t)$ which leads an algorithm with logarithmic regret:

> **Theorem:** (Auer, Cesa-Bianchi, and Fischer 2002)
The regret of the index policy for $c(i,t) = \sqrt{ \frac{2 \log(t)}{n(i,t)} }$ is $O(\log(t))$

As we will explain in another blog post, this is asymptotically optimal, as proved by Lai and Robbins in 1985.

### Why the choice of $c(i,t)$?

As we were discussing above, the role of $c(i,t)$ is to quantify the accuracy of the empirical reward.
Indeed, if the machine $i$ was not played a lot of times, its emprical reward may be off by a lot, and it is therefore interesting to choose it, so $c(i,t)$ should be large.

The Chernoff-Hoeffding bound gives some information on the accuracy of the empirical reward:

$$
\P(\widehat{X}(i,t) \ge \mu_i + c) \le \exp^{-2 c^2 n(i,t)}
$$

and similarly

$$
\P(\widehat{X}(i,t) \le \mu_i - c) \le \exp^{-2 c^2 n(i,t)}
$$

So, choosing $c(i,t) = \sqrt{ \frac{2 \log(t)}{n(i,t)} }$ yields bounds in $t^{-4}$, which decreases just fast enough.

### Proof of the logarithmic regret

We first observe that the regret $R$ is equal to

$$
\sum_{i \in [1,K]} (\mu_* - \mu_i) n(i,T)
$$

Indeed, $\E[\sum_{t \in [1,T]} X_t] = \sum_{i \in [1,K]} \mu_i n(i,T)$.

This means that it will be enough to upper bound $n(i,T)$ for the non-optimal machines $i$.

We let $\Delta_i$ denote $\mu_* - \mu_i$.

To simplify the notations, we do not account for the initial $K$ steps, which are used to initialise the rewards by playing each machine once.

We fix a machine $i$. 
Let $\ell$ to be determined later: since when the machine $i$ has been played only a few times, the empirical reward is very noisy,
we will mostly be interested in studying the situation were it has been played at least $\ell$ times.
For an event $A$, we write ${A}$ for the random variable with value $1$ if $A$ is realised and $0$ otherwise. 

$$
n(i,T) \le \ell + \sum_{t \in [1,T]} {M(t) = i \wedge n(i,t-1) \ge \ell}
$$

By definition of the algorithm, for the machine $i$ to be chosen over the optimal machine $*$, we must have

$$
\widehat{X}_{*,t-1} + c_{*,t-1} \le \widehat{X}_{i,t-1} + c_{i,t-1}
$$

A fortiori,

$$
\min_{s \in [1,t-1]} \widehat{X}_{*,s} + c(*,s) \le \max_{s' \in [\ell,t-1]} \widehat{X}_{i,s} + c(i,s)
$$

So we get

$$
n(i,T) \le \ell + \sum_{t \ge 1} \sum_{s \in [1,t-1]} \sum_{s' \in [\ell,t-1]} { \widehat{X}_{*,s} + c(*,s) \le \widehat{X}_{i,s'} + c(i,s') }
$$

The event $\widehat{X}_{*,s} + c(*,s) \le \widehat{X}_{i,s'} + c(i,s')$ cannot be realised if all three following events are realised:
* $\widehat{X}_{*,s} \le \mu_* - c(*,s)$, meaning that $*$ is underapproximated
* $\widehat{X}_{i,s'} \le \mu_i + c(i,s')$, meaning that $i$ is overapproximated
* $\mu_* < \mu_i + 2 c(i,s')$

The discussion above for the Chernoff-Hoeffding bound shows that the first two events have each probability upper bounded by $t^{-4}$.
We let $\ell = \frac{8 \log(n)}{\Delta_i^2}$.
For $s' \ge \ell$, the third event cannot be realised by definition of $c(i,s')$.

It follows that

$$
n(i,T) \le \ell + \sum_{t \ge 1} \sum_{s \in [1,t-1]} \sum_{s' \in [\ell,t-1]} 2 t^{-4} \le \ell + \frac{\pi^2}{3}
$$
