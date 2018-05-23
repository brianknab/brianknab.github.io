---
layout: post
title:  "MSE is an unbiased estimate of the variance"
categories: regression
---

Finally, consider

$$SSE = [Y - X\hat{\beta}]'[Y  - X \hat{\beta}]$$

and notice

$$
\begin{align*}
	E(SSE) &= E[Y'Y - 2\hat{\beta}'X'Y + \hat{\beta}'X'X\hat{\beta}] \\
	&= E[Y'Y - 2Y'X(X'X)^{-1}X'Y + Y'X(X'X)^{-1}X'X(X'X)^{-1}X'Y] \\
	&= E[Y'Y - Y'X(X'X)^{-1}X'Y]
\end{align*}
$$

Now, let $$A$$ be any square matrix, and notice
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

So 

$$
\begin{align*}
	E(SSE) &= E[Y'Y - 2\hat{\beta}'X'Y + \hat{\beta}'X'X\hat{\beta}] \\
	&= E[Y'Y - 2Y'X(X'X)^{-1}X'Y + Y'X(X'X)^{-1}X'X(X'X)^{-1}X'Y] \\
	&= E[Y'Y - Y'X(X'X)^{-1}X'Y] \\
	&= E[Y'Y] - E[Y'X(X'X)^{-1}X'Y] \\
	&= tr(cov(Y)) + \beta'X'X\beta - [tr(X(X'X)^{-1}X' cov(Y)) + \beta'X'X(X'X)^{-1}X'X\beta] \\
	&= n\sigma^2 + \beta'X'X\beta - [tr(X(X'X)^{-1}X'\sigma^2) + \beta'X'X\beta] \\
	&= n\sigma^2 - tr(X(X'X)^{-1}X')\sigma^2
\end{align*}
$$



where $$r(X)$$ is the rank of $$X$$, an unbiased estimate of $$\sigma^2$$ is given by

$$MSE = \frac{1}{n - r(X)} [Y - X\hat{\beta}]'[Y  - X \hat{\beta}] $$

If we replace $$\sigma^2$$ with that estimate, then $$\hat{\beta}$$ follows a t-distribution with mean $$\beta$$ and degrees of freedom $$n - r(X)$$. And MSE follows a $$\chi^2$$ distribution, also with $$n - r(X)$$ degrees of freedom. 




To wrap up then:
	
1. Ignoring stochasticity altogether, $$(X'X)^{-1}X'Y$$ minimizes quadratic loss over any fixed data set.

2. If we regard $$Y$$ as random and assume conditional independence and homoskedasticity (constant variance) in the $$y_i$$'s' -- i.e., if we assume $$ cov(Y) = \sigma^2 I$$ -- then $$cov((X'X)^{-1}X'Y \vert X) = \sigma^2(X'X)^{-1}$$.

3.  $$(X'X)^{-1}X'Y$$ is an unbiased estimate of the conditional mean of $$Y$$ given $$X$$, if that conditional mean is in fact linear in $$X$$.

4. If $$Y$$ is normally distributed, then $$(X'X)^{-1}X'Y$$ is also. 

And I think that's the standard intro to the simple linear regression model. 