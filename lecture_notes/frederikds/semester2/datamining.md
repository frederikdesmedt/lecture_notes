Data Mining
===========

Overall information
-------------------

Try to not go to the exercise session of Wednesday (historically more crowded).

Additional readings seem to be important.

Exam: written, closed book

Lecture 1: Introduction
-----------------------

Data mining seems to have a very practical use and definition and seems to be more like a practice, rather than a research topic, using a bunch of topics
such as machine learning, statistics, etc.

> Retrospective study := Study that analyses cause of a previously known consequence (prospective <-> retrospective).

Observational data: not collected from some scientific process

Goals of data mining:

- Understanding data;
- Extracting (new) knowledge from data;
- Predicting the future.

Data mining scales more than machine learning by considering difficulties of scaling up:

- What happens when data is not available over RAM?
- How can data mining techniques be implemented over a distributed network?
- etc.

Descriptive <-> predictive modeling is the same as descriptive <-> predictive modeling in ML, yet a data mining model can also be a statistical model.

> ? Focus in predictive modeling more on accuracy than on comprehensibility, yet comprehensibility is one of the major goals of data mining, so is predictive
modeling less relevant than descriptive modeling in data mining?

Feature construction is the most important step in data mining!

Easy to make mistakes in preprocessing phase, yet in general lots of issues can arise in the data mining process -> redo it multiple times.

> Data mining process := Data selection -> preprocessing -> data mining algorithms (e.g. ML) -> interpret/evaluate

Example of graph pattern: if A is friends with B and B is friends with C, how often will A be friends with C?

Lecture 2: Get to Know Your Data
--------------------------------

Getting to know your data + preprocessing: Most critical part of the pipeline.

Having the controls in collection of prospective data is key, i.e., making sure the only changing variables are the ones we want to be able to change.

Retrospective studies are used for finding correlations. It is based on observational data, which makes it cheap to do.

Prospective studies are used for finding causations. There is an explicit data collection step in the research, which is very expensive and time intensive.

Data mining are mainly retrospective studies.

In prospective studies there are two groups: the control group and treatment group. They (should) differ in a very precise manner (say, a single variable),
if there is a change in the outcomes of the two groups, the change must either be caused by the changing variable or by chance.
How to decide which one it is? Use statistics for this (formal hypothesis testing).

> p-value := **probability** the data is "generated from" the null hypothesis.

> Type 1-error := Rejecting the $H_0$ while it is correct, probability of this error is $\alpha$.
>
> Type 2-error := Accepting $H_0$ while it is incorrect, probability of this error is $\beta$.
>
> Power = $1 - \beta$ := Probability that the alternative hypothesis is found **if it exists**.

A simple proof of why Power = $1 - \beta$:

$\lnot (type2err) \Leftrightarrow \lnot (\lnot reject \wedge H_0 = false) \Leftrightarrow H_0 \neq false \vee reject \Leftrightarrow H_0 = false \Rightarrow reject$, i.e., the negation of a *Type 2-error* is what *power* should cover and thus $Power = P(\lnot type2err) = 1 - P(type2err) = 1 - \beta$.

From this insight it is obvious that *power* is a very intuitive measure and how it relates to a *Type 2-error*.

> Overall Evaluation Criteria (OEC) := Metric we are measuring and use in the *formal hypothesis test*, should be fixed, needs to be representative, ...
>
> Effect size := *OEC(treatement group)* - *OEC(control group)*

> Newness effect := People might just look around on a website more, simply to look at the new interface, it doesn't mean the interface is better.

Transaction data can be viewed as a huge sparse vector with binary components.

> Class skewness := How uniformly are the classes distributed? E.g. are there many more positives than negatives?

Insight in the domain you are mining is very important, you need this to explain some phenomena in your data.

In Simpson's paradox, trends can change because the sample set changes.

Simpson's paradox is caused by different class distributions, e.g., the paradox in the UC-Berkeley example occurs because the distribution of women over different
departments is different than the distribution of men. This paradox could be recognized/avoided by using ratios rather than percentages or similar (or in general:
by using something that also holds the information concerning the class distribution).

> Censored data := you do not get any data in some case, e.g., you do not get data from before or after some timepoint.

> IQR = $Q_3 - Q_1$ := Interquartile range

Lecture 2: Predictive modelling
-------------------------------

