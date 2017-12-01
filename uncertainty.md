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

