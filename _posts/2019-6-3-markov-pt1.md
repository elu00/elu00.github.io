---
title: "Markov Chains and the Perron-Frobenius theorem"
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
The content of this post is based off material from 21-242 at CMU as taught by [Clinton Conley](https://www.math.cmu.edu/~clintonc/).
Consequently, I claim no credit for any of the ideas presented below.
# About
A well-known result in linear algebra is the [Perron-Frobenius theorem](https://en.wikipedia.org/wiki/Perron%E2%80%93Frobenius_theorem), which implies that (among other results) every **stochastic matrix** admits a **stochastic eigenvector** with eigenvalue one (definitions of those terms to follow later). 
The latter result finds a variety of uses in the study of probability, but most proofs (at least, those easily Googleable) appeal to results outside the realm of linear algebra (using e.g. an appeal to compactness or fixed point theorems).
Since we're ultimately dealing with *linear* operators, however, such analysis seems unnecessary. 
In this post, we'll be going over the background needed for this result, ultimately proving it using nothing more advanced than the triangle inequality and basic linear algebra. 

# Motivation
Suppose we're modeling some system whose state can be fully described by some finite set of states $S = \{S_1, S_2 \cdots, S_n,\}$.
For the purposes of imagination, we can think of such states as a collection of lily pads that a frog is jumping inbetween, a set of stations that a train goes between, or, in the case of Google's PageRank, a set of websites that someone is browsing through.
We'll view all possible evolutions of our system as changes between these states, and we'll also make the assumption that these evolutions occur in **discrete time**; that is, that the evolution of our system is described by the timesteps $t = 0, 1, 2, \cdots$ and an assignment of a state $s \in \{S_1, S_2 \cdots, S_n,\}$ to each timestep.
We also make the assumption that these transitions happen randomly at every timestep, with transition probabilities depending only on the current state (in particular, independently of the current time), meaning that at any state $S_i$ and time $t_1$, one will be at state $S_j$ at time $t_1 +1$ with a probability $p \in [0,1]$ only depending on $S_i$ and $S_j$.
This assumption means that, for example, if you're on the front page of reddit, you're just as likely to visit a particular post whether or not you've seen it 1 times or 100 times before.
With these assumptions in mind, then, let's move onto the problem of how to mathematically model such a system.

We note that to fully encapsulate the **properties** of such a system, it suffices to capture the transition probabilities between any pair of states. 
To do this, we'll use a $n \times n$ matrix $A$ where $A_{i,j}$ represents the probability of being in state $i$ one time step after being in state $j$. Since probabilities are always positive, and we insist that our system is always in some state, we naturally impose the constraint that $A_{i,j} \ge 0$ and that for all $j \in [n],  \sum_{i \in [n]} A_{i,j} = 1$.
We call such a matrix satisfying these properties **stochastic**.

Given our definition of stochastic matrices, the idea of a **stochastic vector** naturally follows. We'll define such vectors to be any valid column vectors of a stochastic matrix. More formally, this means any vector $\overrightarrow{v} \in \mathbb{R}^n = \{v_1, v_2, \cdots, v_n \}$ with $v_i \ge 0, \sum_{i \in [n]} v_i = 1$. 
Whereas stochastic matrices serve to model the transition behaviors of our system, stochastic vectors can be thought of as encoding probability distributions for a single point in time, with $\overrightarrow{v}_i$ corresponding to the probability that one is in state $i$.
Supposing that $\overrightarrow{v}$ specifies such probabilities at $t = i$, it follows fairly quickly from the definition of matrix multiplication that $A\overrightarrow{v}$ specifies the same probabilities at $t = i+1$.
Given these definitions, we're now ready to begin looking at various properties of these objects.

# Properties of Transition Matrices
Firstly, we'll check some basic intuitions about how these two objects interact with each other.
## Stability under vector multiplication
As noted above, multiplication by $A$ effectively corresponds to taking a single timestep forward.
Given that an original vector $\overrightarrow{v}$ encodes a probability distribution, then, $A\overrightarrow{v}$ should as well, which we now check explicitly.
Suppose that $A$ is a stochastic matrix and that $\overrightarrow{v}$ is stochastic. We'll now show that $A\overrightarrow{v}$ is stochastic.
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
Suppose that $A$ and $B$ are two stochastic $(n \times n)$-matrices.  Then $AB$ is also stochastic.
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
With these properties out of the way, we move onto our main result, which we state as follows:

## Theorem
For any stochastic matrix $A$, there exists a stochastic eigenvector $\overrightarrow{v}$ of $A$ with eigenvalue $1$; that is $A\overrightarrow{v} = \overrightarrow{v}$.

A more intuitive way of stating this result is saying that any such system has a "steady state" or fixed point, with the probability distribution represented by this eigenvector remaining fixed under the evolution of the system.
Using results that we'll be proving in the second part of this post, it can actually be shown that **any** initial probability distribution will tend to such a steady state as $t \to \infty$, meaning that for sufficiently large $t$, the probability distribution will be effectively independent of $t$.

At this point, a relatively overkill but functional proof of this theorem would be as follows.
Note that the set of stochastic vectors in $\mathbb{R}^n$ is simply the convex hull of the $n$ standard basis vectors and is hence compact. 
Furthermore, since this set is [stable under multiplication by $A$](#stability-under-vector-multiplication), $A$ (viewed as a continuous linear operator) sends this set to itself.
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
by the result above. Since $1 \neq 0$, $A-I$ is non-invertible, and we know that any stochastic matrix $A$ has a eigenvalue of 1. 

Now all we need to show is that there is some stochastic eigenvector associated to this eigenvalue.

As it turns out, doing this is a bit more complicated than it may first appear, as the eigenvector produced above may have both positive and negative entries.
Nevertheless, we'll be able to prove that some eigenvector exists with only non-negative entries, which finishes as we can then normalize the entries so that they sum to 1.

To do so, it suffices to be able to prove that, given **any** eigenvector with entries of potentially mixed signs, we can construct an eigenvector with all positive or negative entries (for convenience, we'll call such vectors single-signed).

The basic approach will be to define a decomposition of any vector into two single-signed vectors, then to show that the vectors in this decomposition remain eigenvectors (provided that the original vector is also an eigenvector). In some sense, the decomposition we use is the "obvious one" (taking one vector to be all the positive entries, and the other to be all the negative entries), but **uniqueness** will play a crucial role in our argument.

We'll begin by demonstrating existence and uniqueness in one dimension, which turns out to be pretty trivial.

## Lemma 1
Fix some real $c$, and consider $(a,b)$ such that $a - b = c$ and $a, b \ge 0$. 
Then there is a unique solution $(a,b)$ that minimizes $a+b$.
Furthermore, this minimum is exactly $|c|$, and is achieved when one of $a$ or $b$ is equal to $|c|$, and the other is zero.
## Proof (Lemma 1)
From the triangle inequality, we know that 
$|c| = |a - b| \le |a| + |b| = a+b$, so $a + b$ is bounded below by $|c|$.
Furthermore, we know that equality holds if and only if $a$ and $-b$ have the same sign.
However, since $a$ is non-negative and $-b$ is non-positive, this can only happen when one of them is zero. 
Based on the sign of $c$, this uniquely determines the solution $(a,b)$ mentioned above.

Given this result involving non-negative reals, we can naturally extend it to non-negative vectors.
## Lemma 2
Let $|\overrightarrow{v}|$ be the $L_1$ norm of $\overrightarrow{v}$ (that is, $|\begin{pmatrix} v_1 \\ v_2 \\ \cdots \\ v_n \end{pmatrix}| = |v_1| + |v_2| + \cdots |v_n|$).
    Any vector $\overrightarrow{v}$ has a unique decomposition of the form $\overrightarrow{v} = \overrightarrow{x} - \overrightarrow{y}$ that minimizes $|\overrightarrow{x}| + |\overrightarrow{y}|$ subject to the constraint that $\overrightarrow{x}$ and $\overrightarrow{y}$ have strictly non-negative entries.
Furthermore, this minimum is exactly $|\overrightarrow{v}|$.
## Proof (Lemma 2)
Note that since all the components of these vectors are independent, this is equivalent to minimizing the component-wise sums of $\overrightarrow{x}$ and $\overrightarrow{y}$ $n$ times.
By the results of the previous lemma, each component has a unique solution under these constraints, where the total sum is the sum of the absolute values of the components of $\overrightarrow{v}$, which is exactly it's $L_1$ norm.

Again, we stress that this decomposition is the "obvious one"; for example, the decomposition of $\begin{pmatrix} 1 \\ -2 \\ -3 \\ 4 \end{pmatrix}$ most people would likely pick is $\begin{pmatrix} 1 \\ 0 \\ 0 \\ 4\end{pmatrix} - \begin{pmatrix} 0 \\ 2 \\ 3 \\ 0 \end{pmatrix}$, and formalizing this construction is not particularly difficult. The utility of this result comes from it's guarantee of *uniqueness*.

## Proof (main result, continued)

Consider any eigenvector $\overrightarrow{v}$ satisfying $A \overrightarrow{v} = \overrightarrow{v}$. 
By lemma 2, we know that $\overrightarrow{v}$ has a unique composition as $\overrightarrow{v} = \overrightarrow{x} - \overrightarrow{y}$, where $\overrightarrow{x}$ and $\overrightarrow{y}$ are non-negative.
Since $A$ is linear, we know $A \overrightarrow{v} = A\overrightarrow{x} - A \overrightarrow{y} = \overrightarrow{v}$. 
Additionally, we know that $A$ preserves non-negativity of vectors as well as the $L_1$ norm of non-negative vectors (by a natural extension of [stability under vector multiplication](#stability-under-vector-multiplication)).
Thus, $A\overrightarrow{x}$ and $A\overrightarrow{y}$ are both non-negative vectors, and have the same $L_1$ norms as $\overrightarrow{x}$ and $\overrightarrow{y}$. 
Therefore, $A\overrightarrow{x}$ and $A \overrightarrow{y}$ decompose $\overrightarrow{v}$ into the sum of two non-negative vectors with a minimal "total" $L_1$ norm. 
By **uniqueness** of our decomposition, $A\overrightarrow{x} = \overrightarrow{x}$ and $A \overrightarrow{y} = \overrightarrow{y}$. 
Since $\overrightarrow{x}$ and $\overrightarrow{y}$ are not both zero, at least one of $\overrightarrow{x}$ or $\overrightarrow{y}$ is an eigenvector of $A$ with strictly non-negative values with eigenvalue 1, and we're done!

# Conclusion
As I've already mentioned, I think this proof is pretty neat, and the decomposition we introduced to justify it turns out to be useful in a couple other results related to stochastic matrices.
Now that we've completed our proof of the desired result, in part II of this post, we'll continue to use these results to prove other tightly coupled results.

With that, I hope you've enjoyed reading this post, and thanks for your time!

{% endkatexmm %}
