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

> `squash/3` := First argument: list of continuous variables, second argument is the interval of the neighbourhood in which to test,
> e.g., squash with 0.1 will keep on increasing or decreasing the lower and upper bound until it finds possible assignments within the neighbourhood.

One should be able to give the choicepoints to the search itself instead of giving it to Prolog in the case of disjunctive constraints,
this can be used to optimize more.

The output variable in the reified version of constraints will be a number (0 or 1). One can perform arithmetic on these output variables to enforce
the constraints to either be true, false, or any combination of the two. Note how the reified constraint does not express a constraint by itself,
you should also put a constraint on the reified output variable in order for it to do something!

Arithmetic on the output variables of reified constraints can be used to implement disjunctive constraints.

> `or/2` := A disjunctive constraint such that one of the parameter constraints should be satisfied. One can implement
> it by converting the constraint to reified constraints and then constraining that the sum of the two is #>= 1
> (this is also how it is implemented in IC!)

> Backtrack-free search := Select the minimal value for each variable in the search tree, never backtrack. This is (obviously) a kind of incomplete search.
>
> Shallow backtracking := Only backtrack over the current variable, if all constraints are satisfied for the current variable the value assignment is final!
> Also a kind of incomplete search, but can actually find solutions to some kinds of problems, e.g., SMM.
>
> Labeling := (Basic) search with backtracking, will loop through the variables assigning them to values in their domains, starting from small to big.
> Has a very simple kind of backpropagation. How is it simple? -> It uses a default variable ordering, yet this can be optimized, e.g., if a variable
> only has a remaining domain with one variable, then choose this one! So we can provide various heuristics to improve the choices labeling makes.

Different kinds of heuristics:

- > Naive variable-ordering heuristic: Label variables in the order in which they are provided.
- > Middle-out heuristic (static): Label variables in the middle first, e.g., start with the centered columns/rows in N-queens.
- > First_fail heuristic (dynamic): Label variables with the smallest domain first, intuitively it focuses more on bottlenecks. Works well with forward-checking propagation. In general, works almost always.
- > Middle-out value ordering heuristic: Similar to middle-out variable heuristic, but for values instead of variables.

Middle-out heuristic works well for N-queens because deciding a variable for the middle columns will produce more constraints! 

Lecture 4: ilog
---------------

If a problem P must be solved using constraint processing, the problem must be converted to a *Constraint Satisfaction Problem*. Which means the objects part of
the problem must be expressed in terms of variables and values. The exact variables and values one decides to use defines the viewpoint of the problem.

> Viewpoint := Tuple with a variable list and a list of the domains of the variables (`viewpoint(Vars, Domains)`).

Different viewpoints can provide different advantages, e.g., better propagation, number of constraints, ...

> Permutation problem := A problem with a viewpoint such that the union of the domains have the same number of elements as there are variables and
> each variable must be assigned a different value (`alldifferent(Vars)`).

In a permutation problem one can switch the role of the variables and values, e.g., in the set-of-columns-representation of N-queens the vars are the columns
and the values are the rows, yet it can also be reversed.

> `sum/1` := Sum of list elements.

"+" in front of argument name -> input, "-" in front of argument name -> output.

> Channeling constraints := Constraints that combine the constraints between two different viewpoints. This allows different viewpoints to be combined,
> this way we could represent the constraints in any of the two viewpoints!

Channeling arises trivially when the CSP is a permutation problem.

When adding channeling constraints for two viewpoints of a permutation problem, the `alldifferent`-constraints are inferred, i.e., they no longer have to be
mentioned explicitly.

> Symmetry := Can be seen as a property of (1) the solution set: multiple elements in the solution set are symmetric, (2) the statement of the problem: the
> problem is defined in such a way that it allows symmetric solutions, (3) the values (of a variable): they can be permuted.

Because of the different kinds of symmetry (look at definition above) we can break symmetry (making sure there are no (multiple) symmetric solutions) in
different ways:

- Reformulate the problem;
- Add symmetry breaking constraints;
- Use a search procedure that performs symmetry breaking.

One can reformulate the problem by introducing the notion of a set, rather than a list, since a set does not have ordering and thus all symmetric lists
(lists that are the same, but with a different ordering) are not considered. But propagation for sets is a difficult problem, so it's not used a lot, in general:
no known general technique.

> Lex-Leader := Create a lexicographic ordering and only return the solution that is smaller (according to the lexicographic ordering) than all the other
> symmetric variants.

Lex-Leader can be implemented in the search procedure: look at a branch for an assignment `var=val`, after that, go to a different branch and add the additional
constraint that `var=val` will never occur in that subtree (look at last slide). This gets rid of symmetric variants.

Lecture 5: Optimisation with active constraints
-----------------------------------------------

> All-solution method := The CSP-algorithm

> `minimize/2` := First argument is the goal (e.g. the search procedure), the second argument is the cost of a specific assignment, this is what is minimized.
>
> Branch-and-bound := COP-technique that searches for a solution, then a constraint is added that specifies that the cost must be smaller than or equal to
> the cost of the solution found.

Maximization of a cost/score can be expressed as the minimization of the negation of that cost/score.

`Value $= sum(List)` is syntactic sugar for $sum(List, Sum), Value $= Sum$.

> `eval/1` := First argument is a variable that at runtime, will evaluate to a symbolic expression (a formula), `eval(Expr)`
> can then be used in any kind of constraint, e.g., in `eval(Expr) #= 5`. So `eval/1` allows constraints to be compiled
> at runtime.

> Dichotomic branch-and-bound: In a minimalization problem when a solution is found, this solution is considered an upper bound, if we also have a lower bound, e.g., the cost of a solution 
> of the relaxed version of the problem. Then we split the search space in two version, the one with cost in $[lower + (higher - lower) / 2, higher]$ and one in the interval
$[lower, lower + (higher - lower) / 2)$.

