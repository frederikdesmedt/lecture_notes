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

We use convex functions in optimizations because they have "nice mathematical properties". To see this, look at how gradient descent evolves
in a convex function. Example of convex function: squared sum.

Gradient descent: go towards the negation of the gradient.

> Regularization := Introducing a penalty on the size of the weights. This way, one can encode a preference for "simpler" models that ignore outliers. Useful for the tradeoff between faithfully fitting the data and having an easy (non-complex) model.

The tradeoff between model complexity and training loss is a classical tradeoff that occurs pretty much always.

> L2 := Make sure squared sum of weights is below some threshold.
>
> L1 := Make sure sum of absolute weights is below some threshold.

L1 is more popular than L2 right now, LASSO allows the model to select which features it needs because it has a preference for weights close to 0. This is why
L1/LASSO is more popular right now. Why does L1 have a bias for 0-weights? Look at the visualizations in the slides (the one with the square).

LASSO selects one correlated variable (at random) from a group of correlated variables, because it can calculate the other correlated variables from it.

> Elastic Net Regularization := Combine L1 and L2 penalties.

Logistic regression is useful for *scoring problems*.

> Scoring problem := Finding the probability of something happening, e.g., the probability a client will suitably pay his/her loan, probability of a customer spending money, ...

Logisitic regression allows finding a probability distribution with numerical input!

> Logistic regression := sigmoid of linear combination of input data, which can be found by maximizing some function, e.g., the sum of the conditional probabilities.

Naïve Bayes and logistic regression can (only) express the exact same hypotheses, they are equally expressive. Yet there is a difference in bias.

Naïve Bayes tends to set probabilities towards 0 and 1, logistic regression however is well calibrated.

> Concave := *inverse* of convex

Usually L1- or L2-regularization is applied on top of logistic regression.

The flu prediction challenge is a kind of problem that can be on the exam.

When using temporal data, you usually do not want to do regular cross-validation, instead use some kind of cross-validation that respects time, e.g., do not simply skip a year of data.

> ? How would one combine logistic regresssion and decision trees? Perhaps by using a decision tree with logistic regression models as leaves? This could increase expressivity of the (combined) model a lot!

Linear regression -> real-value, scoring -> logistic regression