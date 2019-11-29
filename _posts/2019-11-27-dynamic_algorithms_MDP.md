---
layout:     post
title:      Reinforcement learning course, part 1, dynamic algorithms
date:       2019-11-27 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Reinforcement learning
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      Q: "{\\mathbb{Q}}",
      N: "{\\mathbb{N}}",
      Z: "{\\mathbb{Z}}",
      M: "{\\mathcal{M}}",
      A: "{\\mathcal{A}}",
      B: "{\\mathcal{B}}",
      E: "{\\mathbb{E}}",
      P: "{\\mathbb{P}}",
      val: "{\\text{val}}",
      Dist: "{\\mathcal{D}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e present some fundamental properties of MDPs and the two families of dynamic algorithms, .</p>

The basic notations are given in the [course outline]({{ '/blog/MDP' | prepend: site.baseurl }}).

Dynamic algorithms make two (unrealistic) assumptions:
* the MDP has a finite set of states and actions, which can be both tabularised
* the transition function is known

The first item means that in particular we can write programs including *for loops* on the set of states and actions,
in other words sweep over all possible states and actions.
Why making these assumptions if they are not realistic? Because the algorithms we will construct in this setting will be useful for the more general setting.

We start with a simple lemma saying that Markovian strategies are as powerful as general ones.

> **Lemma:** For any (general) strategy $\sigma$, there exists a Markovian strategy $\sigma'$ such that for all $t$

