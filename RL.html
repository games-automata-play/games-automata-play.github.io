---
layout: default
title: Games automata play
---

<div class="home" id="home">
  <h1 class="pageTitle">Reinforcement learning course</h1>

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

The reference book and key inspiration for this course and for reinforcement learning in general is the book by [Sutton and Barto](http://incompleteideas.net/book/the-book.html) (it can be downloaded for free at that address).
The reason for these lecture notes to exist is that we will have a more theoretical focus and we will provide toy code to play with the different algorithms.

**EDIT 26/12/2019**: Look at the [github repository](https://github.com/nathanael-fijalkow/TicTacToe_RL) for implementations of some algorithms solving Tic-Tac-Toe.

**EDIT 01/01/2020**: Look at the [github repository](https://github.com/nathanael-fijalkow/Multi-Armed-Bandits) for implementations and plots of some algorithms for the multi-armed bandits.

The previous two blog posts on [multi-armed bandits]({{ '/blog/armed_bandits_upper_bound' | prepend: site.baseurl }}) and [the Monte Carlo tree search algorithm]({{ '/blog/monte_carlo_tree_search' | prepend: site.baseurl }}) serve as introduction to the topic. 

The rest of the lecture will use Markov decision processes (MDP) as a model.
A Markov decision process is given by a set of states $S$, a set of actions $A$, and a transition function

$$
\Delta : S \times A \to \Dist(S \times \R)
$$

In words: from the state $s$ and playing action $a$, the outcome is drawn according to the distribution $\Delta(s,a)$, it yields a reward $r$ and leads to state $s'$ with probability $\Delta(s,a)(s',r)$.
Sometimes, the next-state and reward distributions are given independently, and actually in many cases the reward is deterministic and not stochastic.

We also specify an initial state $s_0$.

The objective of Markov decision processes is to model decision-making for a controller in a stochastic environment with immediate rewards.
Indeed, at every step a reward is observed (although in many scenarios we will consider the reward will be $0$ at each step except for the terminal step).

The distinctive properties of Markov decision processes is the **Markov property**: the distribution for the next state and the reward depend only on the **current** state and action, and not on the whole history. One commonly say "given the present, the future does not depend on the past", although I'm not sure this formulation helps.

A **play**, or **history**, is a sequence $$(s_0,a_0,r_0,s_1,a_1,r_1,\dots)$$, where $s_0$ is the first state, $a_0$ the first action, $r_0$ the first reward, and so on.

The decisions are represented by **strategies** (also called **policies**). There are five types:
* (general) strategies are functions $$\sigma : (S \times A \times \R)^* \times S \to \Dist(A)$$: given a history and the current state, the strategy chooses a distribution on actions
* **pure** strategies (also called **deterministic**) choose not a distribution on actions but a single action: $$\sigma : (S \times A \times \R)^* \times S \to A$
* **Markovian** strategies depend only on the current state and the time step: $$\sigma : \N \times S \to \Dist(A)$$ 
* **stationary** strategies (also called **positional** or **memoryless**) depend only on the current state: $$\sigma : S \to \Dist(A)$$
* **pure stationary** strategies: $$\sigma : S \to A$$.

It turns out that in most cases the simplest strategies will be enough for our purposes (we will prove this in due course).

An MDP $\M$ together with a strategy $\sigma$ induce a Markov chain, and in particular a probability measure $\P_\sigma$, or $\P_{\sigma,s_0}$ when specifying the initial state $s_0$.
(We will not go into the formal definitions of the Markov chain and the measure here.)
We let $S_i$ denote the random variable denoting the state, $A_i$ the action played, and $R_i$ the reward obtained at the $i$-th step.

Given a strategy $\sigma$ we write $\val_\sigma$ for the value function defined by

$$
\val_\sigma(s) = \E_{\sigma,s} [\sum_i R_i],
$$

meaning the total expected reward obtained while playing $\sigma$ and starting from $s$.
The sum is a priori infinite, hence may not exist in general. 
There are two different solutions to that: 
* we fix some horizon and only look at paths of length the fixed horizon,
* we introduce a discount factor $\gamma \in (0,1)$ we consider the total expected discounted reward:

$$
\E_\sigma [\sum_i \gamma^i R_i]
$$

Since $$\gamma$$ is less than $$1$$, the rewards collected at the beginning are more important than the rewards in a more distant future.

The (algorithmic) goal in **reinforcement learning** is to find a strategy maximising the total expected reward:

$$
\val_*(s_0) = \sup_{\sigma \text{ strategy}} \val_\sigma(s_0)
$$


### Outline

#### Part 1: Dynamic algorithms 

See the corresponding [blog post]({{ '/blog/dynamic_algorithms_MDP' | prepend: site.baseurl }})

#### Part 2: Monte Carlo approaches, temporal differences and off-policy learning

See the corresponding [blog post]({{ '/blog/Monte_Carlo_MDP' | prepend: site.baseurl }})

#### Part 3: Deep reinforcement learning





  <!--
  <ul class="posts noList">
    {% for post in site.categories."Reinforcement learning" %}
      <li>
        <span class="date">{{ post.date | date: '%B %d, %Y' }}</span>
        <h3><a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h3>
        <span class="date">{{ post.author }}</span>
        <p class="description">{% if post.description %}{{ post.description | strip_html | strip_newlines | truncate: 250 }}{% else %}{{ post.content | strip_html | strip_newlines | truncate: 250 }}{% endif %}</p>
      </li>
    {% endfor %}
  </ul>
 Pagination links
  <div class="pagination">
    {% if paginator.previous_page %}
      <a href="{{ paginator.previous_page_path | prepend: site.baseurl }}" class="previous button__outline">Newer Posts</a> 
    {% endif %}
    {% if paginator.next_page %}
      <a href="{{ paginator.next_page_path | prepend: site.baseurl }}" class="next button__outline">Older Posts</a>
    {% endif %}
  </div>
  -->
</div>
