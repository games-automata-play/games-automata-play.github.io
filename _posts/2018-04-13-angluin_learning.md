---
layout:     post
title:      Angluin's style learning for weighted automata 
date:       2018-04-13 9:00:00
author:     Nathana&euml;l Fijalkow
category:   
- Weighted automata
- Learning theory
- research
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      A: "{\\mathcal{A}}",
      rk: "{\\text{rank}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e show that weighted automata over the reals can be learned efficiently in Angluin's supervised scenario.</p>

This post uses all the notations of the [previous post]({{ '/blog/fliess_theorem' | prepend: site.baseurl }}),
and the result presented here was proved in this [paper](https://dl.acm.org/citation.cfm?id=337257).

The scenario is Angluin's style learning: a master knows a function $$f : \Sigma^* \to \R$$, and can answer two types of queries.
* membership query: when asked about $w \in \Sigma^*$, the master replies with $$f(w)$$
* equivalence query: when asked whether $f = \A$ for a given weighted automaton, the master replies YES or gives a counter-example.

> **Theorem:** (Beimel, Bergadano, Bshouty, Kushilevitz, Varricchio, 2000)
Weighted automata are efficiently learnable using a perfect teacher.

The algorithm maintains a set $$X$$ of rows with $$\varepsilon \in X$$ and a set $$Y$$ of columns with $$\varepsilon \in Y$$
such that
* $X$ and $Y$ have the same size,
* the matrix $$H_f(X,Y)$$ guaranteed to have full rank.

The first question is how to initialise the data, and then how to maintain it.
For initialisation, start with an equivalence query asking whether the function is identically zero.
The master replies with a counter-example $y$, and we can construct the data for $$X = \{\varepsilon\}, Y = \{y\}$$ by asking a membership query for $$f(y)$$. 
This matrix has rank $1$.

We now explain how to extend the data. This goes in two steps:
* constructing an automaton and submitting it to the master,
* expanding the matrix using the counter-example given by the master.

We construct a hypothesis automaton. The first step is to fill in $$H_f(Xa,Y)$$ using at most $$n^2$$ membership queries.
The automaton $$\A$$ is as follows.
* The set of states is $$X$$.
* The initial vector is $$e_1$$.
* The transition is defined to satisfy $$H_f(Xa,Y) = \Delta(a) \cdot H_f(X,Y)$$, i.e. $$\Delta(a) = H_f(Xa,y) \cdot H_f(X,Y)^{-1}$$.
* The final vector is $$H_f(X, \{\varepsilon\})$$.

Note that we **do not claim** that this automaton satisfies $$\A(xy) = f(xy)$$ for $$x \in X, y \in Y$$.
(Thanks Jérôme Leroux for the question! One can find counter-examples to this claim:
over a one-letter alphabet:
$$H_f(\varepsilon,\{a^*\}) = (1, 0, 1, 0, 0, 1, 0, 0, \ldots)$$
with $$X = \{ \varepsilon, a^3 \}$$ and $$Y = \{ \varepsilon, a^2 \}$$.)

We submit $$\A$$ to the master, and assume that we get in return a word $z$ such that $$\A(z) \neq f(z)$$.

We say that a word $w$ is correct if 
$$\Delta(w) \cdot \eta = H_f(Xw, \{\varepsilon\})$$.

We observe that $\varepsilon$ is correct and that $z$ is not correct,
since this would imply $$\A(z) = \alpha \cdot \Delta(z) \cdot \eta = f(z)$$.

Let $w$ such that $w$ is correct and $aw$ is not correct.
Such a word can be detected by querying the suffixes of $z$ of increasing length, thanks to the two observations above. 

By definition $w$ is correct:

$$\Delta(w) \cdot \eta = H_f(Xw, \{\varepsilon\})$$.

And $aw$ is not correct, so there exists $x \in X$ such that 

$$\Delta(aw) \cdot \eta(x) \neq f(xaw)$$.

> **Lemma:**
$$H_f(X \cup \{ xa \}, Y \cup \{ w \})$$ has full rank.

Since $H_f(X,Y)$ has full rank, it is enough to show that the last column is linearly independent from the others.
Assume towards contradiction that this is not the case, there exists $\lambda \in \R^Y$ such that
$$H_f(X \cup \{ xa \},\{ w \}) = H_f(X \cup \{ xa \}, Y) \cdot \lambda$$.

This implies

$$\begin{array}{ccc}
\Delta(aw) \cdot \eta & = & \Delta(a) \cdot \Delta(w) \cdot \eta \\
& = & \Delta(a) \cdot H_f(Xw, \{ \varepsilon \}) \\
& = & \Delta(a) \cdot H_f(X, \{ w \}) \\
& = & \Delta(a) \cdot H_f(X, Y) \cdot \lambda \\
& = & H_f(Xa, Y) \cdot \lambda
\end{array}$$

Instantiating in $x$ we get $$\Delta(aw) \cdot \eta = f(xaw)$$, a contradiction with $aw$ 
being not correct.

Note that the lemma implies that $xa \notin X$ and $w \notin Y$, so we do maintain that the size of $X$ and $Y$ are equal.
 
> How long does the algorithm run?

Let $n = \rk(H_f)$, we claim that the automaton computed when $X$ and $Y$ have size $n$ does compute $$f$$.
Indeed, if it did not, then we could extend $X$ and $Y$ as the algorithm does, obtaining a submatrix of $H_f$ of rank $n+1$, which is impossible.
It follows that the total number of queries is:
* exactly $n-1$ equivalence queries,
* $O(n^3)$ membership queries.

> Note that the algorithm we presented here is based on the construction of the backward automaton (see the [previous post]({{ '/blog/fliess_theorem' | prepend: site.baseurl }})).
A dual algorithm can be constructed using the forward automaton, which is actually what is done in the [original paper](https://dl.acm.org/citation.cfm?id=337257).