$$
\P_{\sigma,s_0}(S_t = s, A_t = a) = \P_{\sigma',s_0}(S_t = s, A_t = a)
$$ 

implying that

$$
\E_{\sigma,s_0}[R_t] = \E_{\sigma',s_0}[R_t]
$$ 

Recall that a Markovian strategy chooses a distribution on actions based only on the current state and the current time step,
i.e. $$\sigma' : S \times \N \to \Dist(A).$$

The proof is simply to define $\sigma'$ as follows: after $t$ steps from the state $s$, play action $a$ with probability
$$\P_{\sigma,s_0}(A_t = a \mid S_t = s).$$

### The finite horizon case

We consider plays of length (exactly) $T$, a fixed finite value.

We let $$\val_*(s,t)$$ denote the optimal expected total reward from $s$ in $t$ moves.

> **Lemma:** $$\val_*(0,s) = 0$$ and 

$$
\val_*(t,s) = \max_{a \in A} \sum_{s' \in S, r \in R} \Delta(s,a)(s',r) (r + \val_*(t+1,s'))
$$

To see that this equation is correct, we remark that the value function for a (Markovian) strategy $\sigma$ satisfies the recursive equation:

$$
\val_\sigma(t,s) = \sum_{a \in A} \sigma(s)(a) \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \val_\sigma(t+1,s'))
$$

Then a convexity argument shows the equality above.

The lemma in particular implies that there are optimal deterministic Markovian strategies. 
The dependence on the time step is necessary: closer to the time step $T$, it makes sense to play moves with higher immediate rewards but dire consequences,
while for a small $t$, moves should be selected to optimise all the rewards to come.

The equation can easily be turned into an algorithm (with pseudo-polynomial complexity).

### The discounted case

For the remainder of this post we consider the expected discounted payoff defined by

$$
\E_\sigma [\sum_i \gamma^i R_i]
$$

For a fixed discount factor $$\gamma$$ less than $$1.$$

We will present two algorithms:
* **value iteration**
* **policy iteration** (sometimes called **strategy iteration** or **strategy improvement**)

In the end we will show that these algorithms both fall into a larger family of algorithms.

#### Characterisation of the optimal values

Recall that $$\val_*(s) = \sup_{\sigma \text{ strategy}} \val_\sigma(s).$$

> **Lemma:** The optimal values satisfy the following equations

$$
\val_*(s) = \max_{a \in A} \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_*(s'))
$$

We first show an inequality. Let $\sigma$ denote an optimal strategy from $s$. Note that a priori, it may not be optimal from other states.

$$
\begin{array}{lll}
\val_*(s) & = & \val_{\sigma}(s) \\
& = & \sum_{a \in A} \sigma(s)(a) \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_\sigma(s')) \\
& \le & \sum_{a \in A} \sigma(s)(a) \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_*(s')) \\
& \le & \max_{a \in A} \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_*(s'))
\end{array}
$$

To show the converse inequality, let $a$ reaching the maximum in the term on the right hand side. Let $\sigma'$ denote an optimal strategy from $s'$.
We define $\sigma$ the strategy playing first $a$, and then simulating $\sigma'$ from $s'$.
Then 

$$
\val_{\sigma}(s) = \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_{\sigma'}(s')) = \sum_{s' \in S,r \in \R} \Delta(s,a)(s',r) (r + \gamma \val_*(s'))
$$

By definition $$\val_*(s) \ge \val_{\sigma}(s)$$, so we proved the converse inequality.

#### The value iteration algorithm

We consider functions $$v : S \to \R$$ as vectors $$v \in \R^S$$, and write $$|v|$$ for the infinity norm of $$v$$.
Let us denote by $$L$$ the operator $$L : \R^S \to \R^S$$ defined by

$$
L(v)(s) = \max_{a \in A} \sum_{s' \in S, r \in \R} \Delta(s,a)(s',r) (r + \gamma v(s')).
$$

> **Lemma:** The vector $$\val_*$$ is a fixed point of $$L$$

This is just a reformulation of the lemma above.

> **Lemma:** The operator $$L$$ is $$\gamma$$-Liptschitz, meaning

$$
|L(v) - L(v')| \le \gamma |v - v'|.
$$

It follows from Banach fixed point theorem (in the complete space $$\R^S$$ equipped with the infinity norm) 
that $$LL$$ has a unique fixed point and that it can be computed as follows:
let $$v_0$$ an arbitrary vector, define $$v_{n+1} = L(v_n)$$, then

$$|v_{n+1} - v_n| \le \gamma^n |v_1 - v_0|$$

implying that 

$$(v_n)_{n \in \N}$$ is a Cauchy sequence, hence converges to $$v_\infty$$ such that $$v_\infty = L(v_\infty)$$,
and since $$\val_*$$ is the unique fixed point of $$L$$ we have $$v_\infty = \val_*.$$

We also get an upper bound on the convergence rate:

$$|\val_* - v_n| \le \frac{\gamma^n}{1 - \gamma} |v_1 - v_0|$$

We obtain an approximation algorithm: for a fixed $$\varepsilon > 0$$,
* By iterating the operator $$L$$ from any initial vector, compute $$v$$ such that $$|L(v) - v| \le \frac{\varepsilon}{2}$$,
implying that $$|\val_* - v| \le \varepsilon$$
* Construct a pure positional strategy $\sigma$ by $$\sigma(s)$$ is an action $$a$$ such that 

$$
|v(s) - \sum_{s' \in S, r \in \R} \Delta(s,a)(s',r) (r + \gamma v(s'))| \le \varepsilon.
$$

Then $$\sigma$$ is an $$\varepsilon$$-optimal strategy.
Thanks to the upper bound on the convergence rate, the number of iterations of the operator $$LL$$ is $$O(\log(\frac{1}{\varepsilon}).$$


#### The policy iteration algorithm

The algorithm manipulates only pure positional strategies. We define two tasks:
* **evaluation**: given a strategy $$\sigma$$, compute (or approximate) $$\val_\sigma$$
* **improvement**: given a strategy $$\sigma$$ and its (approximate) value function $$\val_\sigma$$, construct an improved strategy $$\sigma'$$

##### The evaluation task

Given a strategy $$\sigma$$, its values satisfy the following equations

$$
\val_\sigma(s) = \sum_{s' \in S,r \in \R} \Delta(s,\sigma(s))(s',r) (r + \val_\sigma(s'))
$$

One way of understanding this is that this forms a system of linear equations whose unknowns are $$\val_\sigma(s)$$ for each $$s \in S$$.
This can be solved using Gaussian elimination, which theoretically works but would be very ineffective.

A more practical solution is to use the previous fixed point algorithm specialised to the strategy $$\sigma$$:
we start from some vector $$v_0$$ and then construct better and better approximations:

$$
v_{n+1}(s) = \sum_{s' \in S, r \in \R} \Delta(s,\sigma(s))(s',r) (r + \gamma v_n(s')).
$$

For the same reasons as above, this process converges exponentially fast towards $$\val_\sigma$$.
We fix a threshold $$\varepsilon > 0$$ and stop when $$|v_{n+1} - v_n| \le \frac{\varepsilon}{2}.$$

##### The improvement task

Given a strategy $$\sigma$$ and its value function $$\val_\sigma$$ (we ignore the fact that we actually only compute an approximation!), 
we define 

$$
\sigma'(s) = \text{argmax}_{a \in A} \sum_{s',r} \Delta(s,a)(s',r) (r + \gamma \val_\sigma(s'))
$$

> **Lemma:** We have $$\val_{\sigma'} \ge \val_{\sigma}$$.
Further, if for some $$s$$ we have $$\sum_{s',r} \Delta(s,a)(s',r) (r + \gamma \val_\sigma(s')) > \val_{\sigma}(s)$$,
then the inequality above is strict for $$s.$$

To see that the lemma holds, we write the equations for $$\val_\sigma$$ and $$\val_{\sigma'}$$ and compare them.

At this point let us introduce a notation, the $q$-values.
For a strategy $$\sigma$$, the $q$-value is

$$
q_\sigma(s,a) = \sum_{s',r} \Delta(s,a)(s',r) (r + \gamma \val_\sigma(s'))
$$

So the improvement task is solved by setting

$$
\sigma'(s) = \text{argmax}_{a \in A} q_{\sigma}(s,a)
$$

##### The complete algorithm

The policy iteration algorithm is: 
starting from any strategy, apply alternatively evaluation and improvement to construct better and better strategies.
Note that since we consider only pure positional strategies, which are in finite number, and that each one strictly improves over the previous one,
then the algorithm terminates (that is, assuming we indeed compute the value function exactly, which we don't!).

#### The generalised policy iteration algorithm

Let us get back to the value iteration algorithm, and observe that it also consists in alternating evaluation and improvement steps.
The difference is that at every step:
* the policy iteration algorithm performs a (near-)perfect evaluation of the strategy, 
i.e. it iterates the fixed point algorithm to compute $$v$$ such that $$|v - \val_\sigma| \le \varepsilon$$, 
* whereas the value iteration algorithm only performs one step of the evaluation.

More specifically, the value iteration algorithm does the following. Given $$v_n$$,
it computes $$v_{n+1} = L(v_n)$$ defined by

$$
v_{n+1}(s) = \max_{a \in A} \sum_{s' \in S, r \in \R} \Delta(s,a)(s',r) (r + \gamma v_n(s'))
$$

We can interpret this as doing the following two steps in one go:
* from the strategy $$\sigma_n$$ induced by $$v_n$$ through the improvement task, 
compute the one-step approximation of its $q$-value, which is for state $s$ and action $a$

$$q_n(s,a) = \sum_{s' \in S, r \in \R} \Delta(s,a)(s',r) (r + \gamma v_n(s'))$$

* construct the strategy $$\sigma_{n+1}$$ induced by $$q_n$$ through the improvement task 


