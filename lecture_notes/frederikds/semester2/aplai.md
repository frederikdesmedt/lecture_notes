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

Lecture 1: Introduction
-----------------------

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

Lecture 2: Introduction
-----------------------

A constraint is easy to understand from a set-theoretic POV.

> CSP := Constraint **Satisfaction** Problem
>
> COP := Constraint **Optimization** Problem

Constraint != CSP!

Satisfiability is to a constraint as consistency is to a CSP.

It's possible that a CSP is inconsistent but does not fail.

Interpretation of a constraint is similar to the interpretation of a predicate in logic.

Difference between arithmetic and linear constraints: arithmetic constraints allow non-linear expressions, such as $x * y$.

> Branching := Split CSP's such that the union of the solution sets is the same as the solution set of the original one.
> One (usually) splits the domain of variables when branching, e.g., assigning a value to some variable.

> Propagation := reducing the CSP to a simpler one by applying sound simplifications, e.g., domain reduction.

The combination of branching and propagation allows for a more efficient search tree, make sure to implement backtracking.

Lecture 2: Passive constraints
------------------------------

> Labelling := Branching by variable assignment (restricting domains in branches to singletons for some variable).

We can decide which variable to label first (**variable ordering**), general rule of thumb: label the variable with the smallest domain first.
Why does this work? Imagine having a variable $A$ and $B$ with $\vert dom(A) \vert = 4$ and $\vert dom(B) \vert = 2$. If $A$ is selected first, the number
of internal nodes in the search tree will be 5 (1 for the root and 4 for each assignment of $A$), if $B$ is selected first, there will - similarly -
be 3 internal nodes, the number of leaves must the same. Therefore the size of the search tree is reduced when selecting the variable with the smallest
domain first.

`foreach` and `fromto` are *iterators*, it's build into the language itself (and is optimized at the lower level?).

`fromto` works by passing transforming `In`, unifying it with `Out`, in the next iterator `In` will be the `Out` of the previous iteration until `Out = Rest`.

The examples about combining `fromto` and `foreach` work  because of **synchronous iteration**, i.e., `fromto` and `foreach` are iterated simultaneously.
Whenever a `fromto` or `foreach` goes into the next iteration, so will every other related `fromto` or `foreach`.
> ? When is another `fromto` or `foreach` related? Are they all related?
>
> Currently assuming an iterator is related to another iterator when they are in the same clause, i.e., if they can unify with each others variables.

The minus in `Var-Domain` is an infix functor.

`Search([A-Dom, ...])` will assign each value in `Dom` to `A`, this happens for each var-dom pair supplied in the list.

Incomplete search assumes the best values appear earlier in the domain, from this one can create several strategies:

- N-best := Only consider the $N$ best values of the domain.
- Credit-based := Assign credit to each assignment, a good value should correspond to a high credit, only consider value assignments above some credit threshold.
- Limited discrepancy search := For `lds(n)` allow the algorithm $n$ to "go right" $n$ times (considering the left-most value is the "best").

> `param(A)` := The variable `A` will be constant across all iterations of the loop.

*Limited discrepancy search* is useful when we order the values from left to right according to "value importance".
> Limited Discrepancy Search (`lds(n)`) := You are allowed to go right $n$ times or less, in all the other choices you must go left.

`succeed(Q, N)` works because of `fail`, everytime this `fail` occurs, the system backtracks to `Q` after which the counter will be incremented every time.

Lecture 3: Passive constraints
----------------------------------------------

> Generate := search + labeling
>
> Test := Constraints
>
> Co-routining/interleaving := interleaving between generating and testing
>
> Interleaving in `suspend` library := Gather constraints in a global "constraint store" -> start search -> during search wake up constraints for testing. This is a kind of interleaving.

suspended arithmetic constraints: `$` for constraints on reals, `#` for integer constraints.

In variable declaration with `suspend` `0..9` is an integer interval because the bounds are integers.

Reified constraints are constraints in which a Boolean variable is introduced such that the value of this variable shows whether the constraint is true or false.

> Reification := turn something abstract into something concrete, e.g., in a reified constraint, the truth value of the constraint is put into a variable, rather than being an abstract truth value used by the backpropagation mechanism.

> `suspend/3` := First argument: constraint to suspend, second argument: priority (in case there are multiple constraints that should be woken up), third argument: the condition on which the constraint should be waken up, e.g. when some variable is grounded.

diff_list exercise:

```prolog
:- library(suspend).

diff_list(List) :-
    (fromto(List, [El|Rest], Rest, [])
     do foreach(F, Rest), param(El)
        do El #\= F)
```

We cannot implement `diff_list` by using `member/2` because it is not a constrant, it's a Prolog-predicate, but we want to have a collection of constraints.

Lecture 3: Active constraints
-----------------------------

> Propagating bound information := constraint propagation concerning the bounds of variables.
>
> Arc consistency for disequality := remove values from the domain for which there is an inequality in your constraint list.

> `alldifferent/1` := Global constraint that checks whether the argument is a list of all different values.
