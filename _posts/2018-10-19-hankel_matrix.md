---
layout:     post
title:      Fliess' theorem for minimising weighted automata 
date:       2018-10-19 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Weighted automata
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      R: "{\\mathbb{R}}",
      A: "{\\mathcal{A}}",
      Tree: "{\\text{Tree}}",
      Context: "{\\text{Context}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e discuss two extensions of Fliess' theorem about minimising weighted tree automata and weighted context-free grammars.</p>

This post is somehow a follow-up of [this one]({{ '/blog/fliess_theorem' | prepend: site.baseurl }}), but it can be read independently.

The goal is to present two different results, this time without proof (because they are formal and not so instructive).

### Minimising weighted tree automata

We consider tree formal series (here over the reals), i.e. functions $$f : Tree(A) \to \R$$.
Here A is a signature, it contains letters with arities, for instance a(2) and b(0).

A context is a tree over the signature $A \cup \\{ \square(0) \\}$, with the restriction that $\square$ appears exactly once.
We write $\Context(A)$ for the set of contexts.

A context $c$ and a tree $t$ yield a tree $c[t]$ simply by substituting in $c$ the leaf $\square$ by the tree $t$.

The Hankel matrix of $$f$$ is a bi-infinite matrix $$H_f \in \R^{\Context(A) \times \Tree(A)}$$ defined by 

$$H_f(u,v) = f( c[t] )$$

For recognising formal series we use weighted tree automata, which we do not define here but naturally extend the word case.

> **Theorem:** (Bozapalidis and Loscou-Bozapalidou)
* Any weighted tree automaton recognising $f$ has at least $\rk(H_f)$ many states,
* There exists a weighted tree automaton recognising $f$ with $\rk(H_f)$ many states.

### Minimising weighted context-free grammars

A weighted context-free grammar is a context-free grammar (over words) which comes with a weight function on rules, in a very natural way.
Such a grammar defines a function $f : A^* \to \R$. 

We know that the Hankel matrix can be used to characterise functions recognised by weighted automata. 
Unsurprisingly, weighted context-free grammars are more powerful, and the Hankel matrix as defined in this case does not contain enough information.

The idea of [Bailly, Carreras, Luque, and Quattoni](https://www.cs.upc.edu/~aquattoni/AllMyPapers/emnlp_2013.pdf) is to use more information.
We see $f$ as a function $f : (A^* \times A^*) \times A^* \to \R$.
For a weighted context-free grammar $G$, the definition of $f$ is

$$
f((x,y), z) = \sum_{A \text{ terminal of } G} f(S \to x A y) \cdot f(A \to z)
$$

The idea is that we restrict the computations of $x z y$ to those having a cut in $z$.

The Hankel matrix of such a function $$f$$ is a bi-infinite matrix 
$$H_f \in \R^{(A^* \times A^*) \times A^*}$$ defined by 

$$H_f((x,y) , z) = f((x,y), z)$$

> **Theorem:** (Bailly, Carreras, Luque, and Quattoni)
* Any weighted context-free grammar recognising $f$ has at least $\rk(H_f)$ many states,
* There exists a weighted context-free grammar recognising $f$ with $\rk(H_f)$ many states.

