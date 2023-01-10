---
title: "Markov Chains and the Perron-Frobenius theorem (Part 1/2)"
category: Math
toc: true
tags: 
    - linear-algebra 
    - pagerank
    - Perron-Frobenius
excerpt: "A neat result in Linear Algebra"
---
{% katexmm %}
# Disclaimer
Much of the content of this post is based off content from 21-242 at CMU as taught by [Clinton Conley](https://www.math.cmu.edu/~clintonc/).
Consequently, I claim no credit for any of the ideas presented below, but (as explained below), I think that this approach in particular is worth sharing because of the very elementary ideas it builds upon.
# About
A well-known result in linear algebra is the [Perron-Frobenius theorem](https://en.wikipedia.org/wiki/Perron%E2%80%93Frobenius_theorem), which implies that (among other results) every **stochastic matrix** admits a **stochastic eigenvector** with eigenvalue one (definitions of those terms to follow later). 
The latter result finds a variety of uses in the study of probability (in particular, in Markov analysis), but most proofs (at least, those easily Googleable) appeal to unnecessarily advanced results like compactness and a variety of different fixed points theorems to prove this result. 
Since we're ultimately dealing with *linear* operators, however, such analysis seems unnecessary. 
In this post, we'll be going over the background needed for this result, ultimately proving it using nothing more advanced than the triangle inequality and basic linear algebra. 
With that said then, we'll start by going over relevant definitions and motivations for this problem.

# Motivation
Suppose we're modeling some system that can be fully encapsulated by some finite set of states $S = \{S_1, S_2 \cdots, S_n,\}$.
For the purposes of imagination, we might think of such states as a collection of lily pads that a frog might be jumping to, a set of stations that a train may be visiting, a list of tourist destinations you might want to travel to, or, in the case of Google's PageRank, a set of websites you might be browsing.
We'll view all possible changes in our system as changes between these given states, and we'll also make the crucial assumption of a **discrete time step**; that is, we have $t = 0, 1, 2, \cdots$ as all possible times we can observe our system,and that there is some $s \in \{S_1, S_2 \cdots, S_n,\}$ corresponding to each timestep.
Effectively, what we're saying here is that we're only interested in *times where our system is in one of our enumerated states*, with our timestep representing opportunities to *move between states*.
We also make the assumption that these state transitions are *probabilistic* and *time-independent*, meaning that at any state $S_i$ and time $t_1$, one will be at state $S_j$ at time $t_2 = t_1 +1$ with a probability $p \in [0,1]$ that is purely a function of $S_i$ and $S_j$.
This assumption means that, for example, if you're on the front page of reddit, you're equally as likely to visit a particular linked article you see regardless of how many times you've read it before.
With these assumptions in mind, then, let's move onto the problem of how to mathematically model such a system.

We note that to fully encapsulate the **properties** of such a system, it suffices to capture the transition probabilities between any pair of states. 
To do this, we'll use a $n \times n$ matrix $A$ where $A_{i,j}$ represents the probability of being in state $i$ one time step after being in state $j$. Since probabilities are always positive, and we insist that our system is always in some state, we naturally impose the constraint that $A_{i,j} \ge 0$ and that for all $j \in [n],  \sum_{i \in [n]} A_{i,j} = 1$.
We call such a matrix satisfying these properties **stochastic**.

Given our definition of stochastic matrices, the idea of a **stochastic vector** naturally follows. We'll define such vectors to be any valid column vectors of a stochastic matrix. More formally, this means any vector $\overrightarrow{v} \in \mathbb{R}^n = \{v_1, v_2, \cdots, v_n \}$ satisfying that  $\sum_{i \in [n]} v_i = 1$. 
Whereas stochastic matrices serve to model the transition behaviors of our entire system, stochastic vectors serve the purpose of modeling probabilities for a single state or point in time, with $\overrightarrow{v}_i$ corresponding to the probability that one is in state $i$ at some given time. Effectively, these vectors encode a **discrete probability distribution**. Supposing that $\overrightarrow{v}$ specifies such probabilities at $t = i$, it follows fairly quickly from the definition of matrix multiplication that $A\overrightarrow{v}$ specifies the same probabilities at $t = i+1$.
Given these definitions, we're now ready to begin looking at various properties of these objects.

# Properties of Transition Matrices
Firstly, we'll check some basic intuitions about how these two objects should interact with each other.
## Stability under vector multiplication
As noted above, multiplication by $A$ effectively corresponds to taking a single timestep forward.
Given that an original vector $\overrightarrow{v}$ encodes a probability distribution, then, $A\overrightarrow{v}$ naturally should as well, and this is something we can check explicitly.
More formally, suppose that $A$ is a stochastic matrix and that $\overrightarrow{v}$ is stochastic. We'll now show that $A\overrightarrow{v}$ is stochastic.
Let 
$$
    \overrightarrow{v} = 
    \begin{pmatrix}
        a_1 \\ a_2 \\ \cdots \\ a_n
    \end{pmatrix}
$$	
By the definition of a stochastic vector, we know that $\sum \limits_{i=1}^n a_i = 1$. \\
Consider the representation of $A$ as a series of column vectors; that is,
$$
    A =
    \begin{pmatrix}
        \mid  & \mid  &        & \mid  \\
        \overrightarrow{w}_1 & \overrightarrow{w}_2 & \cdots & \overrightarrow{w}_n \\
        \mid  & \mid  &        & \mid  \\
    \end{pmatrix}
$$
By definition, we know that all such $\overrightarrow{w}_i$ are stochastic, which means that $\sum \limits_{j = 1}^n (\overrightarrow{w}_i)_j = 1$ for all $i$.
By the definition of matrix multiplication,
$$
    A\overrightarrow{v} = \sum \limits_{i=1}^{n} a_i \overrightarrow{w}_i
$$
Since every coordinate of $A\overrightarrow{v}$ is the sum of the product of non-negative reals, each coordinate is a non-negative real.
Furthermore, the sum of all the coordinates of $A\overrightarrow{v}$ is
$$
    \sum \limits_{i=1}^n a_i \sum_{j = 1}^{n} (\overrightarrow{w}_i)_j = \sum \limits_{i=1}^n a_i (1) = 1
$$
so $A\overrightarrow{v}$ is stochastic.

## Stability under matrix multiplication
Since $A$ corresponds to a single step forward of unit length, it naturally follows that powers of $A$ correspond to taking multiple timesteps forward simultaneously.
To check this basic intuition, we prove the following, more general property:
Suppose that $A$ and $B$ are two stochastic $(n \times n)$-matrices.  Show that $AB$ is also stochastic.
Consider the representation of $B$ as a series of column vectors; that is,
$$
B =
    \begin{pmatrix}
        \mid  & \mid  &        & \mid  \\
        \overrightarrow{v}_1 & \overrightarrow{v}_2 & \cdots & \overrightarrow{v}_n \\
        \mid  & \mid  &        & \mid  \\
    \end{pmatrix}
$$
By definition, we know that all such $\overrightarrow{v}_i$ are stochastic.
Then 
$$
AB =
    \begin{pmatrix}
        \mid  & \mid  &        & \mid  \\
        A\overrightarrow{v}_1 & A\overrightarrow{v}_2 & \cdots & A\overrightarrow{v}_n \\
        \mid  & \mid  &        & \mid  \\
    \end{pmatrix}
$$
By the previous result, we know that all such $A \overrightarrow{v}_i$ are stochastic. Thus, by definition, $AB$ is stochastic.

# Main Result
With these properties out of the way, we move onto our main result, which we state as follows

## Theorem
For any stochastic matrix $A$, there exists a stochastic eigenvector $\overrightarrow{v}$ of $A$ with eigenvalue $1$; that is $A\overrightarrow{v} = \overrightarrow{v}$.

A more intuitive way of stating this result is saying that any such system has a "steady state" or fixed point, with the probability distribution represented by this eigenvector remaining fixed under iteration through the system.
Using results that we'll be proving in the second part of this post, it can actually be shown that **any** initial probability distribution will tend to such a steady state as $t \to \infty$, meaning that for sufficiently large $t$, the probability distribution will be effectively independent of $t$.

At this point, a relatively overkill but functional proof of this theorem would be as follows.
Note that the set of stochastic vectors in $\mathbb{R}^n$ is simply the convex hull of the $n$ standard basis vectors and is hence compact. 
Furthermore, by [stability under vector multiplication](#stability-under-vector-multiplication), $A$ (as a linear operator) sends this set to itself, and it clearly continuous. 
Then we can apply [the Brouwer fixed point theorem](https://en.wikipedia.org/wiki/Brouwer_fixed-point_theorem) to conclude that $A$ has a fixed point.
However, as already mentioned, this is a bit overkill, so let's look at a different approach.

## Proof
We first show that there exists some eigenvector with eigenvalue one.

Recall that, to do so, it suffices to demonstrate that $A-I$ is not invertible.

We now consider the representation of $A$ as a series of column vectors; that is,
$$
A =
    \begin{pmatrix}
        \mid  & \mid  &        & \mid  \\
        \overrightarrow{w}_1 & \overrightarrow{w}_2 & \cdots & \overrightarrow{w}_n \\
        \mid  & \mid  &        & \mid  \\
    \end{pmatrix}
$$
By definition, we know that all such $\overrightarrow{w}_i$ are stochastic, which means that $\sum \limits_{j = 1}^n \overrightarrow{w}_i = 1$ for all $i$.

Now let
$$
A- I = 
    \begin{pmatrix}
        \mid  & \mid  &        & \mid  \\
        \overrightarrow{w}'_1 & \overrightarrow{w}'_2 & \cdots & \overrightarrow{w}'_n \\
        \mid  & \mid  &        & \mid  \\
    \end{pmatrix}
$$
where 
$$
(\overrightarrow{w}'_i)_j = 
\begin{cases}
    \overrightarrow{w}_j -1 & j = i \\
    \overrightarrow{w}_j & \text{otherwise}
\end{cases}
$$
Note that every column of this new matrix has the property that $\sum \limits_{j = 1}^n (\overrightarrow{w}'_i)_j = 0$. 
To demonstrate that this new matrix is not invertible, it suffices to show that there is a non-trivial linear combination of it's rows that equals the zero vector. 
Given the above equation, we may simply take the linear combination of the rows of the matrix with all coefficients equal to 1. 

This vector will be
$$	
\begin{pmatrix}
    \sum \limits_{j = 1}^n (\overrightarrow{w}'_1)_j \\
    \sum \limits_{j = 1}^n (\overrightarrow{w}'_2)_j \\
    \cdots \\
    \sum \limits_{j = 1}^n (\overrightarrow{w}'_n)_j  \\
\end{pmatrix}
= \overrightarrow{0}
$$
by the result above. Since $1 \not = 0$, $A-I$ is non-invertible, and we know that any stochastic matrix $A$ has a eigenvalue of 1. 

Now all that we need to show is that there is some stochastic eigenvector associated with this eigenvalue.

Initially, one might think that since any eigenvector can be normalized such that the sum of it's components is equal to 1, existence of such a eigenvector should be trivial. 
However, this observation in of itself both is not completely true (as a simple counterexample, consider the vector $\begin{pmatrix} 1 \\ -1 \end{pmatrix}$), and also does not sufficiently complete the problem. 
However, it is sufficient to prove that some eigenvector exists that has only non-negative entries, as we can then apply the aforementioned observation.

In some attempt to provide intuition for our approach, we note that it suffices to be able to prove that, given **some** eigenvector with entries of potentially mixed signs, we may construct an eigenvector with all positive or negative (for convenience, we'll call such vectors single-signed).

To do so, we define a unique decomposition of any vector into two single-signed vectors, then prove that both of these vectors are also eigenvectors. In some sense, the decomposition we use is the "obvious one" (taking one vector to be all the positive entries, and the other to be all the negative entries), but the property of **uniqueness** plays a crucial role in our argument.
Consequently, we recommend you take some time to consider how one might formalize these properties(and how such a decomposition might lead to our desired result) prior to reading on.

With that said, we'll first begin by demonstrating uniqueness and validity in one dimension, which proves to be pretty trivial.

## Lemma 1
Fix some real $c$, and consider $(a,b)$ such that $a - b = c$ and $a, b \ge 0$. 
Then there is a unique solution $(a,b)$ that minimizes $a+b$.
Furthermore, this minimum is exactly $|c|$, and is achieved when one of $a$ or $b$ is equal to $|c|$, and the other is zero.
## Proof (Lemma 1)
From the triangle inequality, we know that 
$|c| = |a - b| \le |a| + |b| = a+b$.
Thus, the minimum value of $a + b$ is necessarily $|c|$.
Furthermore, we know that equality holds if and only if $a$ and $-b$ have the same sign.
However, since $a$ is non-negative, and $-b$ is non-positive, then this is only true when one of them is zero. 
Based on the sign of $c$, this uniquely determines the solution $(a,b)$ as mentioned above.

Given this result involving non-negative reals, we find a natural extension to non-negative vectors.
## Lemma 2
Denote by $|\overrightarrow{v}|$ to be the $L_1$ norm of $\overrightarrow{v}$ (that is, $|\begin{pmatrix} v_1 \\ v_2 \\ \cdots \\ v_n \end{pmatrix}| = |v_1| + |v_2| + \cdots |v_n|$).
Any vector $\overrightarrow{v}$ has a unique decomposition of the form $\overrightarrow{v} = \overrightarrow{x} - \overrightarrow{y}$, where $\overrightarrow{x}$ and $\overrightarrow{y}$ have strictly non-negative entries and $|\overrightarrow{x}| + |\overrightarrow{y}|$ is minimized.
Furthermore, this minimum is exactly $|\overrightarrow{v}|$.
## Proof (Lemma 2)
Note that since all the components of these vectors are independent, this is equivalent to minimizing the component-wise sums of $\overrightarrow{x}$ and $\overrightarrow{y}$ $n$ times.
By the results of the previous lemma, each component has a unique solution under these constraints, where the total sum is the sum of the absolute values of the components of $\overrightarrow{v}$, which is exactly it's $L_1$ norm.

Again, we stress that this decomposition is the "obvious one"; for example, the decomposition of $\begin{pmatrix} 1 \\ -2 \\ -3 \\ 4 \end{pmatrix}$ most people would likely pick is $\begin{pmatrix} 1 \\ 0 \\ 0 \\ 4\end{pmatrix} - \begin{pmatrix} 0 \\ 2 \\ 3 \\ 0 \end{pmatrix}$, and actual construction of the decomposition is not particularly difficult. The utility of this result comes from it's guarantee of *uniqueness*, which will come up in our ensuing application of it.

## Proof (main result, continued)

Consider any eigenvector $\overrightarrow{v}$ satisfying $A \overrightarrow{v} = \overrightarrow{v}$. 
By lemma 2, we know that $\overrightarrow{v}$ has a unique composition as $\overrightarrow{v} = \overrightarrow{x} - \overrightarrow{y}$, where $\overrightarrow{x}$ and $\overrightarrow{y}$ are non-negative.
Since $A$ is linear, we know $A \overrightarrow{v} = A\overrightarrow{x} - A \overrightarrow{y} = \overrightarrow{v}$. 
Additionally, we know that $A$ preserves non-negativity of vectors as well as the $L_1$ norm of non-negative vectors (by a natural extension from [stability under vector multiplication](#stability-under-vector-multiplication).
Thus, $A\overrightarrow{x}$ and $A\overrightarrow{y}$ are both non-negative vectors, and have the same $L_1$ norms as $\overrightarrow{x}$ and $\overrightarrow{y}$. 
Therefore, $A\overrightarrow{x}$ and $A \overrightarrow{y}$ decompose $\overrightarrow{v}$ into the sum of two non-negative vectors with a minimal "total" $L_1$ norm. 
By **uniqueness** of our decomposition, $A\overrightarrow{x} = \overrightarrow{x}$ and $A \overrightarrow{y} = \overrightarrow{y}$. 
Since $\overrightarrow{x}$ and $\overrightarrow{y}$ are not both zero, at least one of $\overrightarrow{x}$ or $\overrightarrow{y}$ is an eigenvector of $A$ with strictly non-negative values with eigenvalue 1, and we're done!

# Conclusion
As I've already mentioned, I think this proof is pretty neat, and the decomposition we introduced to justify it turns out to be useful in a couple other results related to stochastic matrices.
Now that we've completed our proof of the desired result, in part II of this post, we'll continue to use these results to prove other tightly coupled results.

With that, I hope you've enjoyed reading this post, and thanks for your time!

{% endkatexmm %}
