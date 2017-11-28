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

Prevent early deadlock in refinements: "disjunction of the guards of new and refined events" is an invariant (G1 or G2 or G3 or ...), i.e., an event should occur every time (since the guard of one of the events has to be specified)
Prevent divergence in refinements: just one of the usual proofs of termination -> assign a natural number to a state and proof that it decrements each time in a loop, by the time it reaches 0 the loop HAS to stop.

? Preventing early deadlock and divergence makes sure correctness property 2 is satisfied?

? In Pidgin language->while implementation: why do we need to do these in two different refinement levels? Is it to make sure not(Q) does not occur all the time? If so, why does this work?
  There's a little note about this right before the conclusion, but it doesn't seem to answer these questions.

ProB still not powerful enough to be used in practice (to model computer programs for example)

Chapter VII: Classical results on Predicate Logic Provability, Expressivity, Decidability of deduction, Incompleteness
-----------------

Axiom schema: A template for a class of formulas, which we can get by substituting some variables, e.g. $\phi \vee \neg \phi$.