> Model class := Class of all possible instantiations of a model.

We use gradient descent instead of finding least-squared solutions because it is faster/more performant.

Exercise session 1
------------------

When you need to calculate some data, e.g., get the month from the year, then consider adding a column for "month", you can do this as follows:
`data['hour'] = data['datetime'].dt.hour`.

`factorplot` for the win.

**Crucial** insight: look at a `Dataframe` as a dict of `Series`, this way the API makes sense.

Lecture 3: Predictive modelling
-------------------------------

Predictive learning can be seen as: (1) Choose a model for the prediction function (2) Choose a loss function that must be minimized
(3) Tune the parameters of the model to minimize the loss function applied over some training (and possibly validation) set.

We use convex functions in optimizations because they have "nice mathematical properties", e.g., the convex function has one local (and therefore global)
minimum/maximum. To see this, look at how gradient descent evolves in a convex function. Example of convex function: squared sum.

Gradient descent: go towards the negation of the gradient, which is the direction of greatest decrease.

> Regularization := Introducing a penalty on the size of the weights. This way, one can encode a preference for "simpler" models that ignore outliers. Useful for the tradeoff between faithfully fitting the data and having an easy (non-complex) model.

Regularization can reduce variance and 

The tradeoff between model complexity and training loss is a classical tradeoff that occurs pretty much always.

> L2 := Make sure squared sum of weights is below some threshold.
>
> L1 := Make sure sum of absolute weights is below some threshold.

L1 is more popular than L2 right now, LASSO allows the model to select which features it needs because it has a preference for weights close to 0.
Why does L1 have a bias for 0-weights? Look at the visualizations in the slides (the one with the square).

LASSO selects one correlated variable (at random) from a group of correlated variables, because it can calculate the other correlated variables from it.

> Elastic Net Regularization := Combine L1 and L2 penalties.

When using L1 or L2, one needs to try to optimize the hyperparameter $\lambda$: do this using cross-validation.

One shouldn't use gradient descent when using L1 because it causes minimization function to not be convex.

Logistic regression is useful for *scoring problems*.

> Scoring problem := Finding the probability of something happening, i.e., $E[Y \mid X]$, e.g., the probability a client will suitably pay his/her loan,
> probability of a customer spending money, ...

Logisitic regression allows finding a probability distribution with numerical input!

> Logistic regression := sigmoid of linear combination of input data, which can be found by maximizing some function, e.g., the sum of the conditional probabilities.

Encoding discrete variables in logistic regression can be done by using $k - 1$ binary variables, rather than one integer variable from 0 to $k - 1$.
Why? Because it allows more complex model learning since the examples are represented in higher dimensions.

Naïve Bayes and logistic regression can (only) express the exact same hypotheses, they are equally expressive. Yet there is a difference in bias.

Naïve Bayes tends to set probabilities towards 0 and 1, logistic regression however is well calibrated.

With calibration curves, being on the diagonal is the best. Why? X signifies the predicted probability of being positive, Y signifies the actual probability
according to the examples. If on the diagonal, both perfectly align.

> Concave := *inverse* of convex

Usually L1- or L2-regularization is applied on top of logistic regression.

The flu prediction challenge is a kind of problem that can be on the exam.

> Confounding variables := Variables that are not considered, but are correlated with both the target variable and the predictor variable(s).

When using temporal data, you usually do not want to do regular cross-validation, instead use some kind of cross-validation that respects time, e.g., do not simply skip a year of data.

> ? How would one combine logistic regresssion and decision trees? Perhaps by using a decision tree with logistic regression models as leaves? This could increase expressivity of the (combined) model a lot!

Linear regression -> real-value, scoring -> logistic regression

Lecture 4: Recommender systems
------------------------------

In recommender systems we do not necessarily want to show popular products, because they should already be easily accessible, instead recommend products that
cannot easily be found, i.e., that aren't popular, but that have a high probability of being bought by the current user.

Different kinds of recommender systems:

- Content-based filtering
- Collaborative filtering
- Hybrid: content-based + collaborative filtering

The model build by ML is a *user profile*.

Feature representation problem: how do we represent items?

> Item profile := Description of an item.

Content-based filtering can be seen as a classification algorithm, i.e., based on features that indicate user preference predict the likes and dislikes
of the user. This is nice, because there is lots of research going on for classification problems, but bad because it doesn't use information from other users.

