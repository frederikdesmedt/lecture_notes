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

Reinforcement learning is a lot harder in computer science than in real life (parent - kid example) because the reward or punishement might be delayed, also, there are no learning examples!

The maximization of $V^\pi(s_t)$ (the value function) makes sure that we consider all the rewards in the future as well as the rewards in the near future (say in the next state). The discount factor $\gamma^i$ will define some weight to make sure that future states are less important than rewards in the near future (so usually $0 < \gamma < 1$?)

The Bellman equation shows the relation between the state-value of the current state and the state-value of the next state.

In the policy evaluation $V_{i+1}$ represents the $(i + 1)$st approximation for $V^\pi$. If we do this infinitely often, we get a closed form of the Bellman equation, which can be calculated. *Note: we do not need to do this infinitely often, because the algorithm converges to $V^{\pi}$.*

The Bellman error provides us with a stop criteria: stop the moment the Belmann error falls below some threshold $\epsilon$.

In the *policy iteration* algorithm example, the shortest path is taken because of the discount factor.

**Greedy action selection** := we use a different policy in the first step (in the $argmax$). Yet we still use $V^\pi$, so all the other actions are found by using the older policy, but if we do this iteratively, we already know that all the previous steps are optimal (because we have maximimized them in a previous iteration).

*Note: To get an idea of $V^\pi$ "the robot" needs to run experiments.*

**Question:** Partial evaluation and value iteration is a cheaper variant of the previously seen greedy action selection?

With **Q-learning** we don't know what's happening in advance, instead we update the Q-value as we go along (while the robot is performing actions).

Note how in **Q-learning** we are also interested in the state we are getting at and the actions we can perform there (and the states we might be able to reach after that as well!) and not only in the (immediate) reward $r$ we are getting. Yet distant futures are less important for $Q$, except if the discount factor is 1 (or higher, which seems crazy).

Boltzmann gives us a way of deciding when to use a certain action, gradually, when the Q-value for some $a$ has increased up to the point it is much greater than the rest, we will most likely choose that action. So we are slowly starting to use the information we've learned. If the Q-value for all actions are still pretty much the same, the probability of choosing some action is pretty much the same for all actions, i.e., we are still exploring.

*Hint to recognize curse of dimensionality: try to visualize a table containing all information we are trying to learn, if the table size increases exponentially when adding some attribute, we have the curse.*

**Question:** Generalization in reinforcement learning: assigning labels to examples: similar to classification or something similar?  
**Answer:** It might be, yet in the lecture they are simply generating a model to represent the Q-function (rather than a table). Doing this is the cost we have to pay to get rid of the curse of dimensionality.

13 Inductive Logic Programming
-----

Deduction proofs new stuff from existing information while preserving truth, induction tries to generalize (possibly making mistakes in the process).

> **Multi-instance setting** := one example contains multiple "rows"/"data"/instances.

The multi-instance setting is more complex than a simple attribute-value setting, example of more complex settings: examples are graphs, sets, ...

**Question:** Is the setting where examples are graphs, sets, ... an example of the multi-instance setting? Or is this yet another setting?
**Answer:** Yes? Because a graph can be represented as a list of tuples?

Soccer example: using a table is not a very good representation because it doesn't generalize well, i.e., *it's hard to perform induction on it* (it's way too specific).

If we can represent the examples in an attribute-value setting then we should do this, because there are some very powerful techniques in this setting.

> **ILP** := learning a theory/"logic program" from data. We can also look at a logic program as a more expressive form of rule sets.

**Slide 20:** In the example Y is universally quantified in the first part of the logical implication, this becomes an existential quantification if we consider $A \implies B \iff \neg A \lor B$.

We can consider a theory of grounded horn clauses as a rule set, this would also mean that a theory of horn clauses can then represent a (possibly infinite) class of rule sets, which (pretty much) confirms that a theory of horn clauses is more expressive than rule sets (we can only know this for sure if we find a class of rule sets that cannot be expressed in a single rule set, this is trivially true if a rule set is finite, considering the class might be infinite). So part of the power of horn clauses is in the fact that they do not have to be grounded, i.e., in the use of variables.

We try to define induction as the inverse of $|-$ (where $|-$ is a deductive proof procedure, e.g. Prolog's resolution).
Based on the exact deductive proof procedure we get different kinds of inductive procedures.

In $\theta$-subsumption a clause $c1$ is more general than another clause $c2$ if there exists a variable substituion \theta such that $c1\theta <= c2$.
Note how we do not define $c1\theta |= c2$, this is because it would make it intractible, so not very useful.

$\theta$-subsumption is not complete in respect to $|=$ (obviously, since not all proofs can be solved by only using variable substitution). So the generality lattice we form by only considering the $\theta$-subsumptions is incomplete, and the lattice itself can be infinite (constantly repeating the same variable substitution).

With the "minimal specialization and minimal generalisation" we try to find specializations and generalizations *that do not skip any possible intermediate clause*. This is the real difficulty when defining the version space algorithm for logical induction.

> **Term** := Literal | Variable | Functor [Term]

> **Inverting resolution:** The inverse of resolution (as defined in Prolog), there are 4 operators for this (used in the version space algorithm).

We need human help when performing inverse resolution, because the version space is too big, something like this is called **active learning**.

**FOIL:** Start from the most general case (`P(x, y) :- true`) and keep on adding predicates to the right hand side, until we converge (or cannot learn more).

In **FOIL** we should try to avoid useless clauses (say, tautologies).

Introduction of new literals (by introduction of new variables) might not increase our rules, yet they might be important and can provide interesting benefits in the long run, yet they are most likely not chosen (since it doesn't introduce an immediate advantage). FOIL promotes this introduction by providing a bonus to those kind of clauses.

In **Golem**, if the provided examples are in separate clusters, all examples in the area in between those two examples might be considered true, we do not want this, instead we want the example to be in the same cluster. We solve this problem by retrying with different examples a couple of times.

**Abduction:** Introduce a fact that explains a given fact given the background knowledge.

To reduce hypothesis space in Progol, we introduce types, e.g., if we have `Y < 18` we assume `Y` is a number.

