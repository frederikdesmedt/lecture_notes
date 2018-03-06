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

Heuristics that are a good idea for N-queens might also be a good idea for Sudoku?

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

Delaying constraints is the first step towards more sophisticated constraint programming.

suspended arithmetic constraints: `$` for constraints on reals, `#` for integer constraints.

> Variable declaration := Assigning a domain to the variable.

In variable declaration with `suspend` `0..9` is an integer interval because the bounds are integers.

Reified constraints are constraints in which a Boolean variable is introduced such that the value of this variable shows whether the constraint is true or false, i.e.,
making the truth value more concrete which is called reification.

> Reification := turn something abstract into something concrete, e.g., in a reified constraint, the truth value of the constraint is put into a variable, rather than being an abstract truth value used by the backpropagation mechanism.

> `suspend/3` := First argument: clause to suspend, second argument: priority (in case there are multiple constraints that should be woken up), third argument: the condition on which the constraint should be waken up, e.g. when some variable is grounded.

diff_list exercise:

```prolog
:- library(suspend).

diff_list(List) :-
    (fromto(List, [El|Rest], Rest, [])
     do foreach(F, Rest), param(El)
        do El #\= F)
```

We cannot implement `diff_list` by using `member/2` because it is not a constraint, it's a Prolog-predicate, we want to have a collection of constraints.

Lecture 3: Active constraints
-----------------------------

> Active constraint := Constraint that prunes impossible assignments from a variable's domain.

> Propagating bound information := constraint propagation concerning the bounds of variables.
>
> Arc consistency for disequality := remove values from the domain for which there is an inequality in your constraint list. Note, inequalities can also cause propagation of bound information.

Different *kinds* of propagation:

- > Forward checking := When a variable `X` is assigned a value, for each variable `Y` that shares a constraint with `X` its assignments are checked, this keeps on propagating.
- > Bounds consistency := A kind of propagation that checks whether there can stil be an assignment for a variable in between its lower and upper bounds.
- > Domain consistency := A kind of propagation that checks whether for each assignment of a variable `X` there is a consistent value in the domain of a variable `Y` which shares a constraint with `X`.

> `alldifferent/1` := Constraint that checks whether the argument is a list of all different values, several different versions: trade-off between time spend on
producing constraints and time spend on searching.

Lecture 4: Active constraints
-----------------------------

Need $n$ `param`s in `foreach` or `fromto`? Use `param/n` (you don't have to use $n$ separate `param`s)!

> `squash/3` := First argument: list of continuous variables, second argument is the neighbourhood in which to test, e.g., squash with 0.1 will keep on increasing or decreasing the lower and upper bound until it finds possible assignments.

One should be able to give the choicepoints to the search itself instead of giving it to Prolog, this can be used to optimize more.

The output variable in the reified version of constraints will be a number (0 or 1). One can perform arithmetic on these output variables to enforce the constraints to
either be true, false, or any combination of the two. Note how the reified constraint does not express a constraint by itself, you should also put a constraint on the
reified output variable in order for it to do something!

> `or/2` := A disjunctive constraint such that one of the parameter constraints should be satisfied. One can implement it by converting the constraint to reified
constraints and then constraining that the sum of the two is #>= 1 (this is also how it is implemented in IC!)

> Shallow backtracking := Only backtrack over the current variable, if all constraints are satisfied for the current variable the value assignment is final!

> Labeling := Will loop through the variables assigning them to values in their domains, starting from small to big. Has a very simple kind of backpropagation. How is
it simple? -> It uses a default variable ordering, but this might be optimized, e.g., if a variable only has a remaining domain with one variable, then choose this one!

Middle-out heuristic works well for N-queens because deciding a variable for the middle columns will produce more constraints! 

Lecture 4: ilog
---------------

> Viewpoint := Tuple with a variable list and a list of the domains of the variables (`viewpoint(Vars, Domains)`).

Different viewpoints can provide different advantages, e.g., better propagation, number of constraints, ...

> Permutation problem := A problem that has a viewpoint such that the union of the domains have the same number of elements as there are variables and each variable must be assigned a different value (`alldifferent(Vars)`).

> `sum/1` := Sum of list elements.

"+" in front of argument name -> input, "-" in front of argument name -> output.

> Channeling constraints := Constraints that combine the constraints between two different viewpoints. This allows different viewpoints to be combined, this way we could
represent the constraints in any of the two viewpoints!

How can we break symmetry (making sure there are no (multiple) symmetric solutions):

- Reformulating the problem;
- Symmetry breaking constraints;
- Use a search procedure that performs symmetry breaking.

> Lex-Leader := Create a lexicographic ordering and only return the solution that is smaller (according to the lexicographic ordering) than all the other symmetric variants.
