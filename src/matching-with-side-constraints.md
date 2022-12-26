---
title: Matching with some common side constraints
---

The main question is want kind of constraints we can add to matching problem, while maintaining the polynomial time solvability of the problem.

First, we start with the base problem: *bidirected graph capacitated $b$-matching problem*, or *bidirected graph degree constrained subgraph problem*.

$G=(V,E,\chi)$ is a bidirected graph, if $\chi:V\times E\to \{-2,-1,0,1,2\}$ such that for any $e\in E$, $\sum_{v\in V} |\chi(v,e)| \leq 2$.

Let $c_\ell:E\to \Z$ and $c_u:E\to \Z$ be lower and upper bound on the capacity.

Consider the following integer program, where the input are $G=(V,E,\chi),c_\ell,c_u,w:E\to \Z$, $\ell,u:V\to \Z$.

First, let $\deg_x(v)=\sum_{e} x(e) \chi(v,e)$, so this is the degree of $v$ under the vector $x$.

\[
\begin{align}
& \min_{x\in \Z^E} & & \sum_{e} w_e x(e) & \\
& \text{s.t.} & & \ell(v) \leq \deg_x(v) \leq u(v) & \forall v\in V \\
& & & c_\ell(e) \leq  x(e) \leq c_u(e) & \forall e\in E \\
\end{align}
\]

We can also consider the boolean special case, so we avoid the upper and lower capacity bounds.

\[
\begin{align}
& \min_{x\in \mathbb{B}^E} & & \sum_{e} w_e x(e) & \\
& \text{s.t.} & & \ell(v) \leq \deg_x(v) \leq u(v) & \forall v\in V \\
\end{align}
\]


# Capacitated matching

## Degree parity constraint

For some vertices $P\subseteq V$, consider the constraint.
\[
\begin{align}
\deg_x(v) \equiv r_v \pmod 2 &~~~ \forall v\in P
\end{align}
\]

Base problem together with degree parity constraint is in $P$, because this can be reduced to the base problem.

## Laminar degree constraint
Let's extend $\deg_x(U) = \sum_{v\in U} \deg_x(v)$. Consider a laminar family $\mathcal{F}$.

\[
\begin{align}
\ell(U) \leq \deg_x(U) \leq u(U) &~~~~ \forall U\in \mathcal{F}
\end{align}
\]

Base + laminar degree constraint is also in $P$. Again, it is because it can be reduced to the base problem. 

## Laminar cut constraint
Define $f_x(U) = \sum_{e\in \delta(U), v\in U} x(e) \chi(v,e)$. Consider a laminar family $\mathcal{F}_C$.

\[
\begin{align}
\ell(U) \leq f_x(U) \leq u(U) &~~~~ \forall U\in \mathcal{F}_C
\end{align}
\]

Base + laminar cut constraint: I don't know. I suspect it is in P. 

## Laminar parity constraint
Consider a laminar family $\mathcal{P}$, and $r_U\in \{0,1\}$ for each $U\in \mathcal{P}$.

\[
\begin{align}
\deg_x(U) \equiv r_U \pmod 2 &~~~~ \forall U\in \mathcal{P}
\end{align}
\]

Base + laminar parity constraint: don't know

## Edge parity constraint
$P_E$ a subset of edges
\begin{align}
x(e) \equiv r_e \pmod 2 &~~~~ \forall e\in P_E
\end{align}

