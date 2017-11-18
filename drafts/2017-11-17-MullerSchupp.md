---
layout:     post
title:      Muller and Schupp proof of positional determinacy for parity games
date:       2017-11-10 9:00:00
author:     Nathana&euml;l Fijalkow
---


We fix $\G$ a game with finite (out-)degree, $v_0$ a starting vertex, $\Omega : V \to \colors$ a colouring function,
and $\sigma$ a winning strategy for Eve, \textit{i.e.} ensuring $\Parity(\Omega)$.

The crux in a few sentences: assume we have a winning strategy for parity, we want to construct a positional one. The problem is that the strategy we start from may perform stupid moves at various times during the game. We define a preference order on plays, which induces the notion of the worst possible history when reaching a vertex. The positional strategy plays assuming this worst case; if the preference order is well chosen, this forces the strategy to play a good move.
\subsection{Ordering histories}

A history is a finite or infinite word over $\colors$,
we denote by $\colors^\infty = \colors^* \cup \colors^\omega$ the set of histories.
It is equipped with the Hamming distance, inducing a notion of limit for sequences of histories.

The parity condition is satisfied by infinite histories such that the \textit{maximal} color appearing infinitely
often is even.

For a history $p$, we denote by $\tr_c(p)$ the suffix of $p$ that starts after the last occurrence of $c$.
The infix from position $n$ to position $n'$ is denoted by $p[n,\ldots,n']$.

We will consider sets of natural numbers, ordered by $\lex$ defined as follows:
for two sets $X,Y \subseteq \N$, 
we say that $X \lexstrict Y$ if there exists $n \in \N$ such that
$n \in Y \setminus X$ and $X \cap \set{0,\ldots,n-1} = Y \cap \set{0,\ldots,n-1}$.
The set of sets of natural numbers is denoted by $\P(\N)$,
and is equipped with the Hamming distance.
For a history $p$, define $\xi_c(p)$ the set of positions for which $p$ has value $c$.

We define the order $\preceq$ over for finite histories;
intuitively speaking, $p \preceq q$ reads ``$p$ is worse than $q$ with respect to $\Parity(\Omega)$''.
We first define $\preceq_d$ by induction on the maximal color $d$:
\begin{enumerate}
	\item If $d = 0$, then $p \preceq_d q$ if $|p| \le |q|$.
	\item If $d$ is even and different from $0$, then $p \preceq_d q$ if 
