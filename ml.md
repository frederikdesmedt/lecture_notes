Machine Learning and Inductive Inference: Notes
================

10 Bayesian Learning
---------------------

Q: Concerning MAP and MDL solution: Why is there no link between optimal and practical encodings?

A: Because the observations of $h_{map} = h_{mdl}$ only say something about the *optimal* encoding, they say nothing about non-optimal encodings that might be better than some others.

If the assumptions imposed by Naive Bayes (involving conditional independence) are violated, the error of $P(y \mid a_1, a_2, ...)$ can indeed be high. However, for $argmax_y P(y \mid ...)$ the error seems to be fine, and it's only the last formula we use after all.

Bayesian belief networks = belief network of Uncerainty In AI, as we would suspect.

> Belief network where edges follow causal influences := the parent is the event causing the child event to occur (causal).

Creating a belief network that does not follow the causal influences is usually more complex! Yet it does work.

Mapping Naive Bayes in a belief network: have a root node representing the heuristic that occurs, and create an edge from this node to every attribute node.

Learning probability tables in belief network: just count the number of occurences and divide total number.
Learning structure of belief network (topology of the graph):

- Start from a fully connected graph and remove edges as you discover independencies. Might not work very well because arrows can only go in one direction (DAG).
- Another way is to start with no edges and add them as you discover dependencies.
- This kind of learning is still under active research!

Probability tables are larger the more parents it has. In general, if a node has $n$ parents, it will have a probability table of $2^{n+1}$ entries. This is why Naive Bayes is so efficient, since every attribute node only has one parent.

10.1 Statistical learning
-----

**Slide 40:** in the linear regression example, $\theta = {a;b}$ and $X = aX + b$.

> Question: What is "Gaussian noise"?

In Bayesian learning in the statistical learning setting we just use an approximation or something similar to estimate the prior $p(\theta)$.

Generative models contain more information than discriminative models, yet discriminative models are better for predicting the task for which they are meant (predicting $Y$ based on $X$).
An intuitive idea of this: we specialize in this kind of prediction in the discriminative model, so it makes sense that we get better results if we spend an equal amount of work in both kind of models.

Naive Bayes is an example of a *generative* model! Which is surprising since the formulas involving Naive Bayes are based on conditionals.

**Exam:** With statistical learning we only really need to understand the difference between discriminative and generative models, we don't need to understand it that in depth.

11 Combining classifiers
--------

> Ensemble := hypothesis that is the result of combining two separate hypotheses (and works better than two of them separately).

Voting system works if every model is as accurate as every other model.
But if one is always correct and the others are always wrong, suddenly the voting system no longer works.

With bagging, the size of the generated training sets is the same as the size of the original one, yet they are not always the same since an element can be chosen multiple times.
The result: $m$ different training sets of size $n$.

Bagging works when the model is unstable: then the $m$ different generated training sets differ enough such that the resulting models are heterogonous enough to be useful in an ensemble.

Boosting analogy: raise the bounty of some examples in de training set. Future models will work harder to try and predict these correctly. The result are usually pretty different models, since future models will focus on examples previous models did not work very well on.

Stacking: stack one learner upon another. We use ML to find an "efficient" ensemble, an "efficient" combination of the list of learners we received after the first step (where we might use bagging and/or boosting). The top learner can use the different models + the original data for this.

12 Reinforcement Learning
--------

A kind of unsupervised learning.

Problem statement: Learning "what to do"
Solutions:

- **Planning:** we know the world we're in, the actions we can do, and the consequences of those actions. Based on that plan something that will work. Modelling the world is the difficult part here.
- **Behavioral cloning:** Imitate some teacher. The teacher will perform actions based on some situations. Problem here: the model/student will never really become better than the teacher. We also need a way of cloning behavior (a policy that explains how to handle and generate policies).
- **Reinforcement learning:** The learner is not guided and has to experiment. Based on experimentations it will be rewarded or punished, this guides it to become better. *This is the most difficult learning setting of the three listed here!*

Reinforcement learning is a lot harder in computer science than in real life (parent - kid example) because the reward or punishement might be delayed.

The maximization of $V^\pi(s_t)$ makes sure that we consider all the rewards in the future as well as the rewards in the near future (say in the next state). The discount factor $\gamma^i$ will define some weight to make sure that future states are less important than rewards in the near future (so usually $0 < \gamma < 1$?)

The Bellman equation shows the relation between the state-value of the current state and the state-value of the next state.

In the policy evaluation $V_{i+1}$ represents the $(i + 1)$st approximation for $V^\pi$. If we do this infinitely often, we get a closed form of the Bellman equation, which can be calculated.

The Bellman error provides us with a stop criteria: stop the moment the Belmann error falls below some threshold $\epsilon$.

**Greedy action selection** := we use a different policy in the first step (in the $argmax$). Yet we still use $V^\pi$, so all the other actions are found by using the older policy, but if we do this iteratively, we already know that all the previous steps are optimal (because we have maximimized them in a previous iteration).

*Note: To get an idea of $V^\pi$ "the robot" needs to run experiments.*

**Question:** Partial evaluation and value iteration is a cheaper variant of the previously seen greedy action selection?