This constraint is probably NP-hard because [max flow version is NP-hard](https://cstheory.stackexchange.com/questions/51243/maximum-flow-with-parity-requirement-on-certain-edges).

## Edge set parity constraint

Given edge sets $S_1,\ldots,S_k$, and $r_1,\ldots,r_k$, we also require:
\[
\begin{align}
x(S_i) \equiv r_i \pmod 2 &~~~~ \forall i\in [k]
\end{align}
\]

Is this solvable in polynomial time for fixed $k$?

## Edge set cardinality constraint

Given edge sets $S_1,\ldots,S_k$, and $a_1,\ldots,a_k$, and $b_1,\ldots,b_k$ we also require:
\[
\begin{align}
a_i \leq x(S_i) \leq b_i &~~~~ \forall i\in [k]
\end{align}
\]

The special case when $a_i=b_i$ and $k=1$, is the (weighted) exact matching problem [@PapadimitriouY82], which is solvable in randomized polynomial time. 

## Degree set constraint

For each vertex, we have a set $I_v$, such that.
\[
\begin{align}
\deg_x(v) \in I_v &~~~~ \forall v\in V
\end{align}
\]

It is NP-hard if $I_v$ can be arbitrary. However, one can require the gap to be at most 1.
This is sometimes called generalized matching.

## Laminar degree set constraint

There is a laminar family $\mathcal{F}$, such that we have constraints
\[
\begin{align}
\deg_x(U) \in I_U &~~~~ \forall U\in \mathcal{F}
\end{align}
\]

Probably polynomial time solvable if gap is at most 1.

## Paramodular set constraints

Consider we have a paramodular pair $p,b$, that is $p$ is supermodular, $b$ is submodular, and $b(X)-p(Y)\geq b(X-Y)-p(Y-X)$ holds for all $X,Y\subseteq V$.

\[
\begin{align}
p(U)\leq \deg_x(U) \leq b(U) &~~~~ \forall U\subseteq 2^V
\end{align}
\]

## Submodular set constraints
submodular functions $p$ and $b$. 
\[
\begin{align}
p(U)\leq \deg_x(U) \leq b(U) &~~~~ \forall U\subseteq 2^V
\end{align}
\]

## Matroid edge constraints
We have a matroid $\mathcal{M}=(\mathcal{I},E)$.

 - Independence constraint: we need $M\in \mathcal{I}$. 
 - Basis constraint: We need $M\in \mathcal{B}$.

For example, exactly $k$ red edge. 

This problem is NP-hard in general, for example, consider each edge has a color, we can create a matroid where a subset is independent if it does not have two edges of the same color. This would force a rainbow matching! In fact this is a partition matroid! So it is hard even for partition matroids.

## Conflict edges
There is a conflict graph on the edges, so two edges cannot both appear in the matching. Rainbow matching reduces to this. So this is NP-hard.

## Appear together pairs
There are pairs of edges that must appear together. NP-hard. See [this](https://d-nb.info/1197702415/34).

## One of many
There are set of edges $S_1,\ldots,S_k$, such that at least one edge in $S_i$ must be chosen. Probably can be reduced to 3D matching or something. 

## Master slave capacitated matching
a directed graph $D$ such that if $(u,v)\in D$, then if $\deg(u)\leq \deg(v)$. 

Maybe this is also solvable in polynomial time if each component of $D$ has size $2$? 

## Simultaneous assignment constraint

A much stronger generalization exists, this time, we consider simultaneously solve multiple matching problems at the same time, but under the laminar conditions [@Madarasi21]. 

# Uncapacitated matching
This is when we know each edge can either be picked or not. Also, a matched vertex can also be defined (degree at least $1$). So we define $\partial M$ of a set of edges $M$ is the set of vertices with non-zero degree.

## Master slave matching
A directed graph $D$ such that if $(u,v)\in D$, then if $u$ matched, $v$ has to be matched. Solvable in polynomial time if each component of $D$ is size $2$. This is a special case of delta matroid matching. [@KakimuraT14]

## A family of edges incident to $v$
Consider there is a $\mathcal{F}_v$, such that $\delta_x(v) \in \mathcal{F}_v$. 

If each $\mathcal{F}_v$ is a matroid, then this is the matroid matching problem. NP-hard for general matroid. Solvable for linear matroid.

We can even consider when each $\mathcal{F}_v$ is an even delta-matroid? 

## $\delta M$ is a independent set in a matroid. 

This is the matroid matching problem, NP-hard in general.
Solvable in polynomial time for linear matroids (linear matroid parity problem), laminar matroid (hierarchical b-matching).

## $\delta M$ is independent in a $\delta$-matroid
Solvable in polynomial time if $\delta$-matroid based on symmetric matrix.

## Bipartite graph
If there are two matroid, defined on each bipartition, $\partial M\cap A$ and $\partial M \cap B$ are independent. Solvable in polynomial time.

Given a weighted bipartite graph and there are two matroids on each partition class of the vertices, find the largest weight matching such that the matched vertices is an independent set in each of the matroid. 

See the work of Masao Iri and Nobuaki Tomizawa [@IriT76].

The result can be generalized. There is a directed graph, and two disjoint set of vertices $S$ and $T$, find a max weight edge disjoint paths such that the end points forms a matroid in both $S$ and $T$. 

It can be generalized even further, into independent flows. Directed graph with two disjoint set of vertices $S$ and $T$. We require the outgoing flow and incoming flow satisfies a polymatroid constraint.

Both generalizations are by Satoru Fujishige.

It seems most of them are just special case of submodular flows.

https://www.narcis.nl/publication/RecordID/oai:cwi.nl:10159

# Degree Sequence 

For a fixed sequence of vertices $v_1,\ldots,v_n$, we are interested in making sure the degree sequence over them satisfies certain property. 

 - [Lexicographic best matching](https://www.zhihu.com/question/345016360/answer/818692005). Find a matching that minimizes $(\deg_x(v_1),\ldots,\deg_x(v_n))$ in the lexicographical order. Is it possible to find it in $O(nm)$? Currently, this reduces to $O(n)$ calls to perfect matching algorithm. Or $O(1)$ call to max weight perfect matching, where weights have bit length $\Omega(n)$. this zhihu thing seems to be relevant https://www.zhihu.com/question/394684176/answer/1231445284
 - Make sure $(\deg_x(v_1),\ldots,\deg_x(v_n))$ is non-decreasing? I think it is hard, because this seems to be master-slave matching but with a very large component.