$$\left\{
\begin{array}{ll}
		  & \max(\xi_d(p)) < \max(\xi_d(q))\\
\text{or}\quad & \max(\xi_d(p)) = \max(\xi_d(q)) \text{ and } \tr_d(p) \preceq_{d-1} \tr_d(q)
\end{array}
\right.$$
	\item If $d$ is odd, then $p \preceq_d q$ if 
$$\left\{
\begin{array}{ll}
		  & \xi_d(q) \lexstrict \xi_d(p)\\
\text{or}\quad & \xi_d(q) = \xi_d(p) \text{ and } \tr_d(p) \preceq_{d-1} \tr_d(q)
\end{array}
\right.$$
\end{enumerate}
Now define $\preceq$ as $\preceq_d$.

\begin{fact}\label{fact}
If $p \preceq q$, then for all $r$ we have $p \cdot r \preceq q \cdot r$.
\end{fact}

\subsection{Defining a positional strategy for Eve}

A play is a finite or infinite word over $V$, which naturally induces a history.
From now on, we will always assume that plays start in $v_0$.
Also, we identify a play and its induced history.
For instance, for $\pi,\pi'$ two finite plays, we say that $\pi \preceq \pi'$ if $c(\pi) \preceq c(\pi')$ holds,
where $c(\pi)$ and $c(\pi')$ are the histories induced by $\pi$ and $\pi'$.

In the following lemma, we make use of the finite-degree assumption.

\begin{lemma}\label{lem:compactness}
Let $(p_n)\nN$ be a sequence of finite plays.
Then it contains a converging subsequence.
\end{lemma}

We define a positional strategy $\sigma'$, on all vertices reachable by $\sigma$.
The definition is by induction on the rank, but for it to make sense, 
we need to ensure the following property:
for all vertices $v$ reachable by $\sigma'$, there exists a play ending in $v$
consistent with both $\sigma$ and $\sigma'$.

Assume that $\sigma'$ has been defined on all vertices of rank $k$,
then for all vertices $v \in \VE$ of rank $k+1$ reachable by $\sigma'$,
define $\sigma'(v) = \sigma(\pi_v)$,
where $\pi_v$ is a finite play ending in $v$ and consistent with both $\sigma$ and $\sigma'$,
minimal with respect to $\preceq$ among all such plays.
The property is clearly preserved by this construction.

\begin{theorem}
The strategy $\sigma'$ ensures $\Parity(\Omega)$.
\end{theorem}

\begin{proof}
Consider a play $\pi = v_0 v_1 \ldots$ consistent with $\sigma'$.

For the sake of readability, denote $\pi_n = \pi_{v_n}$.
We first observe that $\pi_{n+1} \preceq \pi_n \cdot v_{n+1}$ for all $n$.
Indeed, by definition of $\sigma'$, the play $\pi_n \cdot v_{n+1}$ is consistent with both $\sigma$ and $\sigma'$.
Since the play $\pi_{n+1}$ is the worst play ending in $v_{n+1}$ consistent with both $\sigma$
and $\sigma'$, it is smaller than $\pi_n \cdot v_{n+1}$ with respect to $\preceq$.

Using this inequality, an easy induction shows that $\pi_{n'} \preceq \pi_n \cdot \pi[n+1,\ldots,n']$
for all $n < n'$, and also $\pi_n \preceq \pi[0,\ldots,n]$. 
Here we rely on the first item of Fact~\ref{fact}.

Thanks to Lemma~\ref{lem:compactness},
the sequence $(\pi_n)\nN$ contains a subsequence indexed by $I \subseteq \N$
which converges to an infinite play, denoted by $\pi_\infty$.
Observe that since for all $n \in I$, the finite play $\pi_n$ is consistent with $\sigma$,
the limit $\pi_\infty$ is consistent with $\sigma$ hence satisfies $\Parity(\Omega)$.

We conclude using Lemma~\ref{lem:comparaison}.
\hfill\qed
\end{proof}

\begin{lemma}\label{lem:comparaison}
Let $\pi = v_0 v_1 \ldots$ be an infinite play and $(\pi_n)\nN$ be a sequence of finite plays, such that:
\begin{enumerate}
	\item $(\pi_n)\nN$ converges to an infinite play $\pi_\infty$,
	\item $\pi_\infty$ satisfies $\Parity(\Omega)$,
	\item for all $n$, $\pi_{n+1} \preceq \pi_n \cdot \pi[|\pi_n+1|,\ldots,|\pi_{n+1}|]$.
\end{enumerate}
Then $\pi$ satisfies $\Parity(\Omega)$.
\end{lemma}

\begin{proof}
We proceed by induction on the maximal color $d$.
The case $d = 0$ is easy.

\textbf{First case:} $d$ is even and different from $0$.
There are two cases: 
\begin{enumerate}
	\item Either $d$ appears infinitely many times in $\pi_\infty$.
We show the following property: 
\begin{framed}
\noindent For every $k$ such that $\Omega(\pi_\infty)(k) = d$,
there exists $k' \ge k$ such that $\Omega(\pi)(k') = d$.
\end{framed}

Consider such a $k$ and $\pi_n$ that coincides with $\pi_\infty$ on the first $k$ positions.
Since $\pi_n \preceq \pi[0,\ldots,n]$, by definition
$\max(\xi_d(\pi_n)) \le \max(\xi_d(\pi[0,\ldots,n]))$.
Observe that $\max(\xi_d(\pi_n)) \ge k$ since $k \in \xi_d(\pi_n)$,
so $\max(\xi_d(\pi[0,\ldots,n])) \ge k$, and the conclusion follows.

It follows that $d$ appears infinitely many times in $\pi$,
so $\pi$ satisfies $\Parity(\Omega)$.

	\item Or $d$ appears finitely many times in $\pi_\infty$.
We conclude by induction hypothesis, considering the sequence $(\tr_d(\pi_n))\nN$.
\end{enumerate}

\textbf{Second case:} $d$ is odd.
We show the following property: 
\begin{framed}
\noindent For every $k$ such that $\Omega(\pi_\infty(k)) = d$,
there exists $k' \ge k$ such that $\Omega(\pi(k')) = d$.
\end{framed}
Let $n$ be a position and choose $k > n$ such that $\Omega(\pi(k)) = d$,
we argue that there exists $k' > n$ such that $\Omega(\pi_\infty(k')) = d$.

Let $n_0,n_1$ such that $\pi_{n_0}$ coincides with $\pi_\infty$ on the first $n$ positions,
and $\pi_{n_1}$ that coincides with $\pi_\infty$ on the first $k$ positions.
Without loss of generality, $n < n_0 < k < n_1$.
We have $\pi_{n_1} \preceq \pi_{n_0} \cdot \pi[n_0+1,\ldots,n_1]$,
so 
\[
\xi_d(\pi_{n_0} \cdot \pi[n_0+1,\ldots,n_1]) \lex \xi_d(\pi_{n_1}).
\]
Since $\pi_{n_0}$ and $\pi_{n_1}$ both coincide with $\pi_\infty$ on the first $n$ positions,
there are equal up to this position.
Since $n_0 < k$ and $\Omega(\pi(k)) = d$, there exists $k'\le k$ such that $\Omega(\pi_{n_1}(k')) = d$.
Since $\pi_{n_1}$ coincide with $\pi_\infty$ on the first $k$ positions, 
it follows that $\Omega(\pi_\infty(k')) = d$.

Since $d$ appears finitely many times in $\pi$,
it implies that $d$ appears finitely many times in $\pi_\infty$.
As above, we conclude by induction hypothesis, considering the sequence $(\tr_d(\pi_n))\nN$.
\hfill\qed
\end{proof}

\end{document}