> ? Why does content-based filtering not consider information about other users? Couldn't it be used when constructing the user profile?
> -> Because this approach is called collaborative filtering.

In collaborative filtering, the user profile is compared to other user profiles based on their preferences, with the idea that users having a similar 
like/dislike-history will also like and dislike the same items in the future.

A general approach of collaborative filtering:

- Define a similarity function between two user profiles (where a user profile is a feature vector with 1's and 0's to indicate whether they liked some items).
- Optionally find prototypes in the user space.
- Perform k-Nearest neighbours in the user space (the new averaged vector is a weighted vector based on Pearson-correlation, look at the slides).
- Return the highest rated items in the averaged feature vector.

Cosine similarity can often return zero, because the user-product matrix is so sparse, even more, simply considering every value that isn't filled in to be zero
is not a good idea.

Root mean square error doesn't focus on highly rated items, e.g., if an item actually has rating 1, but is predicted to have 3 is not that big of a deal
as when the item actually has rating 5, but is predicted a 2! The former case is more desirable than the latter because the item will most likely not be
recommended anyway.

Focusing on accuracy is not necessarily a good idea, because you are missing a lot of other interesting evaluation measures, e.g., order of prediction, diversity, ...

Prediction diversity: when predicting movies, don't just predict 10 action movies.

Rating scale of user: e.g., user only rates between 3 to 5 stars, some only between 2 to 4 stars, ...

> Latent factor models := Just like in IR, consider a latent topic variable. Factorize the user-movie co-occurence matrix by it. This is found by a minimization
> problem, e.g., by minimizing the mean squared error. You can do this by (stochastic) gradient descent.

Lecture 5: Recommender systems
------------------------------

> "Kitchen Sink Approach" := Build lots and lots of predictors and combine the results of these.

Take away of Netflix-challenge: Try to create a baseline that encompasses as much as possible, e.g., the more complex baseline with average user rating and
average movie rating, rather than simply using the overall average rating.

Lecture 6: Model Ensembles
--------------------------

If the error of each classifier in a set of classifiers is independent of the errors of other classifiers, combining them should always yield an improvement.
This can be proven with probability theory.

> Unstable learner := Learners where a small change in training data results in major changes in the resulting model, e.g., decision trees, neural nets, ...

Unstable learners can be used with subsampling, e.g., make minor changes in the training data.

> Bagging := Randomly sample examples with replacement from training set. Do this $T$ times, the result is $T$ quite different datasets that still hold the same
> information. However, when using an unstable learner the $T$ models generated can still be very different.
>
> Boosting (AdaBoost) := Serial process: learn a model from data, from it, look at the kind of examples that are predicted poorly, increase
> the weight of these examples, i.e., make them more important. Then create a new model from the weighted data and keep on doing this.

**Occam's razor doesn't seem to apply with boosting!**

**Margin** can be used to measure the confidence of a classification, this can be used to calculate a weighted majority vote in boosting.

**Gradient Tree Boosting** is the hottest ensemble algorithm at this point in time.

> Gradient boosting := Gradient descent + boosting. Gradients come in to represent the shortcomings of a new model.
>
> Gradient tree boosting := Gradient boosting with decision trees as a model in each stage (most common form of gradient boosting).

