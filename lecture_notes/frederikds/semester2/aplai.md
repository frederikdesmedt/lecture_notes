Advanced Programming Languages in AI
====================================

Overall information
-------------------

There is a Wiki about this course on Toledo.

Materials in slide are sufficient for the exam.

Exam slots are not fixed, they will be all be made available later in the semester (so don't look at the exam roster on KULoket).

The professor really likes the "Constraint Logic Programming using ECLiPSe" book.

To get access to the website of Simonis: use web.archive.org to get an archived version.
Archived version can be found [here](http://web.archive.org/web/20170731071043/http://4c.ucc.ie/~hsimonis/ELearning/index.htm).

Lecture 1
---------

Linear programming: values can be reals, integer programming: values can only be integers (more difficult than linear programming).

Short history: CHIP (Linear programming with finite domain variables + top-down search) -> ECLiPSe (CHIP + database + parallel computing)
-> Later connected to external constraint processing package via interface (no longer high coupling with CHIP?)

> Constraint programming := Expression relations between variables as a set of constraints. The constraints do *not* specify the way a solution must be found,
> but only specify properties of the solution, i.e., constraint programming is a kind of declarative programming. The domain of the variables can be anything
> expressible in bits (so rationals, booleans, ... but not real numbers). Note how a "constraint program" doesn't actually do anything, it's purely descriptive.
>
> Mathematical programming := (LP, IP), finding the optimal solution of a set of linear/integer functions.
>
> Local search := Finding an optimal solution by searching in the solution space, this is an algorithm that can work with a "constraint program".

> Symmetry breaking := If multiple but similar solutions are found, only show one, e.g., sols=[{jan, piet, ann}, {ann, piet, jan}, ...] to sols=[{jan, piet, ann}].
> Branch and bound := Consider a tree with the root the solution set containing *all* solutions. Now consider branches as a strict subset of its parent. Branch and
> bound will start from the root, generate branches from the current node, calculate/estimate upper and lower bounds, discard the ones that do not yield the most
> optimal solutions and visit the remaining branches, repeat.

Occurs check can be explicitly turned on in most Prolog-systems.
> Occurs check := Check whether the head of a clause is in the body of that clause, e.g., `X :- f(.., X, ..)`. Such a clause can be expanded infinitely
by using an `eliminate`-step that does not occurs-check (as defined by Martelli and Montanari).

> Passive constraints := Constraints that will not limit the possible assignments of the variable, instead, the program waits until all variables are assigned
> and only then the constraint is executed, an example: `3*X < Y + 2`.
>
> Active constraint := Restricts the values of the variables, an example of an active constraint is `=/2` (unification).

> ? Active constraints are more useful because they actively reduce the search space during propagation.