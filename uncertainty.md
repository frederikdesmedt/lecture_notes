Uncertainty in AI
=====

Expectation Maximisation
----

**E-step**: Complete the data:
Introduce an entry for each possible value assignment for every missing data entry

**M-step**: Maximize likelihood
 
In the M-step example in the slides we use a derivative to find a stationary point. Note how this might not be the global optimum, this is why it's important to choose good initial parameters.

The fucked up `I` is an indicator function.

All derivations that occur in the lecture to create a simplified version of the maximization-step can always be used, i.e., we don't do them every time, no need to reinvent the wheel, just use the results.

Approximate inference
----

Don't do sampling if you can deduce the probabilities exactly, i.e., don't approximate if you can calculate the exact probabilities.

The real art in using sampling as an approximation technique is to find a sampling procedure (we want one that draws samples independently).
This is a hard problem.

Even if we have a good sampling procedure we can still have a lot of difficulties, because we might need lots of samples (if there is high variance).

In the lecture we assume we have a sampling procedure that draws samples independently and for each sample $s$ we have $s \in [0..1]$.
We can use this sampling procedure to retrieve accurate samples from some distribution by defining a cumulant of the distribution.

Finding a formula for the inverse of a cumulant density function can be very difficult, for example: there is no known closed-form formula to represent the inverse of the normal distribution!

Second condition in rejection sampling: $p^*(x)$ and $q(x)$ do not diverge, i.e., the difference between the two is limited by some multiplier.

$\frac{p^*(x^{cand})}{Mq(x^{cand})}$ gives us a value between 0 and 1 (thanks to the assumptions we make). We can then check if some random value between 0 and 1 is below this value, if it is, we accept the candidate, otherwise we deny it. As a result, the probability that we accept some candidate represents the probability we are trying to sample. Of course this only works if we have a correct way of generating random values between 0 and 1.
The key to understanding this is understanding what the fraction represents: if you understand what the fraction represents you understand the procedure.

In **rejection sampling** we would like to minimize $M$ because otherwise we increase the rejection space. The increase of the rejection space increases exponentially in a highly-dimensional sampling space.

How to handle multi-variate sampling? We can assign a single value (e.g. an integer) to each (multi-variate) sample and then sample on this value. This is, however, not very practical since the number of values grows exponentially with the number of variables in your multi-variate sample. Therefore have other known techniques for generating multi-variate samples: **ancestral sampling**, **Gibbs sampling**, ...

> **Ancestral sampling** := Use a belief network to generate samples. We have some roots, these can be sampled as a univariate sample, then based on this sample, try sampling it's children, then their children, ...

If we can correctly sample from the conditional probabilities (and of the univariate samples), we have perfect sampling in the **ancestral sampling** technique.

**Question:** What do we do with ancestral sampling if we do not know the conditional probability? Something like rejection sampling?

In high-dimensional samples, we can possibly use a belief network to discover independencies, this way we can possibly simplify the sampling to several lower-dimensional samples.

Ancestral sampling with evidence: just reject all samples that do not satisfy the given evidence. Won't be efficient if the evidence is exceptional, i.e., the probability of the evidence is low. So it only works if the evidence does not give us a lot of information (because the probability of it occuring is already high)!

> **Gibbs sampling** := Sample the conditional variables based on already known variables. We start with an initial sample, then we iteratively update the components of the sample by considering the conditionals.

**Gibbs sampling** works well if we can easily represent the conditionals, which is not an unreasonable assumption since the conditional is univariate.

Two big problems with **Gibbs sampling**:

- You need a decent initial sample.
- And all samples generated throughout the iteration are dependent.

So we will need to generate a lot of samples in order to lose dependency with the initial guess, which we definitely want! This is called the *burn-in period*.

Waiting for every $k$th example is a pretty good idea, since if we take the 5000th and 5001st they are pretty much the same, but the 5000th and 6000th differ more: more independent. Though this is very expensive.