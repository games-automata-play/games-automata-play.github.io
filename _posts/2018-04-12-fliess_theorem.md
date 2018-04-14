---
layout:     post
title:      Fliess' theorem for minimising weighted automata 
date:       2018-04-12 9:00:00
author:     Nathana&euml;l Fijalkow
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

<p class="intro"><span class="dropcap">W</span>e state and prove Fliess' theorem, which says that the minimal automaton recognising a given formal series has size the rank of its Hankel matrix.</p>

A formal series (here over the reals) is a function $$f : A^* \to \R$$.
The Hankel matrix of $$f$$ is a bi-infinite matrix $$H_f \in \R^{A^* \times A^*}$$ defined by 

$$H_f(u,v) = f(uv)$$

For recognising formal series we use weighted automata: 

$$\A = (Q, \alpha \in \R^Q, (\Delta(a) \in \R^{Q \times Q})_{a \in A}, \eta \in \R^Q)$$.

Such an automaton recognises the function 

$$\A(a_1 \cdots a_n) = \alpha \cdot \underbrace{\Delta(a_1) \cdots \Delta(a_n)}_{\Delta(a_1 \cdots a_n)} \cdot \eta$$

> **Theorem:** (Fliess '74)
* Any automaton recognising $f$ has at least $\rk(H_f)$ many states,
* There exists an automaton recognising $f$ with $\rk(H_f)$ many states.

### First implication: the backward-forward decomposition

Let $\A$ be an automaton recognising $f$.

Define $P_\A \in \R^{A^* \times Q}$ the formal series recognised by the weighted automaton $\A$ with $e_q$ as final vector,
and $S_\A \in \R^{A^* \times Q}$ the formal series recognised by the weighted automaton $\A$ with $e_q$ as initial vector.

We have

$$H_f(u,v) = \sum_{q \in Q} P_\A(u,q) \cdot Q_\A(v,q)$$

Hence we wrote $H_f$ as a sum of $Q$ terms, each of which is a product of a row vector with a column vector, hence has rank $1$.
We conclude by noting that the rank of a sum of matrices is at most the sum of the ranks.

### Converse implication: construction of the minimal automaton

We introduce some notations. For $X,Y$ two subsets of $A^*$, we let $H_f(X,Y)$ denote the submatrix of $H_f$ restricted to rows in $X$ and columns in $Y$.
Observe the following equality: $$H_f(Xa,Y) = H_f(X,aY)$$.

#### The backward automaton

Without loss of generality let us assume that $f \neq 0$.
It follows that $$H_f( \{\varepsilon\}, A^* ) \neq 0$$.
We consider the family of vectors given by rows of $H_f$. Let us consider $X$ indexing a basis of this family; thanks to the previous assumption we can assume $\varepsilon \in X$.

We construct an automaton $$\A$$ as follows. 
* The set of states is $$X$$. 
* The initial vector is $$e_1$$.
* The transition is defined to satisfy $$H_f(X a, A^*) = \Delta(a) \cdot H_f(X, A^*)$$.
* The final vector is $$H_f( X, \{\varepsilon\} )$$.

To see that there exists $\Delta(a)$ such that $$H_f(X a, A^*) = \Delta(a) \cdot H_f(X, A^*)$$,
let $x \in X$. We decompose $H_f(xa,A^*)$ on the basis $$\{H_f(x,A^*) \mid x \in X\}$$:
there exists $$\Delta(a)(x,x') \in \R$$ such that

$$H_f(xa,A^*) = \sum_{x' \in X} \Delta(a)(x,x') \cdot H_f(x',A^*)$$.

We now argue that $\A$ recognises $f$: we show by induction that $$\Delta(w) \cdot \eta = H_f(Xw, \{\varepsilon\})$$.

$$\begin{array}{ccc}
\Delta(aw) \cdot \eta & = & \Delta(a) \cdot \Delta(w) \cdot \eta \\
& = & \Delta(a) \cdot H_f(Xw, \{\varepsilon\}) \\
& = & \Delta(a) \cdot H_f(X, \{w\}) \\
& = & H_f(Xa, \{w\}) \\
& = & H_f(Xwa, \{\varepsilon\})
\end{array}$$

Note that we used the equality $$H_f(X a, w) = \Delta(a) \cdot H_f(X, w)$$ for all words $$w \in A^*$$, which is the projection on the column $$w$$ of the equality defining $$\Delta(a)$$.

This concludes the proof.
For the sake of curiosity, we show that there exists a dual construction, which considers columns instead of rows.

#### The forward automaton

Since $$f \neq 0$$ it follows that $$H_f( A^*, \{\varepsilon\}) \neq 0$$.
We consider the family of vectors given by columns of $H_f$. Let us consider $Y$ indexing a basis of this family; thanks to the previous assumption we can assume $\varepsilon \in Y$.

We construct an automaton $$\A$$ as follows. 
* The set of states is $$Y$$. 
* The initial vector is $$H_f( \{\varepsilon\}, Y )$$.
* The transition is defined to satisfy $$H_f(A^*, a Y) = H_f(A^*, Y) \cdot \Delta(a)$$.
* The final vector is $$e_1$$.

To see that there exists $\Delta(a)$ such that $$H_f(A^*, a Y) = H_f(A^*, Y) \cdot \Delta(a)$$,
let $y \in Y$. We decompose $H_f(A^*, ay)$ on the basis $$\{H_f(A^*,y) \mid y \in Y\}$$:
there exists $$\Delta(a)(y,y') \in \R$$ such that

$$H_f(A^*, ay) = \sum_{y' \in Y} H_f(A^*,y') \cdot \Delta(a)(y,y')$$.

We now argue that $\A$ recognises $f$: we show by induction that $$\alpha \cdot \Delta(w) = H_f( \{\varepsilon\}, wY)$$.

$$\begin{array}{ccc}
\alpha \cdot \Delta(wa) & = & \alpha \cdot \Delta(w) \cdot \Delta(a) \\
& = & H_f(\{\varepsilon\}, wY) \cdot \Delta(a) \\
& = & H_f(\{w\}, Y) \cdot \Delta(a) \\
& = & H_f(\{wa\}, Y) \\
& = & H_f(\{\varepsilon\}, waY)
\end{array}$$

Note that we used the equality $$H_f(w, a Y) = H_f(w, Y) \cdot \Delta(a)$$ for all words $$w \in A^*$$, which is the projection on the row $$w$$ of the equality defining $$\Delta(a)$$.
