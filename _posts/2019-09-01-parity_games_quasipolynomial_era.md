---
layout:     post
title:      Parity games, the quasipolynomial era
date:       2019-09-01 9:00:00
author:     Nathana&euml;l Fijalkow
category:   Parity games
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
      rk: "{\\text{rank}}",
      NNrk: "{\\text{rank}_+}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">T</span>his post is an overview on my recent research related to the quasipolynomial time algorithms for parity games.
It can serve as a roadmap for various posts on this blog.
</p>

Another piece of material for these results is the [slides for my invited talk at GanDALF](https://nathanael-fijalkow.github.io/Talk/2019-09-03-GanDALF_parity/#/overview_start).

The key underlying combinatorial notion is that of **universal trees**.
**All** quasipolynomial algorithms (and some others) belong to one of three families: separating automata, value iteration, and fixed point.
Each algorithm in these families corresponds to the choice of a universal tree, which dictates its complexity.

<figure>
	<img src="{{ '/images/intro.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>The central role of universal trees</figcaption>
</figure>

Everything there is to know about universal trees can be found in [this post]({{ '/blog/universal_trees' | prepend: site.baseurl }}).

The first family of algorithms, the closest to universal trees, is value iteration algorithms.
It is presented in [this post]({{ '/blog/value_iteration' | prepend: site.baseurl }}), and relies on the fundamental theorem for parity games presented in [this post]({{ '/blog/fundamental_theorem_parity_games' | prepend: site.baseurl }}).
The paper [[F.]](https://arxiv.org/abs/1801.09618) presents the generic value iteration algorithm using universal trees.

The second family of algorithms is described using separating automata.
It is presented in [this post]({{ '/blog/parity_separation' | prepend: site.baseurl }}).
There are two aspects to be discussed. 
First, the separation question can come in two flavours, strong and weak, we refer to [this post]({{ '/blog/separation_strong_weak' | prepend: site.baseurl }}).
Second, the definition of separating automata requires deterministic automata, which does not cover Lehtinen's approach; hence a more suitable notion, extending separating automata,
is **good for small games automata**. There are no blog posts on them (yet?), so we refer to [this paper](https://link.springer.com/chapter/10.1007%2F978-3-030-17127-8_1).

The first equivalence result states that separating automata and universal trees are equivalent. 
The original proof is this result is in [[CDFJLP]](https://arxiv.org/abs/1807.10546).
A conceptually simpler proof introduces and uses the notion of **universal graphs**, discussed in [this post]({{ '/blog/universal_graphs' | prepend: site.baseurl }}).
One of the advantage of this new proof is that it applies to any positionally determined objective, as explained in [this post]({{ '/blog/separation_universal_graphs' | prepend: site.baseurl }}).

Since the notion of universal graphs applies to much more objectives (said differently: universal trees are universal graphs for parity objectives), it opens many perspectives, of constructing new efficient algorithms for other games.
We discuss algorithms using universal graphs for mean payoff in [this post]({{ '/blog/mean_payoff' | prepend: site.baseurl }}).

Unfortunately there are no blog posts yet on the third family of algorithms.

### Other references in the picture

* [[CJKLS]](https://dl.acm.org/citation.cfm?doid=3055399.3055409)
* [[BC]](https://www.mimuw.edu.pl/~bojan/upload/reduced-may-25.pdf)
* [[Lehtinen]](https://dl.acm.org/citation.cfm?doid=3209108.3209115)
* [[CDFJLP]](https://arxiv.org/abs/1807.10546)
* [[FJdKSSW]](https://arxiv.org/abs/1703.01296)
* [[Jurdzi&#324;ski]](https://link.springer.com/chapter/10.1007%2F3-540-46541-3_24)
* [[JL]](https://arxiv.org/abs/1702.05051)
* [[F]](https://arxiv.org/abs/1801.09618)
* [JM] Marcin Jurdzi&#324;ski and Rémi Morvan, ongoing (personal communication)
* [[Zielonka]](https://www.sciencedirect.com/science/article/pii/S0304397598000097?via%3Dihub)
* [[Parys]](https://arxiv.org/abs/1904.12446)
* [[LSW]](https://arxiv.org/abs/1904.11810)


