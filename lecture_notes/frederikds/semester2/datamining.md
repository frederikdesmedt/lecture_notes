Data Mining
===========

Overall information
-------------------

Try to not go to the exercise session of Wednesday (historically more crowded).

Additional readings seem to be important.

Exam: written, closed book

Lecture 1
---------

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
