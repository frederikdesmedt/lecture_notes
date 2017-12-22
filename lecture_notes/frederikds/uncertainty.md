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

In Gibbs sampling we can use sampling to approximate $p(a^b|a^1, a^2, ..., a^{b-1}, a^{b+1}, ..., a^n)$.

With Gibbs sampling with belief nets evidence is handled by fixing values to variables, and we don't generate samples for these variables.

Importance sampling: we get rid of the difference between $p(x)$ and $q(x)$ by *weighting*. You can get an intuitive notion of the weight by looking at $p^*(x^l)/q(x^l)$: if it is high, it means that it is much more probable that $p^*$ will find that sample than that $q$ will find it: so increase the weight so that the sample becomes more important (or occurs more often, if you look at the summation). The opposite happens when the fractional is lower than 1.

Note how importance sampling requires less beforehand than rejection sampling does, which makes sense, since importance sampling reduces to rejection sampling, but not the opposite.

**Sample degeneracy** := Because we're creating samples from a possibly big interval, we will slowly attract all weights to a single sample (the one closest to the actual average), yet this might not be the best sample. Solution: Create new samples with the new distribution that occur in the high probability regions, this way we can get more accurate results.

Conclusion slide tells you what the exam will be about.

Exam: something similar to the robot video in the lecture.

Dynamical models
---

Number of variables increase over time, making the problem more intractable, *but*, the variables that have occurred long ago are not as important, so we slowly forget about them.

State in dynamical models are not simple samples, they are all samples that have occurred until the current timeframe.

If all previous states (in time) are not important, but only the current state matters to determine the next state, then we comply with the **Markov property**. This is an easy way to make the problem way more tractable.

Model that complies to the **Markov property** is a first order Markov chain. A second order Markov chain depends on the last 2 states, third order on the last 3 states, ...

**Stationary Markov chain** := state transition probability (i.e. system model?) is constant.

A discrete first order stationary Markov chain has: $p(x^t = s^' | x^{t-1} = s) = f(s, s')$, i.e., the probability of the current state depends on the last state, and is independent of the current time!

We can represent discrete first order stationary Markov chains using a **state transition diagram** (look at the sunny-rainy example in the slides).

In the transition matrix, the sum of each component in a single row is equal to 1, i.e., the row defines the condition, the column defines the variable we want to know the probability of, given the row.

**Hidden Markov model** := Model where we have some hidden variables that influence the observed variables.

A *stationary hidden Markov model* has a similar factoring as the *stationary Markov chain*, except we also require the emission distribution to be constant (the arrows between the the hidden variable and the observed variable).

With *hidden Markov models* we are typically interested in the probability of the hidden variables, which makes sense, since the observed variables are already observed, so they are not really that big a deal (you could, say, perform sampling if you would like to know something more about them).

Forward algorithm is intuitively obvious if you were to imagine a closed-form by getting rid of the recursion, i.e., expanding $\alpha$.

Forward algorithm allows real-time estimation, considering we have already calculated estimates for the previous timeframes.

The forward algorithm is a special case of the sum-product algorithm, i.e., sum-product with $h_t$ as the root.

> *Forward algorithm == $\alpha$-recursion.*

In the forward-backward algorithm, we have conditional independence of the past and future given the evidence thanks to the Bayesian network of the hidden Markov model.

The **Viterbi** makes sense if you expand $\mu(h_t)$ in backtracing. In backtracing we see that it is indeed correct, the Viterbi-algorithm itself provides an efficient way for solving it.

Last 5 minutes of lecture: explanation as to why Markov property does not always hold. It doesn't hold, because how the robot reacts to an action depends on the previous actions, i.e., the robot might already be moving and would be accelerating when you again move it forwards.
We can however apply a trick to *do* get the Markov property: we "inflate" our state by introducing the velocity in the state.

In the running example of continuous dynamical models, $\omega_t$ is the rotation/angular step that has occured due to input.
The system model includes some noise to account for poorly-calibrated hardware.

Using a Gaussian as summarizing statistic is pretty nice because we only need a mean and a covariance for it, i.e., we don't need a complex data structure to represent it.

In online estimation we use summarizing statistics because of the limited resources and restrictions imposed on it, because of this all results are approximations.

Kalman filter can only handle unimodel data, e.g., a robot cannot be either completely to the left, or completely to the right, since we use a normal distribution to represent the probability of the state (and only a single normal distribution!).

A dynamical system satisfying all Kalman assumptions will be represented perfectly by the Kalman filter, i.e., it will not be an estimation!

The running example in Kalman filter does not have a linear system model, since it uses $cos$ and $sin$, so we need to approximate it with a linear model.

Sample-based filters have bad scaling to higher dimensions because of the exponential increase in samples required.

In particle filters we expect the samples to be drawn according to the posterior distribution, so we can try to estimate this posterior by analysing the distribution of the samples: importance sampling.

> Prediction-step := apply system model to previous samples.
> Correction-step := apply probability of new sample (after applying system model to the previous sample), based on the measurement model.

Particle filtering is a pretty good name, considering we have a lot of samples (particles), and we filter those using the correction-step.

Additional questions last lecture
---------------------------------

Bring a calculator to the exam!

Look at the example exam on Toledo.

Take into account causality when coming up with a belief network for a problem.

Major difficulty on exam: time!

Exam: about doing and understanding the practical stuff.

Exam lasts for 4 hours.