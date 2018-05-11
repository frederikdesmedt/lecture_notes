Knowledge representation
========================

Overall information
-------------------

Exam: 50% theory closed book, 50% exercises open book

Lecture 1
---------

A proposition is always a property of some **domain**, this domain can be any kind of fictional world.

Relation between natural language sentence and a proposition: a proposition is the meaning of the sentence.
Some natural language sentences might not have a corresponding proposition (those sentences without meaning).
The bridge between natural language and information is what makes knowledge representation difficult.

> Propositional attitudes := An attribute about a proposition, the relation between a human and an **information object**, e.g.,
> a belief, a goal, a hope, a promise, ...

The relation between knowledge and belief: a knowledge can be seen as a **justified true belief**, i.e., a belief that can be proven/justified to be true.

> Crisp information := an information that is either totally true of totally false.
> Vague knowledge := knowledge that contains imprecise terms: most, rather, a bit, ...

Declarative == descriptive in the context of this course, this is not the case in other domains, such as declarative programming languages.

> Procedure := a **description** of a dynamic process, not the process itself. For example: the laundry procedure is an information stored in our brain,
> actually doing the laundry is the dynamic process it describes.

Because procedures are considered to be a kind of information, one can distinguish at least 2 types of information:

- Descriptive information: information that describes some property, e.g., information that can be described in first-order logic;
- Procedural information / a procedure.

Learning procedures can be seen as a kind of compilation from declarative knowledge to procedural knowledge (a procedure).

> **Knowledge Representation hypothesis** := AI-systems require knowledge, this knowledge is provided explicitly by *symbolic representations*, the AI-system works
> by using these representations. These kind of AI-systems are called **knowledge-based systems**.
It is already known that the **knowledge representation hypothesis** is false, as there exist AI-systems that do not use a symbolic representation,
e.g., neural networks.

If the **knowledge representation hypothesis** is true, then what kind of knowledge should be represented: declarative or procedural?
There used to be a big controversy about this, with the declarative and procedural parties:

- Argument of procedural party: There exist systems that do not use any declarative knowledge (such as neural nets), so surely procedural knowledge is enough.
- Argument of declarative party: A procedure is a task-oriented compilation of implicit declarative knowledge, i.e., to actually create the procedure one needs declarative knowledge (such as the knowledge known by the developer), so one always needs declarative knowledge. Even more: using a purely procedural software development approach is intrinsically error prone, while a single knowledge base of declarative knowledge can be used to solve a lot of related problems, which each would require a separate procedural approach.

In reality, both the procedural and declarative approach have their advantages and disadvantages (see slides).

Lecture 2
---------

Artificial neural networks cannot **express** the knowledge it knows, yet it can show behavioral characteristics that "prove" that it knows it.

> Formal language := A *precise* language that can be used to precisely express specific kinds of knowledge.

Desirable traits of a formal language:

- Meaningful
- Unambiguous
- Expressive
- Compactness
- Modularity

In choosing a formal language, you need to make a trade-off between expressivity and tractability.
The hard version of this trade-off: if language more expressible $\Rightarrow$ language can be used to express (and solve) harder problems (this doesn't say that
it is less efficient). However it can be more inefficient, e.g., when expressing an NP Complete-problem in the formal language. The weak version is not necessarily true!

Expert systems are the predecessors to *business rule systems*.

The domain of discourse can be seen as a class of possible instantiations of a world.

> Possible world analysis := Look at the worlds in which a proposition is true or false and use these to proof a difference between two propositions,
> e.g., a world in which the first proposition is true and the second is false (or reversed).

Possible world analysis has a shortcoming: it only works for some forms of reasoning, e.g., explanations of inconsistencies can be different but are not covered by it. Why is this different than possible world analysis? Imagine two different inconsistent propositions, both have the same possible (and impossible) worlds, but why they are inconsistent is something else.

Difference between a sentence and a formula: a sentence is a formula *without free variables*. Only sentences can express some proposition of the domain.

Formal and informal semantics are complementary, in general formal semantics is a mathematical representation of a formula while informal semantics are not, e.g., the declarative reading.

