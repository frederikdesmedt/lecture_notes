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

Data mining are mainly retrospective studies.

> p-value := **probability** the data is "generated from" the null hypothesis.

> Type 1-error: Finding a (non-null-)hypothesis while it is not there.
>
> Type 2-error: Saying the effect isn't real while it really is.

> Overall Evaluation Criteria (OEC) := Metric we are measuring, should be fixed, needs to be representative, ...

> Newness effect := People might just look around on a website more, simply to look at the new interface, it doesn't mean the interface is better.

Transaction data can be viewed as a huge sparse vector with binary components.

> Class skewness := e.g. are there many more positives than negatives?

Insight in the domain you are mining is very important, you need this to explain some phenomena in your data.

In Simpson's paradox, trends can change because the sample set changes.

> Censored data := you do not get any data in some case, e.g., you do not get data from before or after some timepoint.

> IQR := Interquartile range

Lecture 2: Predictive modelling
-------------------------------

> Model class := Class of all possible instantiations of a model.

We use gradient descent instead of finding least-squared solutions because it is faster/more performant.
