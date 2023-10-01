# Optimal Mean Reversion Trading with Transaction Costs


This Notebook provides code to replicate the optimal timing strategies for trading a mean-reverting price spread as presented in the paper "Optimal Mean Reversion Trading with Transaction Costs and Stop-Loss Exit" by Tim Leung and Xin Li.

(underlying Paper: https://www.worldscientific.com/doi/epdf/10.1142/S021902491550020X)


## Fitting the Ornstein-Uhlenbeck Process

The paper incorporates a standard Orstein-Uhlenbeck (OU) process driven by the following SDE:

$$dX_t=\mu(\theta-X_t)dt+\sigma dB_t$$

where $dW_t$ denotes a Brownian motion with $\mu,\sigma > 0$ and $\theta$ is $\in R$. 

Be aware that $\mu$ and $\theta$ are in this paper used interchanged compared to the standard convention!

The portfolio that will we traded later will be constructed by going long one risky asset A while shorting a different risky asset B. In order to create a Portfolio that exhibits the desired mean reverting properties, the positions sizes $\alpha$ and $\beta$ will be adjusted in order to maximize those. 

The portfolio value X is represented by the following equation:
$$X_t^{\alpha\beta} = \alpha A_t - \beta B_t$$

Parsing through different pairs of $\alpha$ and $\beta$ and using the observed values, we maximize the average log-likelihood defined by the equation:

$$L(\theta,\mu,\sigma|x_{0}^{\alpha\beta},x_{1}^{\alpha\beta},...,x_{n}^{\alpha\beta})=-\frac{1}{2}ln(2\pi)-ln(\tilde\sigma)-\frac{1}{2n\tilde\sigma^2}\sum\limits_{i=1}^{n}[x_{i}^{\alpha\beta}-x_{i-1}^{\alpha\beta}e^{-\mu\Delta t}-\theta(1-e^{-\mu\Delta t})]^2$$

with

$$\tilde\sigma^2=\sigma^2\frac{1-e^{2\mu\Delta t}}{2\mu}$$


## Solving Optimal Stopping Problem

Using the process parameters $\mu,\sigma$ and $\theta$ of the defined OU Process we further add r and c which represent the set of discount rates and transaction costs.

The Analytical results of optimal sell(b) and buy point(d) can be obtained by solving:

$$F(b) = (b-c)F(b)$$

$$\hat G(d)(V´(d)-1)=\hat G´(d) (V(d) - d - c)$$


To do so we need the following functions outlined in the paper as well as its derivatives:

$$F(x,r) = \int\limits_{0}^{oo}u^{\frac{r}{\mu}-1}e^{\sqrt{\frac{2\mu}{\sigma^2}}(x-\theta)u-\frac{u^2}{2}}du$$
$$F(x,r) = \int\limits_{0}^{oo}u^{\frac{r}{\mu}-1}e^{\sqrt{\frac{2\mu}{\sigma^2}}(\theta-x)u-\frac{u^2}{2}}du$$
$$V(x) = \left\{ \begin{array}{ll}
     (b-c)\frac{F(x)}{F(b)} & \mbox{if $x \in (-\inf,0)$};\\
         x-c & \mbox{otherwise}.\end{array} \right.$$

