---
layout:     post
title:      Algorithms for parity games and separation
date:       2017-11-04 9:00:00
author:     Nathana&euml;l Fijalkow
---

<script type="text/javascript"
    src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

The postulate of this post is that three algorithms for parity games are instantiation of the following separation problem:
find a safety regular language $$L$$ such that

> 1. \$$\text{AllEvenCycles } \subseteq L$$
> 2. \$$L \cap \text{ AllOddCycles} = \emptyset$$

<figure>
	<img src="{{ '/images/separation2.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Three algorithms in one picture</figcaption>
</figure>

<!--
[Some link](https://games-automata-play.github.io/)
 ![_config.yml]({{ site.baseurl }}/images/separation2.png) -->

