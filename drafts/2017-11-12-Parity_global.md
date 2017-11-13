---
layout:     post
title:      Global algorithms for parity games
date:       2017-11-12 9:00:00
author:     Nathana&euml;l Fijalkow
---

<p class="intro"><span class="dropcap">T</span>his post introduces the notion of global algorithms for parity games,
and show that two algorithms for parity games, 
namely the small progress measure algorithm of Jurdzi&#324;ski and the succint progress measure algorithm of Jurdzi&#324;ski and Lazi&#263;, 
are instances of this framework.</p>

See [this post]({{ '/blog/Parity/' | prepend: site.baseurl }}) for the notations on parity games and the associated data structures.

A data structure $\mathcal{D}$ induces a global algorithm for parity games if the following two properties hold:

> 1. If Eve wins the game $G$ with $n$ vertices and $d$ priorities, then there exists a consistent measure $\mu : V \to \mathcal{D}$.
> 2. If $w$ is an odd cycle, then $\mathcal{D}$ rejects $w$.

It follows that solving the parity game is equivalent to finding a consistent measure.

#### In the remainder of this note we explain that:
1. the small progress measure data structure of Jurdzi&#324;ski induces a global algorithm for parity games,
2. the succint progress measure algorithm of Jurdzi&#324;ski and Lazi&#263; induces a global algorithm for parity games,
3. the power counting algorithm of Calude et al induces a global algorithm for parity games.

<!--
The first point is well known, it's due to Julien Bernet, David Janin, and Igor Walukiewicz, in the paper 
[Permissive strategies: from parity games to safety games](www.labri.fr/perso/igw/Papers/igw-perm.ps).
They in particular hint at the separation problem in the conclusion.

The second point has been worked out by Miko&#322;aj Boja&#324;czyk and Wojtek Czerwi&#324;ski in their lecture notes
[An automata toolbox](https://www.mimuw.edu.pl/~bojan/20172018-2/advanced-topics-in-automata-20172018-jezyki-automaty-i-obliczenia-2).

We proceed to the third point.
-->