A structure is a mathematical representation of a possible state of affairs, i.e., of a model.

Translation from FO -> NL: the declarative reading.

> Model theory := mathematical version of possible world analysis.

> Formal semantics of FO-formula := (class of models of the formula?) or as defined by induction on the formula.

FO is considered very expressive since almost every mathematical theory can be expressed in set theory and set theory can be expressed in FO.

Lecture 3
---------

> Deductive logic := formally a triple: (syntax, proof theory, model theory).
>
> Proof theory := A theory consisting of logical axioms and inference rules. The logical axioms are typically axiom schema's.

> Derivable := A formula $\phi$ is derivable from a theory $T$ iff $T \vdash \phi$.
>
> Consistent := A theory $T$ is consistent if there does not exist a formula $\phi$ such that $T \vdash \phi \wedge T \vdash \lnot \phi$

If a proof system is both sound and complete, then $T \vdash \phi \Rightarrow T \models \phi$ because of soundness and $T \models \phi \Rightarrow T \vdash \phi$
because of completeness, and thus $\vdash$ and $\models$ are the same relationship and thus derivability and logical entailment coincide.
When a theory $T$ is satisfiable, then for every model of $T$ and every formula $\phi$, $\lnot (T \vdash \phi) \vee \lnot (T \vdash \lnot \phi)$, since every formula is
either true of false in a model and derivability and logical entailment coincide. Therefore, if $T$ is satisfiable, $T$ must be consistent. If $T$ is not consistent, then
for some formula $\phi$, $T \vdash \phi \wedge T \vdash \lnot \phi$, from soundness then follows $T \models \phi \wedge T \models \lnot \phi$ for every model of $T$.
There does not exist a model $M$ for which $M \models \phi \wedge M \models \lnot \phi$, thus $T$ cannot have any models and is unsatisfiable. From this it follows that
satisfiability and consistency coincide.

Proof of equivalency of sound and complete proof systems: For every two sound and complete proof systems $P_1$ and $P_2$, it follows that a proof for some formula $\phi$
from some theory $T$ exists iff $T \models \phi$. This means that $T \vdash \phi$ according to proof systems $P_1$ iff $T \models \phi$ iff $T \vdash \phi$ according to
proof system $P_2$, which is the definition of proof system equivalence. Therefore any two sound and complete proof systems must be equivalent.

> Difference between a structure and a model := A model is a set of objects, a structure has a model and a mapping from symbols to objects in its model.

> "Overloading does not entail ambiguity"

> Intensional statement := statement in which you switch between the set of worlds you talk about.

In the root of an FO-sentence one is talking about all possible worlds, but in a conditional one is talking about one specific (current) world.

If a proof system is sound and complete, it is equivalent to all other sound and complete proof systems, i.e., a sound and complete provability-relation is unique.

Recent famous proof system: Coq.

Expressing knowledge in KR is a two-step process: (1) ontology design -> (2) explicitating knowledge.

> Ontology := A set of informal concepts, attributes, roles, ... relevant to the domain of discourse and which we would like to talk about.
>
> Vocabulary := A set of formal symbols to denote elements of the ontology.

An ontology gives rise to a set of models representing possible worlds of the domain of discourse, yet this set will/can also consist of worlds that shouldn't be
possible in the domain of discourse. Specifying which worlds are and are not possible is done in phase 2: explicitating knowledge.

Important qualities of an ontology:

- Precision: the ontology - i.e. the concepts introduced - must be suitably specific (you need the right granularity).
- Abstraction: should work at the right level of abstraction. You want to be precise (see previous point), but not too precise. This implies a tradeoff between abstraction and precision but there is a sweet-spot. Some systems walk through this tradeoff, e.g., Event-B.
- Compactness of representation: You want the relevant propositions to be expressible in a compact representation.
- Robustness: Allowing the ontology (and theories in it) to be adaptible and extensible. There are techniques developed for this, e.g., reification.

