---
layout: post
title:  "Simple linear regression part two - with stochasticity this time"
date:   2018-04-26
categories: regression
---

Last time, I began with a graph.

Namely, this one:
![Scatter]({{"/images/no_line.png"}})

And, we regarded the points on that graph as fixed, and found the linear function of $$x$$ which minimized quadratic loss for those points. And recall there was nothing stochastic about anything; we were just minimizing a loss function. And recall also that if we let

$$
	\begin{align*}
		Y = \begin{bmatrix}
			y_1 \\
			y_2 \\
			\vdots \\
			y_n
		\end{bmatrix}, \quad \text{and} \quad
		X = \begin{bmatrix}
			1 & x_1 \\
			1 & x_2 \\
			\vdots & \vdots \\
			1 & x_n
		\end{bmatrix}
	\end{align*}
$$

then we found that the vector 

$$ \hat{\beta} = (X'X)^{-1}X'Y $$

supplies the line which minimizes quadratic loss for our data. Its first element is the intercept, and its second is the slope.

But typically, we do not think of our data as fixed. Instead, we see it as the realization of a chance process --  a realization from a joint probability distribution, $$P(x, y)$$, governing $$x$$ and $$y$$. (We might, for example, think of $$x$$ as weight, and $$y$$ as height, and then imagine selecting 100 people at random. $$P(x,y)$$ would then describe the probability of selecting a person of weight $$x$$ and height $$y$$.)

And if we do see our data as the realization of a chance process, then we might wonder about things besides which line minimizes quadratic loss for our observed data. We might also wonder, for example, if we had seen an alternative realization from $$P(x,y)$$, how would we expect our quadratic-loss-minimizing line to have differed? Below, for example, we have two 100 data point samples from the same underlying distribution on $$x$$ and $$y$$, along with the quadratic-loss-minimizing lines for each:

![TwoSamplesWithLine]({{"/images/two_samp_lines.png"}})

Appreciate how they vary and waver.

Now, an alternative thing we might wonder about is this: if we regard our $$x$$-values as _fixed_, but imagine we had seen an alternative realization from $$P(y \vert x)$$, how would we expect our quadratic-loss-minimizing line to have varied?

The below picture shows 20 100-data point samples from the same distribution $$P(y \vert x)$$, along with quadratic-loss-minimizing lines for each:
![Twenty Lines]({{"/images/lin_fit_lines.png"}})

The vertical striations in the data are a result of the fact that the $$x$$ values are fixed; we are simply sampling $$y$$ from around those fixed $$x$$ values.

Now, the difference between that first question, 'how would I expect the lines to have varied if I had seen an alternative realization from $$P(x,y)$$?' and the second question 'how would I expect the lines to have differed if I had seen an alternative realization from $$P(y \vert x)$$?' is subtle. The first asks -- across samples from $$P(x,y)$$, how do quadratic-loss-minimizers tend to vary. The second asks -- across all samples from $$P(x,y)$$ _where $$x$$ takes the values that $$x$$ happened to take in my data_ -- how do quadratic-loss-minimizers tend to vary? Here, we'll focus on this latter question, partly because it's tradition, but mostly because it's easier to answer.


Now, without knowing the distribution $$P(y \vert x)$$ it's hard to say much about how much variation we should expect in our quadratic-loss-minimizing lines. But we can make the problem tractable by imposing some additional constraints.

For example, assume that, conditional on $$x$$, each response $$y_i$$ is independent of every other response $$y_j$$. Imagine, continuing with our earlier example, we will randomly select 10 people, but we stipulate that the first person will weigh 100 lbs, the second 110, the third 130, etc. What the conditional independence assumption requires is that the probability that the second person selected is five feet tall is independent of whether or not the first person was five feet tall. 

Imagine also that -- again conditional on $$x$$ -- the variance of the responses $$y$$ is constant, where we denote that constant variance by '$$\sigma^2$$'. In our example, this would be to suppose that the average departure -- of the height of people who are 100 lbs from the mean height of 100 pounders -- is equal to the average departure of people who are 110 lbs from the mean height of 110 pounders, and similarly for all other weights. (Strictly speaking, it's to suppose something stronger than that, but that would follow.)

Given those two assumptions, we can compute the covariance matrix of $$\hat{\beta}$$ as follows:

$$
\begin{align*}
	cov (\hat{\beta} | X) &= E[(\hat{\beta} - E(\hat{\beta}))(\hat{\beta} - E(\hat{\beta}))' | X] \\
	&= E\big[\big(X'X)^{-1}X'Y - E((X'X)^{-1}X'Y)\big) \big((X'X)^{-1}X'Y - E((X'X)^{-1}X'Y)\big)' | X] \\
	&= E[(X'X)^{-1}X'YY'X(X'X)^{-1} - \\
	& \quad \qquad (X'X)^{-1}X'E(Y)Y'X(X'X)^{-1} - \\
	& \quad \qquad (X'X)^{-1}X'YE(Y)'X(X'X)^{-1} + \\ 
	& \quad \qquad (X'X)^{-1}X'E(Y)E(Y)'X(X'X)^{-1} | X] \\
	&= (X'X)^{-1}X'E\big[YY' - E(Y)Y' - Y'E(Y)' + E(Y)E(Y)'\big]X(X'X)^{-1} \\
	&= (X'X)^{-1}X'\underbrace{E\big[(Y - E(Y))(Y - E(Y))']}_{cov (Y)}X(X'X)^{-1} \\
	&= (X'X)^{-1}X'\sigma^2IX(X'X)^{-1} \\
	&= \sigma^2 (X'X)^{-1}X'X(X'X)^{-1} \\
	&= \sigma^2 (X'X)^{-1}
\end{align*}
$$

I think the generality of this result is somewhat surprising, in that it only relies on conditional independence and homoskedasticity (constant variance). Consider, to see why, the below picture, which shows 20 100-data-point samples from a fixed-conditional-variance distribution, and where $$E(y \vert x) = x^3$$, along with 20 corresponding quadratic-loss-minimizing lines: 

![Cube mean]({{"/images/cube_fit_lines.png"}})


Now, compare the above picture to a similar picture -- with the same $$x$$-values, but where the $$y$$-values are generated from a distribution with $$E(y \vert x) = x$$ -- i.e., where the conditional mean of $$y$$ is a linear, instead of a cubic, function of $$x$$, and with the same fixed conditional variance as above:

![linear mean]({{ "/images/lin_fit_lines_cube_scale.png" }})

We see generally different slopes across the two plots. That's not surprising. But consider: I've plotted both on the same scale, and the variation in the quadratic-loss-minimizing lines is (approximately) the same in both cases. This, despite the fact that the conditional mean of $$y$$ is a much more complicated function of $$x$$ in the former case versus the latter. 

I think that last _is_ surprising. More precisely, what I think is surprising about the above result is that the covariance matrix of $$\hat{\beta}$$ -- which expresses the amount of variation in our quadratic-loss-minimizing lines -- is independent of $$E(y \vert x)$$ considered as a function of $$x$$. Under every such function you should expect the same amount of variation in your quadratic-loss-minimizing lines.

Maybe that's only surprising to me. 

That's enough for now; something that is also surprising is that there is still (a lot) more to say about drawing straight lines through a crop of data points. So, still more to come on this.



---

Code below; generates 20 100-data-point samples from the same conditional distribution, and plot quadratic-loss-minimizers for each.

{% highlight r %}

require(ggplot2)

## Init structures to store samples
df <- NULL
beta_hat <- NULL

## Generate some x-values. X[,2] ~ N(0, 1)
X <- matrix(c(rep(1, 100), rnorm(100)), ncol = 2)

## Over 20 iterations, generate 100 y-values, Y ~ N(X[,2], 1), 
## and compute quadratic-loss-minimizers
for (i in seq(1:20)){
  Y <- rnorm(100, mean = X[,2])
  df <- rbind.data.frame(df, cbind.data.frame(x = X[,2], y = Y, iter = i))
  
## Find (X'X)^(-1)X'Y.
  bh <- solve(t(X) %*% X) %*% t(X) %*% Y
  beta_hat <- rbind.data.frame(beta_hat, 
                               cbind.data.frame(intercept = bh[1], slope = bh[2], iter = i))
}

## Plot the samples along with their quadratic-loss-minimizers.
ggplot(df, aes(x = x, y = y, color = as.factor(iter))) + 
  geom_point() + 
  theme_minimal() +
  scale_color_grey(start = 0.1, name = 'Sample') + 
  theme(text = element_text(size = 20)) +
  geom_abline(data = beta_hat,
              aes(intercept = intercept, slope = slope, color = as.factor(iter))) 


{% endhighlight %}



