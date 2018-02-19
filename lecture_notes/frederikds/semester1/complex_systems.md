Modelling of Complex Systems
====================
 
Chapter 6: Refinement in Event-B
--------------------

Guard strengthening: If the guard conditions in the concrete event hold, they also hold in the abstract event (i.e. we only "add" additional stuff in the conjunction).
Action simulation: If the actions of the concrete event are performed, they have the same effect as performing the actions of the abstract event + some additional stuff on top.

> **Question:** Correctness of refinement property 1: How then can we be sure the same actions occur if the preconditions of some event are strengthened? After all, we have guard strengthening.
>... Am I misunderstanding the two correctness properties? TODO: Check with book
>... Yes, this is (a part of) correctness property 2!
> **Answer:** This is indeed not verified, which might be desirable or might be a bug. In other words, some path in the abstraction might not exist in the concrete state graph (this does not satisfy property 2). On the other hand, thanks to guard strengthening and action simulation we can abstract any path in the state graph of the refinement to a path in the abstraction, e.g., by grouping together a couple of states as a single state occuring in the abstract model.

> **Question:** skip = "do nothing event"  
> **Answer:** Yes

Prevent early deadlock in refinements: "disjunction of the guards of new and refined events" is an invariant (G1 or G2 or G3 or ...), i.e., an event should occur every time (since the guard of one of the events has to be specified)
Prevent divergence in refinements: just one of the usual proofs of termination -> assign a natural number to a state and proof that it decrements each time in a loop, by the time it reaches 0 the loop HAS to stop.

? Preventing early deadlock and divergence makes sure correctness property 2 is satisfied?

? In Pidgin language->while implementation: why do we need to do these in two different refinement levels? Is it to make sure not(Q) does not occur all the time? If so, why does this work?
  There's a little note about this right before the conclusion, but it doesn't seem to answer these questions.

ProB still not powerful enough to be used in practice (to model computer programs for example)

Chapter VII: Classical results on Predicate Logic Provability, Expressivity, Decidability of deduction, Incompleteness
-----------------

Axiom schema: A template for a class of formulas, which we can get by substituting some variables, e.g. $\phi \vee \neg \phi$.

> Expressivity analysis := the study of what can and cannot be expressed in some logic.

In expressivity analysis we first of all require a vocabulary, then consider the class $C$ of all possible structures over this vocabulary that satisfy the informal requirements and then try to find out if there exists a formula $\phi$ such that the class of all models of $\phi$ is equal to $C$.

**Exam:** Typical proofs of inexpressivity asked: "The universe is finite", "There is a path in a graph G from A to B" and DCA.

**Question:**Does the *inexpressivity theorem* not contradict the *compactness theorem* because it's talking about consistency rather than satisfiability?

If a theorem is (only) infinitely expressed then it's negation is a good candidate for an inexpressive theorem. It makes sense if we look at the example about proving the inexpressivity of finite domains.

Interesting sidenote about there not existing paths from `A` to `B`: there is never an *infinite* path from `A` to `B`, not because there does not exist an infinite path, but because it will never reach `B` (it's infinite after all)!

Peano's axioms use second-order logic, peano's arithmetic uses an axiom schema, i.e., it has infinitely many axioms generated from the schema.

Note: every reasoning and proof ever made by any mathematician can be expressed in FO, yet it cannot express the natural numbers. This can be explained by looking at the way expressivity is defined (it talks about the entire `Mod(T)`).

Semi-decidability theorem is easy to prove: every proof is finite, i.e., there is a countably infinite amount of proofs. We can (theoretically) construct an algorithm that iterates through this list of proofs, which will end in finite time. From this point-of-view it's already kind of intuitive that it's *only* semi-decidable and not fully decidable.

Gödels incompleteness theorem tells us that $theory(\Nat) = \{\phi | \Nat \entails \phi\}$ is not recursively enumerable. This is why we call the natural numbers undecidable.

The FO(.) Knowledge Base System project
----

Project conducted at KU Leuven (under guidance of prof. De Necker?)

Is declarative knowledge not independent of the task? It should be, but in practice it isn't always the case, e.g., Prolog.

We reduce expressivity of a language to increase tractability of tasks performed on them (deduction, induction, model expansion, ...). Even classical FOL is too expressive at the moment.

Model checking is in $P$, model generation is in $NP$ however, so they are actually very different (computationally fundamentally different).

The course selection example in the lecture is (a solution of) an "interactive configuration problem".

Chapter 8
---

> **DPLL algorithm** := Algorithm based on unit propagation. Decide some initial atom, then considering it is true, simplify your CNF theory (pretty easy to do), do this recursively. We get a kind of cascading this way.

> **Conflict clause** := a clause that is false in the current (partial) structure, explains the unsatisfiability. This can further be simplified by some algorithm, like `ResolveYoungest(clause)`. Because of the formality of conflict clauses, it can be used for backjumping (overpowered backtracking) by returning to the "decision points" on which the atoms in the conflict clause are defined.

Chapter 9: Model checking of CTL and LTL
----

CTL transition graphs can be huge, so we definitely need linear model checking for this (this exists, but is not mentioned in the course).

LTL model checking is done by reducing it to CTL model checking.

LTL model-checking algorithm:

1. Label every label with the formula's that are satisfied, i.e., with formulas present in the theory or a negation of it.

Exam
----

Forbidden to take former exams with you.

Only 1 "main" question on the theoretical part! So a shorter exam.
Yet also multiple smaller questions, which is nice.

Type of questions will be the same as last year.

This year exam will be shorter.