Reification allows for improving/changing robustness, compactness and precision/abstraction of an ontology. This is why reification is such a valuable idea.
Why can it improve robustness? Look at the `Purchases`-example in the slides. Because of the reification of a purchase, one can now add additional properties of a
purchase without having to change the theories that already depend on the other properties.

WATCH OUT: Adding arguments to a predicate or function that is the result of a reification is a bad idea!

> ? Why is adding arguments to a predicate or function that is the result of a reification a bad idea? Perhaps because reifying a reification is a bad idea? If that is the
> case, adding arguments to a reified predicate or function is then equally bad (or even worse) than adding an argument to some pre-existing predicate or function without
> reifying.

Many-sorted logic enforces the sets of the types to be disjunct, i.e., subtyping is impossible. Want subtyping? Use order-sorted logics.

> Category clash := Kind of type error in FOL, e.g., "the colour of that building is very heavy".

Lecture 4: KR using classical logic
-----------------------------------

Sum, min, max, and count aggregates will be used in exercise sessions.

Watch out with using existential quantifiers in an aggregate, e.g., slide 67.

Cardinality constraints can be reduced to a corresponding formula in **FO(Agg)**.

> Terminological knowledge := Particular class of very simple sorts of knowledges. Similarly: terminological logics.

Different kinds of terminological facts:

- Subtypes
- Disjointness
- A supersort as the union of subsorts
- Symmetric roles
- Inheritance

> ? Does the list above specify all kinds of terminological facts? I believe so.

Type reification is not recommended because the prof doesn't like it.

In slide 74, `A(x) = y \Rightarrow ...` is too strong because `A(x)` is a total function, so the antecedent is always met and this entails that every
object in the domain must be of type `E`.

If one tries to implement partial functions in FO, one usually uses a dummy value as the result of function application of objects we don't want a value for.
These dummy values are *spurious elements*.

**UNA** and **DCA** are important because they express very common implicatures.

> Sufficient condition := Some condition is sufficient to be true.
>
> Necessary condition := The condition must be satisfied to be true.

The qualification problem is a problem that seems to arise when trying to introduce exceptions to some rules, say, in the antecedent of an implication
in a universal quantification. This problem arises because of the major difference between "common sense reasoning" and reasoning imposed by FOL.

Lecture 5: Extending FO with definitions
----------------------------------------

> Assertional knowledge := Knowledge that some object belongs to some class or relation.
>
> Definitional knowledge := Knowledge that is part of some definition.

Note how assertional knowledge != definitional knowledge.

Most definitions in practice are not inductive, but some are, so it's important to understand them.

Monotone inductive definitions allow an arbitrary selection of rules during each step, the result of non-monotone definitions can change as the choice
of a rule at each timestep changes.

When the order of an inductive definition is fixed, a non-monotone kind of inductive definition will always produce the same results. Orderings that are
consistent with the specified ordering will also produce the same results.

The exact semantics of an ordering? Look at the slides about **well-founded ordering**.

A definitional implication has the following property: $(\phi \rightarrow A) \Rightarrow (\phi \Rightarrow A)$.

$Q(x) \leftarrow false$ doesn't add anything to $Q$!.

> Justification tree := A construction that can be used to (visually) express the semantics of an inductive definition?

${ P \leftarrow Q, Q \leftarrow P}$ results in the empty structure.

Gotcha: the value of a negative (justification) cycle is **true**, the value of a positive cycle is **false**.

Lecture went up to formal semantics of FO(ID) (without the discussion).

Lecture 6: Extending FO with definitions
----------------------------------------

Rules in the same stratum/layer only monotonically depend on each other.

The difference between definitional and material implication can be seen in the relation between the conditional/antecedent and the conclusion/consequent.

> Clark completion := A transformation of a definition to an FO-theory/formula. A Clark completion of definition $\Delta$ is denoted by $Compl(\Delta)$.

$\Delta$ and $Compl(\Delta)$ are equivalent if $\Delta$ is non-inductive.

Relation between logic programming and Prolog: a logic program is Prolog without side-effects.

Inductive definitions are more than what Prolog does.

