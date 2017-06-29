---
layout: post
title:  "CS Theory Reading Group: Push-Relabel Max Flow"
categories: Post 
---

Last year, a PhD student and I presented a lecture/talk on Goldberg and Tarjan's push-relabel algorithm for maximum flow.

Below is a modified version of the reader's guide/outline I made for the talk:

# **Maximum Flow Basics**

TODO


# **Push Relabel**

### **Newly Introduced Definitions:**

For a given graph $$G(V, E)$$ with source $$s$$ and sink $$t$$:

* The **excess** for a vertex $$v$$, $$e(v)$$ is the net flow into $$v$$.
* The **residual capcity** for an edge $$(v, w)$$, $$r_f(v, w) = c(v, w) - f(v, w)$$.
  * If $$r_f(v, w) > 0$$, then $$(v, w)$$ is a **residual edge**.
  * The **residual graph** for a preflow $$f$$ is defined $$G_f = (V, E_f)$$, where $$V$$ is the vertex set of $$G$$ and $$E_f \subset E$$ is the set of the residual edges.
* A **valid labeling** is a mapping $$d: V \to \mathbb{Z}^+$$, where $$d(s) = n = $$\|$$V$$\|, $$d(t) = 0$$, and $$d(v) \le d(w) + 1 $$ for each residual edge $$(v, w)$$.
* A vertex $$v$$ is **active** if $$v \in V - \{s,t\}$$, $$d(v) < \infty$$ and $$e(v) > 0$$.


### **Algorithm:**

#### Proof of Completeness:

#### Proof of termination:


\begin{theorem}
    Suppose that the algorithm terminates and all distance labels are finite at termination. Then the preflow f is a maximum flow; that is, the algorithm is correct.
\end{theorem}

A preflow $f$ is defined as a function on vertex pairs of a graph $G$ that satisfies the following constraints:
\begin{equation}
    f(v,w) \le c(v,w) \hspace{1cm} \forall (v,w)\in V \times V
\end{equation}
\begin{equation}
    f(v,w) = -f(w,v) \hspace{1cm} \forall (v,w)\in V \times V
\end{equation}
\begin{equation}
    \sum\limits_{u \in V} f(u,v) \ge 0 \hspace{1cm} \forall v \in V - \{s\}
\end{equation}
where $c(v,w)$ is the capcity from $v$ to $w$, $s$ is the source.

\begin{theorem}
    The generic algorithm terminates after O($n^2$m) basic operations
\end{theorem}
Where $n = |V|$ and $m = |E|$.


// Algorithm
The algorithm is as follows. We begin with a preflow $f$ that is initialized such that every edge leaving $s$ is set to its capacity and all other edges are set to 0. We also initialize the labeling$d$ to be $n$ for $s$ and $0$ otherwise. Once initialized, we then perform basic operations until there are no more active vertices. 

The Push operation simply pushes excess flow from v to w. This is done by adding $\delta = \text{min}(e(v), r_f(v,w))$ to the flows from v and subtracting from the flows from w. Note push can only be done if $d(v) \ge d(w) + 1$. If $d(v) < d(w) + 1$, then a relabeling is required. To relabel a vertex $v$, simply take the min$\{d(w) + 1|(v,w) \in E_f\}$. Figure \ref{fig:relabel} provides an example of how relabeling functions. 

Showing the correctness and termination of the algorithm utilizes the Ford Fulkerson theorem:
\begin{theorem}
    A flow f is maximum if and only if there is no augmenting path; that is t is not reachable from s in $G_f$.
\end{theorem}
and the following lemma:
\begin{lemma}
    If f is a preflow and d is any valid labeling for f, then the sink t is not reachable from the source s in the residual graph $G_f$.
\end{lemma}
The proof for the lemma is fairly straight forward and utilizes contradiction to produce an invalid labeling.

The proof for theorem 1.1 requires a bit more work. The general idea is to simply show the maximum number of relabeling and push operations that can occur. Before we can count operations, there are three properties that need to be shown (Corresponding with Lemmas 3.5 - 3.7). The first is that $s$ is reachable from any vertex $v$ with positive excess in $G_f$. To prove this, use contradiction to show that $e(v)$ must be 0. 
The second property is that $d(v)$ for any vertex $v$, never decreases. This is fairly straight forward to prove just by using the definition of relabeling. 
Finally, the last property is that for any vertex $v$, and at any time of the execution, $d(v) \le 2n - 1$. The proof for this relies on Lemma 3.5 to create a path from $v$ to $s$ in $G_f$. Since the path has at most length $n-1$, $d(v) \le d(s) + n - 1 = 2n - 1$.

With these three properties, we can now count the number of operations. 
For relabeling, note that since $d(v) = 0$ initially and has a max of $2n -1$, there can be at most $2n -1$ relabelings for each vertex (excluding $s$ and $t$) which is $O(2n^2)$.

For push operations we consider saturated and unsaturated pushes. A push that makes $r_f(v,w) = 0$ saturated. For saturated pushes, consider a saturated push from a vertex $v$ to $w$. In order for another push from $v$ to $w$, there must be at least two relabelings (One to allow a push from $w$ to $v$, and another to allow another push from $v$ to $w$). Since both $d(v)$ and $d(w)$ are bounded by $2n - 1$, there can be at most $2n - 1$ saturated pushes for the $(v,w)$ edge. Thus the maximum number of saturated pushes is $O(2nm)$. For unsaturated pushes, let $P = \sum\limits_{v active}d(v)$. Then, for each unsaturated push from $v$ to $w$, $P$ decreases since $d(v) > d(w)$. Since each saturated push can increase $P$ by at most $2n - 1$, the total increase to $P$ due to saturated pushes is $(2n -1)(2nm)$. Likewise, since the total increase to $P$ due to relabeling is less than $2n - 1$ for each vertex that is not $s$ or $t$. Because $P$ must be 0 at the end, there must be a total of $(2n - 1)2nm + (2n - 1)(n - 2) \le 4n^2m \le 4n^2m$. This proves the algorithm terminates after $O(n^2m)$. 

To get a min-cut, simply modify the definition of an active vertex to be any vertex $v$ such that $v \in V - \{s,t\}$, $e(v) > 0$, and $d(v) < n$, and define the cut $S$ such that $\overline{S}$ contains only the vertices that can reach $t$ in $G_f$. 

Additionally, the choice of data structure and implementation techniques can also affect the runtime. 
\section{Applications}
The algorithm presented can be applied to any max flow problem. In many cases, the better time performance overall makes this specific approach more applicaple in situations where time is an issue.