In gradient tree boosting you want to learn a simple model at each step (such that we don't overfit at some step), this could be by using decision trees.
It's ok if the model isn't very accurate, because the errors are corrected by some other future decision tree.

You can plug in every possible (differentiable) loss function in **gradient boosting**, which makes it very generic and useful in general.

> Manipulating input features := Drop random features in datasets to create multiple datasets. Only good when features are highly connected, i.e., we don't lose
> information when removing a feature.

Random forests is all about finding a good hyperparameter $i$.

Lecture 7: Model ensembles
--------------------------

> Stacking := Construct a learning problem to learn when each learner is good. The training data is the prediction of each learner labeled with the expected value.
> The meta learner will learn from this training data, minimizing some loss function.

Why do ensembles work so well?

- The expected error considering data points are drawn from some probability distribution is small.
- Can be useful to address both bias and variance.

Bias can be caused by:

- The inability to express decision boundary;
- Wrong assumptions about data;
- Classifier is too global, i.e., not expressive enough.

> Variance := Small changes in the dataset cause these changes to be modelled, while we might not want this.

High bias -> underfitting. High variance -> overfitting. So we need a trade-off between bias and variance, this trade-off can be changed by considering
more/less expressive models.

> Bayes optimal classifier := Take a weighted vote over all possible hypotheses. The Bayes optimal classifier is proven to be the best possible classifier.

Bayes optimal classifier cannot be used in general because of the size of the hypothesis space, boosting, however, is a way to approximate Bayes optimal classifier.

Lecture 7: Association Rule Mining
----------------------------------

> Association rule learning := Input is a set of transactions, a transaction is an itemset bought at the same time. An itemset is a set of items.
> Model is a set of if-then-rules that go from an itemset to another itemset that are associated with the input items. There is no labeled data (nor is it necessary)!

Finding frequent pairs is the hardest problem in this setting. Especially considering the I/O-overhead you can have, considering association rule mining
can be performed in big data.

> **Apriori** := Algorithm that allows learning frequent itemsets for size $k$. Main principle: if we have a non-frequent tuple of size $k$, then adding any number
> of items to the tuple will still result in a non-frequent tuple. So, when looking for frequent tuples/itemsets of size $k + 1$, we only need to consider those
> that add a component to a frequent tuple of size $k$.

Idea behind **Apriori**: if $\{1, 2\}$ is not frequent then $\{1, 2,3\}$ can never be frequent.

> **PCY** := Extension of **Apriori** with hash filtering. The hash filter holds the count of the itemset, if the count at a location $l$ is below
> the support threshold, we can get rid of all elements that hash to location $l$ (this is the best-case scenario).

> ? Hash filtering in **PCY** happens right after constructing tuples of size $k + 1$ in **Apriori** (during joining)?
>
> Answer: At that time the hash filter (bitstring) is constructed, yet it's at the pruning-phase (right after **Apriori**-pruning) that itemsets are removed
> that are infrequent according to the bitstring.

> Hash filtering := Solution for problem of finding a value in a large collection. Constructs a bit array, putting 1's on the locations of the hashes of the items
> we are looking for. For each item in the dataset, if there is a zero on the location of the hash of the item, we are not looking for it.
> No false negatives, but there are false positives, we can reduce the number of false positives by increasing bit array size.

Lecture 8: Association Rule Mining
----------------------------------

> Sample algorithm := Fill memory with samples of dataset and perform **Apriori** on the sample set. This is much faster because it fits completely in memory.

By using sampling it's possible to not find frequent itemsets (because they are frequent in the dataset but not in the sample set).

With the sample algorithm: make sure that you scale the support to the partition size (don't require as much occurences as you would require for the entire dataset).

The monotonicity in **SON** works because of the scaled support threshold, e.g., if support threshold is 8 and there are 2 partitions, then support threshold
in partitions is 4, if an itemset occurs >=8 times, then it will be present >=4 times in at least one of the partitions.

> **Negative border** in **Toivonen's algorithm** := An itemset is in the negative border if and only if all subsets of the itemset are frequent in the sample.

> **FP tree** (Frequent-Pattern tree) := A compressed version of the dataset that can (hopefully) completely fit in memory.

Finding an optimal ordering for construction of the **FP tree** is intractable, so we use heuristics.

There is an implicit constraint in every association rule mining algorithm: *number of occurences >= support threshold*.

**MaxMiner** works similar to **Apriori**.

Lecture 9: Sequential Pattern Mining
------------------------------------

Mining sequences is similar to mining transactions but the temporal component is considered in your model.

> Sequence := An ordered list of transactions, the length of the sequence is the number of transactions, not the number of items (within the transactions).
>
> Support of sequence := **Fraction** of users/customers that support the sequence.

> FP-Growth := The main hypothesis that **Apriori** uses

You can construct all prefixes of a sequence by simply adding a new item to the sequence from left to right when looking at the notation for a sequence in the slides.

The algorithms seen in this lecture are all about *divide-and-conquer*.

> Pseudo-projection := Pointer to the initial sequence with some offset to point to a projection of that sequence: much less memory loss.

Lecture 9: Clustering
---------------------

> Intra-class similarity := Similarity between different objects within the same group/cluster.
>
> Inter-class similarity := Similarity between objects of a different group/cluster.
