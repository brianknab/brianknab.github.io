---
layout: post
title:  "Simple linear regression part three - wrapping up"
date:   2018-05-11
categories: regression
---

In the last two posts, we first found the line that minimizes quadratic loss over a fixed data set. That line is given, again, by the vector:

$$
	(X'X)^{-1}X'Y.
$$


We then treated $$Y$$ as a random object -- drawn from some distribution $$P(Y \vert X)$$, and showed that, if $$cov(Y) = \sigma^2 I$$, then $$cov((X'X)^{-1}X'Y) = \sigma^2 (X'X)^{-1}$$.

Now, let us further assume that the conditional mean $$E(Y \vert X)$$ is linear in $$X$$, i.e. that $$E(Y \vert X) = X\beta$$, where again: 

$$
	\begin{align*}
		Y = \begin{bmatrix}
			y_1 \\
			y_2 \\
			\vdots \\
			y_n
		\end{bmatrix}, \quad
		X = \begin{bmatrix}
			1 & x_1 \\
			1 & x_2 \\
			\vdots & \vdots \\
			1 & x_n
		\end{bmatrix}, \quad \text{and }
		\beta = \begin{bmatrix}
			\beta_0 \\
			\beta_1
		\end{bmatrix}
	\end{align*}
$$

Then notice that 

$$
\begin{align*}
	E[(X'X)^{-1}X'Y] &= (X'X)^{-1}X'E(Y) \\
	&= (X'X)^{-1}X'X\beta \\
	&= \beta
\end{align*}
$$

What have we shown? We've shown that if, in fact, the conditional mean of $$Y$$ given $$X$$ is linear in $$X$$, then $$(X'X)^{-1}X'Y$$ is an unbiased estimate of that conditional mean. 

To wrap up then:
	
1. Ignoring stochasticity altogether, $$(X'X)^{-1}X'Y$$ minimizes quadratic loss over any fixed data set.

2. If we assume conditional independence and homoskedasticity (constant variance) in the $$y_i$$'s' -- i.e., if we assume $$ cov(Y) = \sigma^2 I$$ -- then $$cov((X'X)^{-1}X'Y \vert X) = \sigma^2(X'X)^{-1}$$.

3.  $$(X'X)^{-1}X'Y$$ is an unbiased estimate of the conditional mean of $$Y$$ given $$X$$, if that conditional mean is in fact linear in $$X$$.

And I think that's the standard intro to the simple linear regression model. 

