---
layout:     post
title:      The universality problem for automata with bounded ambiguity 
date:       2018-08-03 10:00:00
author:     Nathana&euml;l Fijalkow and Ritam Raha
category:   Weighted automata
published:  false
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: {
    Macros: {
      N: "{\\mathbb{N}}",
      R: "{\\mathbb{R}}",
      M: "{\\mathcal{M}}",
      A: "{\\mathcal{A}}",
      rank: "{\\text{rank}}",
    }
  }
});
</script>

<p class="intro"><span class="dropcap">W</span>e prove that the universality problem for automata with fixed ambiguity is decidable in polynomial time.</p>

Given an automaton $\M$ on alphabet $\Sigma$ recognising the language $L(\M)$, the universality problem asks whether

$$\forall w \in \Sigma^*, w \in L(\M)$$

The universality problem is PSPACE-complete for the class of all non-deterministic automata.
In this note, we will prove that the problem is decidable in polynomial time if the automaton has fixed ambiguity.
This result was proved in [this paper](https://epubs.siam.org/doi/pdf/10.1137/0214044). 
Our proof is very similar.

Recall that an automaton is $k$-ambiguous if for every word, there are at most $k$ **accepting** runs for $w$.
Note that we consider only accepting runs; there might be more (non-accepting) runs.

### Universality of unambiguous automata

Consider an unambiguous automaton $\M$ with $n$ states.

An interesting property of unambiguous automata is the correspondence between accepted words and accepting runs: to an accepted word corresponds a unique accepting run.
Hence one can see a unambiguous automaton $\M$ as a weighted automaton over the reals which computes the function $f_\M$ such that $f_\M(w) = 1$ if $w \in L$ and $0$ otherwise.
This means that we can use Fliess' theorem (see [this post]({{ '/blog/fliess_theorem' | prepend: site.baseurl }})).

> **Lemma 1:**
If there exists a word $w$ that is not accepted by $\M$, then there exists a word $w^{\prime}$ not accepted by $\M$ such that $|w^{\prime}|\leq n$.

We will prove the lemma by contradiction. 
Let us assume that the shortest word $u$ which is not accepted by $\M$ has size $k > n$. 
We write $u = a_1 a_2 \ldots a_k$ with $a_i \in \Sigma$. 

We look at a submatrix of the Hankel matrix of $f$: in the matrix $H$ the rows are labelled by the prefixes of $u$, and the columns by suffixes of $u$.

By assumption all the diagonal entries of $H$ will be 0 as the word $u$ itself is not accepted and the upper triangular part of the matrix will be all 1, 
since it corresponds to words that are shorter than $u$. 
Henceforth, $H$ will look like the following:

<figure> <img src="{{ '/images/matrix.png' | prepend: site.baseurl }}" alt="">  </figure>

Let, $H^{\prime} = J - H$, where $J$ is the matrix with all 1's. 
Now $H^{\prime}$ is a lower triangular matrix with $k + 1$ non-zero diagonal elements. 
Hence, $\rank(H^{\prime}) = k + 1$. 
Since $J$ has rank $1$, $\rank(H) \ge k > n$.

On the other hand, we have already seen that we can see $\M$ as a weighted automaton,
for which $H$ is a submatrix of its Hankel matrix. 
Thanks to Fliess' theorem this implies $\rank(H) \le \rank(H_f) \le n$, contradiction.

Using Lemma 1 it is easy to conclude that universality for unambiguous automata is in **co-NP**: 
we can just guess a word of length at most $n$ and check that it is not accepted by $\M$ in polynomial time.

> **Theorem:**
Universality for Unambiguous automaton can be checked in polynomial time.

Let $\M= \langle Q, \Sigma,\delta, q_0, q_f \rangle$ with $\|Q\|=n$. Let's define two quantities:  
$T_k=$ No. of $k$-length accepting paths of $M$.  
$T_{\leq k}=$ No. of accepting paths of maximum length $k$.  

Clearly, $T_{\leq k}= \sum_{i=1}^k T_i$. Now, for $i \in \N$, let $\Delta^{i}$ be a $\|Q\| \times \|Q\|$ matrix, such that  $\Delta(q,q^{\prime})$= Number of $i$- length paths from $q$ to $q^{\prime}$. Consider $I=e_{q_0}$ and $F=e_{q_f}$, then $I\cdot \Delta^{i} \cdot F$ denotes the number of $i$-length accepting runs in $M$.  

 Hence, $T_k= I\cdot \Delta^{k} \cdot F$.  

 Now, $\Delta^{1}$ is fixed for a given automaton. It  can be computed from the automaton itself by filling up the matrix in $O(n^2)$ time directly from the structure of the automaton. Now, the claim is, $\Delta^{k}= \Delta^{k-1}\cdot \Delta^{1}$.  

 $\Delta^{i}(q,q^{\prime})$ denotes the number of $i$ length paths from $q$ to $q ^{\prime}$. Now, every $i$ length path from $q$ to $q^{\prime}$ can be divided into a $(i-1)$ length path from $q$ to some state $x$ and an $1$-length path from $x$ to $q^{\prime}$. Hence, $\Delta^{i}(q,q^{\prime})=\sum_{x \in Q}\Delta^{i-1}(q,x)\cdot \Delta^{1}(x,q^{\prime}) \Rightarrow \Delta^{i}=\Delta^{i-1}\cdot \Delta^{1}$.  

Thus, $(\Delta^i)_i$ is a linear recurrence system and as $\Delta_1$ can be computed polynomially, $\Delta^{i}$ also can be computed polynomially for all $i$ as it is just a series of matrix multiplications.  

From the above result we get two equations: $T_{\leq k}= T_{\leq k-1} + T_k$ and $T_k = T_{k-1}\cdot \Delta_1$. Note that, we have not used unambiguity yet. Hence, the above equations are true for any automaton.  

Now, we will use that $M$ is unambiguous, hence every run corresponds to one word. Hence, $T_{\leq k}$ will denote all the words of maximum length $k$ accepted by $\M$. Now, as $\M$ is unambiguous, we can use *Lemma 1*. Hence to check universality, it is enough to check that if $\M$ accepts all words of length up to $n$. The number of all words of length at most $n$ is $(\|\Sigma\|^{n+1}-1)/(\|\Sigma\|-1)$. Hence, we have to just check $T_{\leq n} = (\|\Sigma\|^{n+1}-1)/(\|\Sigma\|-1)$ or not and it can be done polynomially.

### Universality of finite ambiguous automaton

Now, consider a finite ambiguous NFA $\A=\langle Q, \Sigma,\delta, q_0, q_f \rangle$ with ambiguity $k \in \N$.

> **Theorem:**  
Universality for finite ambiguous automaton can also be checked in polynomial time.

Note that, similar approach as we did for unambiguous automata fails here! The number of accepting paths of some fixed length do not correspond to the number of accepting words of that length any more.  

Consider any linear ordering $<$ on $Q$. For any natural number $p \leq k$, we construct an automaton $A_p= \langle Q^{\prime}, \Sigma, \delta^{\prime},q_0,{q_f}^{\prime}\rangle$ as follows:  

**States:** $Q^{\prime}=Q \cup Q^2 \cup \cdots \cup Q^p$ separated with atmost $(p-1)$ delimiters,  

**Transitions:** if for some state $q \in Q$, $q \xrightarrow[]{a}q_1$ \& $q \xrightarrow[]{a}q_2$ $\in \delta$ and $q_1 < q_2$, then $q \xrightarrow[]{a}(q_1\|q_2) \in \delta^{\prime}$  

**Final state:** Final states of $A_p$ will be $(\underbrace{q_f\|q_f\|\cdots\|q_f}_{p \text{ times}})$  

Notice that, because of the linear order, some word reaches one final state in $A_p$ in exactly one path. Let $w$ ends in some accepting states in $A_p$. Intuitively we can see that, each group in the final states separated by two delimiters uniquely encodes one run of $w$ on $A$. By construction, as the final states of $A_p$ have $(p-1)$ delimiters i.e. $p$ such groups, it accepts all the words, that have at least $p$ accepting runs on $A$.  
Now, consider $A_k$, where $k$ is the highest ambiguity. It accepts any word which has exactly $k$ accepting runs on $A$ and will end up in exactly one final state uniquely encoding $k$-runs of the word. As, the word can reach the above said final state in exactly one path, $A_k$ is unambiguous.  

Now, consider $A_l$ for any $l < k$. It accepts all the words that have at least $l$-accepting runs on $A$. Clearly, from the construction all the words $w$ having $l$-accepting runs will have exactly one accepting run on $A_l$. Now, consider a word $u$ having $i$ accepting runs on $A$, where $l< i < k$. Consider $\rho_1, \rho_2, \ldots, \rho_i$ denotes the accepting states of $w$ on $A$, denoting $i$  different accepting runs. Choose any $l$ states among these maintaining the linear order, say $\rho^{\prime}_1, \rho^{\prime}_2, \ldots, \rho^{\prime}_l$. Then, by construction $u$ will have a unique accepting run on $A_l$ ending at state $(\rho^{\prime}_1\|\rho^{\prime}_2\| \ldots\| \rho^{\prime}_l)$. Hence, $u$ has $i\choose l$ accepting runs on $A_l$.  

Let, $\alpha(l)$ denotes the number of words of length $l$ accepted by $A$ and  
$\alpha(l,j)$ denotes the number of words among them which are accepted by exactly $j$ runs on $A$. Our aim is to prove that, $$ (\alpha(l))_l $$ is an LRS of order polynomial in $n$. Then, it will be enough to check the universality for those first poly($n$) terms.    

Clearly, $\alpha(l) = \sum_{j=1}^k \alpha(l,j)$. Now, recall $T_l(M)$ denotes the number of $l$- length accepting paths on $M$. From the previous explanation we can construct the following set of equations:  

$$
\begin{split}
T_l(A_k)&=\alpha(l,k)\\
T_l(A_{k-1}) &= \alpha(l,k-1)+ {k\choose k-1} \alpha(l,k)\\
T_l(A_{k-2}) &= \alpha(l,k-2)+ {k-1 \choose k-2}\alpha(l,k-1) + {k \choose k-2}\alpha(l,k)\\
&\vdots\\
T_l(A_1) &= \cdots\\
\end{split}$$  


Previously, we have seen that, $$ (T_l(\M))_{l} $$ is a linear recurrence sequence for any automaton $\M$. Hence, for each $k$, $$ (\alpha(l,k))_l $$ is also a linear recurrence sequence of order poly($n$) as the coefficients in the previous set of equations are only binomials in $k$. From this we can infer that, $$ (\alpha(l))_l $$ is a linear recurrence sequence of order poly($n$) also.  

Hence, the automaton will be universal if $$ \alpha(l)= \Sigma^{l} $$ for first poly($n$) terms. Calculating $$ \alpha(l) $$ can be done in polynomial time as we can calculate $$ T_{l} $$ in polynomial time. Hence, the whole checking process is polynomial.
