#### Part 2: Monte Carlo approach

A Monte Carlo approach is one where information is gathered through random sampling.
The goal is to remove the two assumptions above.

Removing the first assumption means that we do not store in memory the whole set of states, either because it is too large, 
or just because we do not have information about all states. 
Indeed we will focus on what we have seen so far during the simulations.

Removing the second assumption means that we no longer *know* the transition function. What does it mean?
We only assume that we have access to this black-box: given a state $s$ and an action $a$, sample the distribution $\Delta(s,a)$, and output the resulting pair $(s',r)$ of a state and a reward.
Note that by using this black-box a lot of times, we can recover (an approximation of) the transition function.
Of course, we do not want to do this systematically: for instance for most pairs $(s,a)$ we do not need a good approximation of the distribution $\Delta(s,a)$.

We will see how much can be salvaged of the dynamic algorithms using a Monte Carlo approach.

The key limitation in this approach is a new third assumption: episodes have bounded length, also called the finite horizon assumption.
In other words, in every simulation of the MDP we eventually end up in a terminal state.
The reason for this assumption is that the Monte Carlo approach will average over all episodes to update the values.

#### Part 3: Temporal difference

The temporal difference algorithm is another simulation-based algorithm very related to the Monte Carlo approach,
which lifts the assumption that episodes are finite.

The key difference is this:
* the Monte Carlo approach uses full episodes for updating the values
* the temporal difference updates the values in a step-by-step fashion

#### Part 4: Off-policy learning

The two tasks above (evaluation and improvement) suggest that we explore our MDP by constructing better and better strategies.
Although this is a good idea in general, it may be interesting to separate the two tasks, which describes the family of **off-policy learning**:
* one strategy is used for **exploration**, i.e. for finding information on the MDP
* another strategy is used for **exploitation**, i.e. for achieving the best possible expected total payoff

#### Part 5: Deep reinforcement learning

The last assumption to remove is that the set of states is discrete. 
Even if this is true (for instance, in Go, there are just a finite number of configurations), it may be interesting to represent states in a more continuous way.
One reason for that is to construct strategies playing appropriately from states they have never seen, which is bound to happen if the set of states is too large.

We will see how neural networks can be used to represent value functions and how their abilities to generalise beyond training data is crucial for constructing optimal strategies.


















What seems to be a minor technical detail has unexpected consequences.
It is very tempting to solve the improvement task as follows: 
given a value function $v$ we define 

$$
\sigma(s) = \text{argmax}_{a \in A} \sum_{s',r} \Delta(s,a)(s',r) (r + \gamma v(s'))
$$

The problem is that this requires knowing the set of actions and the $\Delta$ function.
In some (most) cases, this knowledge will not be available.

This explains the introduction of the $Q$-values.
A $Q$-value is a function $q : S \times A \to \R$.
Given a strategy $\sigma$ we write $q_\sigma$ for the $Q$-function defined by

$$
q_\sigma(s,a) = \E_{\sigma,s,a} [\sum_i R_i],
$$

meaning the total expected reward obtained while playing $\sigma$ and starting from $s$ with action $a$.

Now the improvement task can be easily solved as follows:

$$
\sigma(s) = \text{argmax}_{a \in A} q(s,a)
$$

Of course there is no free lunch and dealing with $Q$-values will be (slightly) more complicated for the evaluation task.

