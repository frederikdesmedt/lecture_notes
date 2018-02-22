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
