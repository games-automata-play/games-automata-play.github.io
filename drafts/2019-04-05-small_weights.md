---
layout:     post
title:      Weight renormalisation in mean payoff games
date:       2019-04-05 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Games
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      Q: "{\\mathbb{Q}}",
      Z: "{\\mathbb{Z}}",
      A: "{\\mathcal{A}}",
      rk: "{\\text{rank}}",
      NNrk: "{\\text{rank}_+}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e discuss a result of Frank and Tardos reducing weights in mean payoff games.</p>

The following result (and its consequence) was pointed out to me by Stéphane Gaubert.

> **Theorem:** (Frank and Tardos '87)
For every $w \in \Q^n$, there exists $v \in \Z^n$ such that
* for all $b \in \Z^n$ such that $||b||_1 \le N$, we have $w \cdot b > 0$ if and only if $v \cdot b > 0$
* $||v||_\infty \le 2^{4n^3} N^{n+2}$

Furthermore this is effective, one can construct $v$ in polynomial time.
The paper of Frank and Tardos can be found [here](https://link.springer.com/article/10.1007/BF02579200).

### Application to mean payoff games

As Stéphane Gaubert acutely observed, this implies that we can renormalise the weights of a mean payoff game to be bounded by $2^{4m^3} m^{m+2}$,
where $m$ is the number of edges. In other words, the weights use a polynomial number of bits in the size of the graph.

Indeed, a weight function is $w : E \to \Q$, i.e. $w \in \Q^m$.
Two weight functions are equivalent, meaning the induced mean payoff games have the same winner (when considering the mean payoff objective with threshold $0$),
if for all cycles in the underlying graph, positivity is equivalent for the two weight functions.
This is directly implied by saying that for all $b \in \Z^n$ such that $||b||_1 \le m$, we have $w \cdot b > 0$ if and only if $v \cdot b > 0$,
where $w,u$ are the weight functions.
The result follows by applying the theorem above.

