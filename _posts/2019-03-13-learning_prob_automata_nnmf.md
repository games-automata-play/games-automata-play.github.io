---
layout:     post
title:      Learning probabilistic automata using non-negative matrix factorisation
date:       2019-03-12 9:00:00
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

<p class="intro"><span class="dropcap">W</span>e discuss an approach to learning probabilistic automata based on non-negative matrix factorisation.</p>

We use the notations from [this post]({{ '/blog/fliess_theorem' | prepend: site.baseurl }}) about weighted automata and the Hankel matrix.
It may be useful to have read [this post]({{ '/blog/angluin_learning' | prepend: site.baseurl }}) about learning weighted automata in Angluin's style, but not technically necessary.

Recall that for a formal series $$f : \Sigma^* \to \R$$, the following theorem is the basis for weighted automata minimisation as well as learning.

> **Theorem:** (Fliess '74)
* Any weighted automaton recognising $f$ has at least $\rk(H_f)$ many states,
* There exists a weighted automaton recognising $f$ with $\rk(H_f)$ many states.

In this post we are interested in the (natural!) subclass of weighted automata called probabilistic automata: 
the weights are interpreted as probabilities, so $$\Delta(a)$$ and $$\alpha$$ are probabilistic matrices, 
meaning each row is a distribution over states (equivalently, is non-negative and has norm $1$), and $$\beta \in \\{0,1\\}^Q$$.


The question we ask here is: if we assume that $f$ is given by a probabilistic automaton, can we learn $f$?
An obvious answer is **YES**: using the algorithm for weighted automata.
The problem is that we will represent $f$ using a weighted automaton, which in many cases will turn out not to be a probabilistic automaton, 
despite our assumption that out there there is a probabilistic automaton representing $f$.
What if we really want a probabilistic automaton for representing $f$?

The blog post lays the theoretical foundations for answering this question making one assumption about $f$, sometimes called **separability**, sometimes **anchoredness**.
The post is loosely inspired by [this paper](http://www.aclweb.org/anthology/Q16-1018) by Stratos, Collins and Hsu, called "Unsupervised Part-Of-Speech Tagging with Anchor Hidden Markov Models".
The technical content is somewhere there, but using very different notations and point of view.

### Non-negative matrix factorisation

We make a small detour through linear algebra. 
There are various equivalent definitions for the rank of a matrix $M \in \R^{n \times m}$: the most useful one in our context is to say that the rank of a matrix $M$ is at most $r$
if $M$ can be factorised into $M = A W$, where $A \in \R^{n \times r}$ and $W \in \R^{r \times m}$. 
In some sense, it says that the information contained in $M$ can be fitted in a space of dimension $r$,
and recovered using linear transformations.
The rows of $M$ are obtained as linear combinations of rows in $W$.
In particular in our application for weighted automata, $A$ maps words to states.

The non-negative matrix factorisation of a matrix $M$ additionally requires that the entries in $A$ and $W$ are non-negative.
The parallel with probabilistic automata is here: we want $A$ to have non-negative entries. 
(The normalisation part, meaning that the sum is $1$, is a no-brainer.)

We write $\NNrk(M)$ for the smallest $r$ for which there exists a non-negative factorisation of $M$.

Unfortunately, computing the non-negative factorisation is a matrix is hard (NP-hard by Vavasis, see [here](https://arxiv.org/abs/0708.4149)), 
which contrasts with the usual definition of rank, where a factorisation can be found in polynomial time.

### Minimising probabilistic automata

Recall that $H_f$ is the Hankel matrix of $f$.

> **Theorem:**
We assume that $f$ is such that each row of $H_f$ is a distribution.
* Any probabilistic automaton recognising $f$ has at least $\NNrk(H_f)$ many states,
* There exists a probabilistic automaton recognising $f$ with $\NNrk(H_f)$ many states.

The proof is essentially the same as Fliess' theorem, being careful about the non-negativity of various elements.

<!--
Since there are some subtleties for the second item, we detail the construction of the probabilistic automaton.
We consider the family of vectors given by rows of $H_f$. Let us consider $X$ indexing a basis of this family; in other words,
every row of $H_f$ is a linear combination with non-negative coefficients of the rows in $X$.
Since the rows are distributions, this linear combination is actually a convex combination.

Unlike in the weighted automaton case, we cannot assume that $\varepsilon \in X$.
However, $H_f(\varepsilon,\Sigma^*)$ is a convex combination of the rows in $X$, hence

$$H_f(\varepsilon,\Sigma^*) = \sum_{x \in X} \alpha(x) H_f(x,\Sigma^*)$$

We construct the probabilistic automaton $$\A$$ as follows. 
* The set of states is $$X$$. 
* The initial vector is $$\alpha$$.
* The transition is defined to satisfy $$H_f(X a, \Sigma^*) = \Delta(a) \cdot H_f(X, \Sigma^*)$$.
* The final vector is **$$1$$**, the constant vector equal to $1$.

To see that there exists $\Delta(a)$ such that $$H_f(X a, \Sigma^*) = \Delta(a) \cdot H_f(X, \Sigma^*)$$,
let $x \in X$. We decompose $H_f(xa,\Sigma^*)$ on the basis $$\{H_f(x,\Sigma^*) \mid x \in X\}$$:
there exists $$\Delta(a)(x,x')$$ non-negative such that

$$H_f(xa,\Sigma^*) = \sum_{x' \in X} \Delta(a)(x,x') \cdot H_f(x',\Sigma^*)$$.

To see that $\A$ recognises $f$ one uses the same induction as for the weighted automaton case.
-->

### The separability assumption

> **Definition:**
We say that a non-negative factorisation $M = AW$ is **separable** if for all columns $q$ of $A$, there exists a row $w$ of $M$
such that $A(w)$ is the indicator vector of $q$, i.e. it is $1$ on coordinate $q$ and $0$ elsewhere.

This condition was introduced by Donoho and Stodden in 2003, who showed that if a matrix $M$ has a non-negative factorisation $M$ which is separable and satisfies some other properties, 
then it is unique up to permutation of the columns of $A$.
Later, Arora, Ge, Kannan, and Moitra showed in [this paper](https://arxiv.org/abs/1111.0952) that there is a polynomial-time algorithm for computing the non-negative factorisation of a matrix only assuming separability.

Another equivalent way of phrasing the separability property is saying that the rows of $W$ are rows of $M$.
In other words, the matrix $W$ is hiding in plain sight: its rows are rows of $M$.
The algorithm is actually very simple: 
* the first step is to construct $W$, which means selecting the rows of $M$ (which will be rows of $W$) to be those which are not convex combinations of other rows of $M$. 
This can easily be done in polynomial time since checking whether a vector belongs to a convex combination of other vectors is linear programming.
* the second step is to construct $A$, which is easily done knowing $M$ and $W$.

Although the first step of the algorithm can indeed be implemented in polynomial time, in practice the following greedy algorithm is often used: 
the set of rows is picked online, adding at each step the row which is the furthest away from the current convex combination of rows.
This algorithm gives an overapproximation of the smallest set of rows, but it is much faster.

### How things fit like a glove

> **Definition:**
We say that a probabilistic automaton is **anchored** if for all states $q$, there exists a word $w$
such that reading the word $w$ leads to the Dirac distribution over $q$.

Things perfectly fit: a probabilistic automaton is anchored if and only if its Hankel matrix is separable!

Now we have all the ingredients: for learning a function $f$ recognised by an anchored probabilistic automaton, we can apply the same technology as for weighted automata using the Hankel matrix,
but replacing rank by non-negative rank. Some more details about this will come in a later blog post.