> `bb_min` := Branch-and-bound minimization

Often similarity breaking can be expressed by simply adding `<=`-constraints to the CSP.

Lecture 5: Constraints on reals
-------------------------------

Different settings of constraint problems with reals:

- Continuous variables entirely dependent on finite domain variables: search problem can just assign values to the finite domain variables and calculate the continuous variables from it. Very nice property is that the search space does not explode because of an infinite domain.
- Continuous variables not entirely dependent on finite domain variables, but with finite search space, e.g., when the constraints are expressed in such a way that only a couple of real values can be considered (look at the "Intersection of 2 circles"-example in the slides).
- Continuous variables with infinite domain. Cannot perform discrete search algorithms, e.g., labeling. Constraint propagation must be reimplemented, e.g., by using *safe arithmetic*. Try to use integrals as much as possible (more safe). Continuous variables are often not ground (they can still be different values).

How to represent reals?

- Floating-point numbers: a single value, but finite precision, rounding errors, ...
- Interval of floating-points: real number is guaranteed to be somewhere inside the interval.

> `incons/3` := First 2 arguments are two real variables, the third argument is a real variable indicating the difference between the two. Continuously applying the constraints makes `incons`
> become inconsistent. When it does not become inconsistent, it means that adding the difference has no influence on the values of the two variables, i.e., the sum cannot be represented in a
> finite (double floating-point) representation. This continuous process is called *shaving*. It can be used to express a constraint where a real variable only changes the moment it is
> increased by some finite amount greater than some threshold, this threshold is called the *propagation threshold*. This is important for efficiency during propagation, higher threshold:
> more efficient searching, but lower precision, lower threshold: the opposite of higher threshold.

Splitting domain: add a disjunctive constraint specifying the variable must be in one half of the domain, with some luck the problem will turn out to be inconsistent in one half of the domain,
i.e., we can reduce the interval by only considering the remaining half. Keep on doing this until both halves succeed, or until some predetermined *precision* is reached.

> Shaving := excluding a small part of the interval located at the bounds, i.e., increasing the lower bound by a very small amount.

> Locate := Keep on domain splitting until the specified *precision* is reached.
>
> Squash := Keep on shaving the domain interval until we get actual solutions, it shaves stepping over a predefined step.

Interval splitting = exponential, shaving = polynomial.

Lecture 6: eplex
----------------

We have to "manually" invoke the *eplex*-solver. This is not done automatically, such as with the constraints defined in *ic*. You can decide when
the eplex-solver should be invoked by using `eplex_solver_setup`.

> MIP branching := If we first find the solution with reals, e.g., $X = 4.2$ , we can create two branches, one where $X <= 4$ and one with $X >= 5$ with
> integrality constraints on $X$ (`integers([X])`). Keep on doing this for all variables.

> Big M constraint := A kind of constraint of the form: $M \cdot Bool >= Expr$, which can be used to express if-then-else constraints in linear constraints.

Lecture 6: CHR
--------------

> Simplification rule (`H <=> B`) := If all terms in the head are part of the constraint store, they are removed and the terms of the body are added.
>
> Simpagation rule (`H1 \ H2 <=> B`) := Similar to the simplification rule, except that all terms in the head before the backslash are kept in the constraint store.
>
> Propagation rule (`H ==> B`) := All terms in the head stay in the constraint store, the terms of the body are added to the constraint store.

The only difference between the different rules is about what happens with the terms in the head.

In a multi-headed constraints all terms in head should be a member of the constraint store, i.e., `,` signifies a conjunction. Similar with the body.

Simplification rules are tried/performed in the order in which they are defined in the code, e.g., in the Prolog source code.

> ? What happens if a simplification rule is applied: does the simplification rule repeat, does it stop, or does it continue with the next?
> We can perform an experiment to figure this out by applying a simplification rule that can be repeated multiple times after each other.
> *The slides that extensively explain the gcd-example seems to imply that it completely restarts every time.* It's completely explained in the
> "operational semantics"-slides.

Matching heads of rules is not by unification, a head only applies if all ground subterms in the head are also ground in the input. Look at the `and/3` example
in the slides. You could look at it like "one-way unification".

> Guards (`H <=> Guard | B`, `H1 \ H2 <=> Guard | B`, `H ==> Guard | B`) := Additional constraints the head should satisfy.

Lecture 7: CHR
--------------

In CHR, a state is a list of constraints, i.e., a constraint store.

Operation semantics:

- Rule application is applied from top to bottom.
- Rule application completely restarts when new rules are added, to avoid that the system starts looping, a rule with the same constraints is only executed once.
- A rule is applicable if it can unidirectionally unify with the head and the guard constraints are satisfied.

> Unidirectional unification := Unification only of the form $rs = rh\Theta$. Where $rs$ is a rule from the store and $rh$ is a rule from a head, i.e., $rh$ can
> never be more specific than $rs$ anywhere in the rule.

**Never put `chr_show_store` in the body of a rule!**

CHR can be used to construct constraints, while still using a search, which will awaken constraints in the constraint store.

> ? When an `@` occurs in the rule, the text before it is considered the name of that rule?

CHR does not backtrack, this must be done in Prolog.

**Prolog backtracking undoes CHR changes!**

In general, using phases in CHR is a very good programming technique.

> ? CHR-constraints can be viewed as regular Prolog-predicates when using them, they are simply computed differently (not with resolution, but with CHR-rules).

Lecture 7: CHR (CHR solver programs)
------------------------------------

**Propagation** only happens **once** on the same head/rule.
