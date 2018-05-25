---
layout: post
title:  "Linear regression part four - Finding an unbiased estimate of the variance in the linear model."
date: "2018-05-23"
categories: regression
---

(Here, I borrow heavily from Christensen, _Plane Answers to Complex Questions_.) Consider again the linear model 

$$ E(Y|X) = X\beta. $$

Now, regard each column of $$X$$ as a vector in $$\mathbb{R}^n$$, and consider $$C(X)$$ -- the column space of $$X$$ -- i.e. the subspace of $$\mathbb{R}^n$$ spanned by the columns of $$X$$. 

Define the perpendicular projection operator $$M$$ onto $$C(X)$$ as follows: if $$v$$ is in $$C(X)$$, then $$Mv = v$$. And if $$v$$ is in the space perpendicular to $$C(X)$$ then $$Mv = 0$$. 


We need three lemmas.

**Lemma 1:** $$X(X'X)^{-1}X'$$ is the perpendicular projection operator onto $$C(X)$$. 

**Proof**: Let $$v$$ be in $$C(X)$$. Then $$v = Xb$$ for some vector $$b$$. Hence 

$$

X(X'X)^{-1}X'v = X(X'X)^{-1}X'Xb = Xb = v

$$

Now, let $$v$$ be in the space perpendicular to $$C(X)$$. Then if $$x$$ is any column of $$X, x'v = 0$$. Hence $$X'v = 0$$, and therefore 

$$

X(X'X)^{-1}X'v = X(X'X)^{-1}0 = 0

$$

$$\tag*{$\Box$}$$

**Lemma 2:** If $$M$$ is the p.p.o. onto $$C(X)$$, then $$tr(M) = r(X)$$ -- the trace of $$M$$ is the rank of $$X$$.

**Proof:** Recall two things:

1. If $$Av = \lambda v$$, where $$A$$ is a square matrix, then $$v$$ is an eigenvector of $$A$$, and $$\lambda$$ is the associated eigenvalue. The multiplicity of the eigenvalue $$\lambda$$ is the number of linearly independent vectors $$v$$ such that $$Av = \lambda v,$$ and the total number of eigenvalues of an $$n \times n$$ matrix is $$n$$.

2. Any symmetric matrix $$A$$ admits of a _singular value decomposition_: $$A=P'DP$$, where $$P$$ is an orthogonal matrix ($$P'P = I$$), and $$D$$ is a diagonal matrix, with diagonal entries the eigenvalues of $$A$$.

Now, by definition, if $$v$$ is in $$C(X)$$, then $$Mv = v$$. Hence $$1$$ is an eigenvalue of $$M$$, and because every column of $$X$$ is in $$C(X)$$, the multiplicity of $$1$$ is the number of $$X$$'s columns that are linearly independent. In other words, the multiplicity of $$1$$ is the rank of $$X$$.

Further, if $$v$$ is in the space perpendicular to $$C(X)$$, then $$Mv = 0v.$$ So $$0$$ is another eigenvalue of $$M$$, and its multiplicity is, similarly to the above, the dimension of the space perpendicular to $$C(X)$$. And that characterizes all of the eigenvalues of $$M$$.

Note finally that $$M = X'(X'X)^{-1}X$$ is symmetric, so 

$$tr(M) = tr(P'DP) = tr(DP'P) = \sum_i \lambda_i = \underbrace{1 + 1 + \ldots  + 1}_{r(X) \text{ times}} + 0 + 0 + \ldots + 0 = r(X)$$
$$\tag*{$\Box$}$$


**Lemma 3:** if $$A$$ is a square matrix, and $$Y$$ is a random vector, then 

$$E(Y'AY) = tr(A cov(Y)) + E(Y)'AE(Y)$$


**Proof:**  First, recognize that where $$A$$ is any square matrix, $$[Y - E(Y)]'A[Y - E(Y)]$$ is a scalar. Hence $$tr([Y - E(Y)]'A[Y - E(Y)]) = [Y - E(Y)]'A[Y - E(Y)]$$. Second, note

$$
\begin{align*}
	E[[Y - E(Y)]'A[Y - E(Y)]] &= E[Y'AY - 2E(Y)'AY + E(Y)'AE(Y)] \\
	&= E[Y'AY] - E(Y)'AE(Y)
\end{align*}
$$

And hence 

$$
\begin{align*}
	E[Y'AY] &= E[[Y - E(Y)]'A[Y - E(Y)]] + E(Y)'AE(Y) \\
	&= E[tr([Y - E(Y)]'A[Y - E(Y)])] + E(Y)'AE(Y) \\
	&= E[tr(A[Y - E(Y)]'[Y - E(Y)])] + E(Y)'AE(Y) \\
	&= tr(E[A[Y - E(Y)]'[Y - E(Y)]]) + E(Y)'AE(Y) \\
	&= tr(A E[[Y - E(Y)]'[Y - E(Y)]] ) + E(Y)'AE(Y) \\
	&= tr(A cov(Y)) + E(Y)'AE(Y)
\end{align*}
$$ 

$$\tag*{$\Box$}$$



That, finally, supplies all the resources we need to find an unbiased estimator of $$\sigma^2$$. Recall our quadratic-loss-minimizing line $$\hat{\beta} = (X'X)^{-1}X'Y$$, and suppose that $$E(Y) = X\beta$$, and $$cov(Y) = \sigma^2 I$$. 

Now, consider

$$SSE = [Y - X\hat{\beta}]'[Y  - X \hat{\beta}]$$

and notice

$$
\begin{align*}
	E(SSE) &= E[Y'Y - 2\hat{\beta}'X'Y + \hat{\beta}'X'X\hat{\beta}] \\
	&= E[Y'Y - 2Y'X(X'X)^{-1}X'Y + Y'X(X'X)^{-1}X'X(X'X)^{-1}X'Y] \\
	&= E[Y'Y - Y'X(X'X)^{-1}X'Y] \\
	&= E[Y'Y] - E[Y'X(X'X)^{-1}X'Y]
\end{align*}
$$

So, using **Lemma 3** twice, 

$$
\begin{align*}
	E(SSE) &= E[Y'Y] - E[Y'X(X'X)^{-1}X'Y] \\
	&= [tr(cov(Y)) + \beta'X'X\beta] - [tr(X(X'X)^{-1}X' cov(Y)) + \beta'X'X(X'X)^{-1}X'X\beta] \\
	&= n\sigma^2 + \beta'X'X\beta - [tr(X(X'X)^{-1}X'\sigma^2) + \beta'X'X\beta] \\
	&= n\sigma^2 - tr(X(X'X)^{-1}X')\sigma^2
\end{align*}
$$


But by **Lemma 1**, $$X(X'X)^{-1}X'$$ is the perpendicular projection operator onto $$C(X)$$. Hence, by **Lemma 2**, $$tr(X(X'X)^{-1}X) = r(X)$$. If $$X$$ is non-singular, and of dimension $$n \times p$$, then $$r(X) = p$$.

Hence 

$$ E(SSE) = (n - p)\sigma^2 $$

Or in other words, $$\frac{1}{n -p} SSE$$ is an unbiased estimator of $$\sigma^2$$, where $$p$$ is the rank of $$X$$. 