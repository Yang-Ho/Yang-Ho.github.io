---
layout: post
title: "Reader's Guide: Push-Relabel Max Flow"
categories: Post 
tags: [research, RG]
---

[A new approach to the maximum-flow problem](https://dl.acm.org/citation.cfm?id=61051), Goldberg and Tarjan, JACM, 1988.

Sometime ago, a PhD student and I presented a lecture/talk on Goldberg and Tarjan's push-relabel algorithm for maximum flow for the CS Theory Reading Group at NC State.

This is a modified version of the reader's guide/outline I made for the talk. 


# **Push Relabel**

### **Terminology:**

For a given graph $$G(V, E)$$ with source $$s$$ and sink $$t$$:

* The **excess** for a vertex $$v$$, $$e(v)$$ is the net flow into $$v$$.
* The **residual capcity** for an edge $$(v, w)$$, $$r_f(v, w) = c(v, w) - f(v, w)$$.
  * If $$r_f(v, w) > 0$$, then $$(v, w)$$ is a **residual edge**.
  * The **residual graph** for a preflow $$f$$ is defined $$G_f = (V, E_f)$$, where $$V$$ is the vertex set of $$G$$ and $$E_f \subset E$$ is the set of the residual edges.
* A **valid labeling** is a mapping $$d: V \to \mathbb{Z}^+$$, where $$d(s) = n = $$\|$$V$$\|, $$d(t) = 0$$, and $$d(v) \le d(w) + 1 $$ for each residual edge $$(v, w)$$.
* A vertex $$v$$ is **active** if $$v \in V - \{s,t\}$$, $$d(v) < \infty$$ and $$e(v) > 0$$.


### **Preflow:**
The algorithm and associated theorems are based on a construct called the **preflow**:

A preflow $$f$$ is defined as a function on vertex pairs of a graph $$G$$ that satisfies the following constraints:
\begin{equation}
    f(v,w) \le c(v,w) \hspace{1cm} \forall (v,w)\in V \times V
\end{equation}
\begin{equation}
    f(v,w) = -f(w,v) \hspace{1cm} \forall (v,w)\in V \times V
\end{equation}
\begin{equation}
    \sum\limits_{u \in V} f(u,v) \ge 0 \hspace{1cm} \forall v \in V - \{s\}
\end{equation}
where $$c(v,w)$$ is the capacity from $$v$$ to $$w$$, $$s$$ is the source.
Where $$n = |V|$$ and $$m = |E|$$.

### **Algorithm:**
The algorithm is as follows: 
* Initialize a preflow $$f$$ such that every edge leaving $$s$$ is set to its capacity and all other edges are set to 0. 
* Initialize a labeling $$d$$ to be $$n$$ for $$s$$ and $$0$$ otherwise. 
* Perform basic operations until there are no more active vertices. 

There are two basic operations:
1. The Push operation simply pushes excess flow from v to w. 
  * This is done by adding $$\delta = \text{min}(e(v), r_f(v,w))$$ to the flows from v and subtracting from the flows from w. 
  * Note: a push can only be done if $$d(v) \ge d(w) + 1$$. 
2.  To relabel a vertex $$v$$, simply take the min$$\{d(w) + 1|(v,w) \in E_f\}$$. 
  * If $$d(v) < d(w) + 1$$, then a relabeling is required

Below is an example of how the relabelling operation works:
<img src='/images/RG-Flow/relabel.png' alt='Example of relabeling' width="400" caption="test"/>

We can visualize the nodes as a series of buckets and the label of a vertex is the "height" of the vertex.
In order for there to be flow from one bucket to another, the first bucket must be "higher" than the other bucket.
In the above example, we want to push flow from $$B$$ to $$C$$. 
However, $$B$$ is "lower" than $$C$$ so we must raise the height of $$B$$ to just above $$C$$.

### **Proof of Correctness:**
Showing the correctness and termination of the algorithm extends the Ford Fulkerson theorem into the following lemma (Lemma 3.3 in the paper):
**Lemma:** If f is a preflow and d is any valid labeling for f, then the sink t is not reachable from the source s in the residual graph $$G_f$$.
The proof for the lemma is fairly straight forward and utilizes contradiction to produce an invalid labeling.

### **Proof of $$O(n^2m)$$ time:**
The proof of the $$O(n^2m)$$ time is trickier. The general idea is to determine the maximum number of relabeling and push operations that can occur. 
Before we can count operations, there are three properties that need to be shown (Corresponding with Lemmas 3.5 - 3.7 in the paper). 

1. $$s$$ is reachable from any vertex $$v$$ with positive excess in $$G_f$$. 
  * To prove this, use contradiction to show that $$e(v)$$ must be 0. 
2. $$d(v)$$ for any vertex $$v$$, never decreases. 
  * This is fairly straight forward to prove just by using the definition of relabeling. 
3. For any vertex $$v$$, and at any time of the execution, $$d(v) \le 2n - 1$$. 
  * The proof for this relies on property (1) to create a path from $$v$$ to $$s$$ in $$G_f$$: 
    * Since the path has at most length $$n-1$$, $$d(v) \le d(s) + n - 1 = 2n - 1$$.

With these three properties, we can now count the number of operations. 
For relabeling, note that since $$d(v) = 0$$ initially and has a max of $$2n -1$$, there can be at most $$2n -1$$ relabelings for each vertex (excluding $$s$$ and $$t$$) which is $$O(2n^2)$$.

For push operations we consider saturated and unsaturated pushes. 
A push that makes $$r_f(v,w) = 0$$ is $$saturated$$. 

Consider a saturated push from a vertex $$v$$ to $$w$$. 
In order for to push from $$v$$ to $$w$$ again, there must be at least two relabelings (One to allow a push from $$w$$ to $$v$$, and another to allow another push from $$v$$ to $$w$$).
Since both $$d(v)$$ and $$d(w)$$ are bounded by $$2n - 1$$, there can be at most $$2n - 1$$ saturated pushes for the $$vw$$ edge. 
Thus the maximum number of saturated pushes is $$O(2nm)$$. 

Let $$P$$ be the sum of $$d(v)$$ for all active $$v$$.
Since each saturated push can increase $$P$$ by at most $$2n - 1$$, the total increase to $$P$$ due to saturated pushes is $$(2n -1)(2nm)$$. 
Likewise, since the total increase to $$P$$ due to relabeling is less than $$2n - 1$$ for each vertex that is not $$s$$ or $$t$$.
$$P$$ only decreases when there is an unsaturated push from $$v$$ to $$w$$ since $$d(v) > d(w)$$. 
Because $$P$$ must be 0 at the end, there must be a total of $$(2n - 1)2nm + (2n - 1)(n - 2) \le 4n^2m \le 4n^2m$$ unsaturated pushes. 

From this it is clear that the final complexity of the algorithm is $$O(2n^2 + 2nm + 4n^2m) = O(n^2m)$$.

### **Applications**
The algorithm presented can be applied to any max flow problem. In many cases, the better time performance overall makes this specific approach more applicable in situations where time is an issue.

