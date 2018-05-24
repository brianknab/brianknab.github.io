---
layout: post
title:  "Linear regression part three - if the mean is linear, the quadratic loss minimizing line is an unbiased estimate of that mean. A normal response yields normality in the quadratic loss minimizer."
date:   2018-05-11
categories: regression
---

In the last two posts, I first found the line that minimizes quadratic loss over a fixed data set. That line, again, is given by the vector:

$$
	(X'X)^{-1}X'Y.
$$


We then regarded $$Y$$ as a random object -- drawn from some distribution $$P(Y \vert X)$$, and showed that, if $$cov(Y) = \sigma^2 I$$, then $$cov[(X'X)^{-1}X'Y] = \sigma^2 (X'X)^{-1}$$.

Now, let us make additional assumptions.  Let us assume that the conditional mean $$E(Y \vert X)$$ is linear in $$X$$, i.e. that $$E(Y \vert X) = X\beta$$, where again: 

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
	E[(X'X)^{-1}X'Y \vert X] &= (X'X)^{-1}X'E(Y) \\
	&= (X'X)^{-1}X'X\beta \\
	&= \beta
\end{align*}
$$

So if, in fact, the conditional mean of $$Y$$ given $$X$$ is linear in $$X$$, then $$X(X'X)^{-1}X'Y$$ is an unbiased estimate of that conditional mean. 

Further, if we assume that each of the $$y_i$$'s is normally distributed around its respective mean. Then because any linear combination of independent normally distributed random variables is itself normally distributed, $$(X'X)^{-1}X'Y$$ is also normally distributed; and hence by our earlier results 

$$\hat{\beta} = (X'X)^{-1}X'Y \sim N(\beta, \sigma^2(X'X)^{-1})$$