Deduction in *FO(ID)* is undecidable and not semi-decidable, FO is undecidable and semi-decidable. This doesn't mean *FO(ID)* is useless, it's only useless
for doing deduction.

CWA can very easily lead to inconsistencies, e.g., when you have a disjunction of two positive literals. In general, it only really works without
inconsistencies in *Horn logic*. Yet CWA can be useful in some knowledge representation settings: Default assumptions and communication agreements.

A place where default assumption is unacceptable: formal verification (of software).

> Communication agreement := CWA **only in a scenario where we have total knowledge of the domain**, then we only construct a theory $T$ for all true formulas,
> we then don't need to right all false formulas.

A common misusage of CWA happens when we only want to apply CWA locally (for some specific axioms).

> ? What's a default assumption? How is it different from a communication agreement?
>
> Answer: They are more informal concepts, default assumption means we expect a default value for *objects we know nothing about*, while a communication
> agreement is used when we do know all information about the world and want to express everything without actually specifying every false atom explicitly
> (so we can reduce the size of the theory).

> Inheritance hierarchies := A tree-like structure of types (parent is supertype, child is subtype).
>
> Default inheritance hierarchies := An inheritance hierarchy together where properties for subtypes are by default the properties of the supertype,
> yet they can be overridden.

Inductive definitions are quite *elaboration tolerant*, we can easily extend the predicate expressed by the definition by providing additional rules.

Lecture 7: Definitions summary
------------------------------

**Only some pages are required for the exam, look on Toledo!**

Lecture 7: Using logic for problem solving
------------------------------------------

> **Knowledge base system** := A system that contains a **knowledge base** (a bag of information) and that supports various inference tasks to be performed
> on this knowledge base.

> Model revision := Change a model so that it satisfies a new set of constraints, model expansion is a specific kind of model revision that only adds elements
> to the sets of a structure.

> Propagation inference / computing ramifications := Propagate consequence of changes to a model by using the theory.

Visualisations in IDP are done by the theory itself! It passes all relevant information to the structure, which is then read and rendered by some algorithm.

Lecture 8: Model logics and agents
----------------------------------

> Deontic information := Information that *should* be true, but *is not necessarily true*.

> Epistemic logics := model logics that reason about the knowledge of agents.

> Modal logic := A logic that supports multiple **propositional attitudes**.

The knowledge of a world is what worlds are possible and impossible, this intuition explains the use of the reachability relation in **Kripke structures**.

In modal logic, if something is necessarily true, it doesn't imply it is possible that it is true.

> Logic engineering := Adding axioms in a modal logic to express the subtle differences between interpretations of the **propositional attitudes** you are using.

Lecture 9: Model logics and agents
----------------------------------

> Correspondence theory := Theory concerned with the relation between axioms and the properties the reachability relation has.

Slide 74 implies that general knowledge is *weaker* than common knowledge.

> General knowledge := Everyone knows it.
>
> Common knowledge := Everyone knows it, everyone knows that everyone knows it, everyone knows that everyone knows that everyone knows it, ...

It's impossible to get common knowledge when sending the corresponding information over an unreliable medium.

Lecture 10: Probabilistic logics
--------------------------------

> Probabilistic process (CP-logic) := A tree expressing the possible causations of the theory. A probabilistic process is to a CP-logic theory as a structure is
> to an FO-logic theory.
>
> Execution model (CP-logic) := A probabilistic process that is possible for the related CP-logic theory, an execution model is to a CP-logic theory as a model is
> to an FO-logic theory.

Lecture 11: Probabilistic logics
--------------------------------

> ? How is the instance-based semantics identical to the causal semantics if the ordering matters? For example, in the following CP-logic program:
> `{ 0.3::a, 0.5::b <- a, 0.6::c <- b`.
>
> Answer: Make sure that head selections are in a stratified ordering.

Advantages of CP-logic over Bayesian networks:

- It's easier to recognize conditional independence.
- CP-logic can, in general, more compactly represent probabilistic systems.
- One can change some parts of a CP-logic without having to change everything else, this is much more difficult in BNs.
