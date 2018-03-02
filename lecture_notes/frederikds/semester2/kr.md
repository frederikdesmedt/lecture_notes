Knowledge representation
========================

Overall information
-------------------

Exam: 50% theory closed book, 50% exercises open book

Lesson 1
--------

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

Lesson 2
--------

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

Lesson 3
--------

> "Overloading does not entail ambiguity"

> Intentional statement := statement in which you switch between the set of worlds you talk about.

In the root of an FO-sentence one is talking about all possible worlds, but in a conditional you are not.

If a proof system is sound and complete, it is equivalent to all other sound and complete proof systems, i.e., a sound and complete provability-relation is unique.

Recent famous proof system: Coq.

Expressing knowledge in KR is a two-step process: (1) ontology design -> (2) explicitating knowledge.

> Ontology := A set of informal concepts, attributes, roles, ... relevant to the domain of discourse and which we would like to talk about.
>
> Vocabulary := A set of formal symbols to denote elements of the ontology.

An ontology is more than defining a vocabulary, e.g., UML diagrams represent an ontology for object-oriented design.

Important qualities of an ontology:

- Precision: the ontology - i.e. the concepts introduced - must be suitably specific (you need the right granularity).
- Abstraction: should work at the right level of abstraction. You want to be precise (see previous point) but not too precise. This implies a tradeoff between abstraction and precision but there is a sweet-spot. Some systems walk through this tradeoff, e.g., Event-B.
- Compactness of representation: You want the relevant propositions to be expressible in a compact representation.
- Robustness: Allowing the ontology (and theories in it) to be adaptible and extensible. There are techniques developed for this, e.g., reification.

Reification allows for improving/changing robustness, compactness and precision/abstraction of an ontology. This is why reification is such a valuable idea.

WATCH OUT: Adding arguments to a predicate or function that is the result of a reification is a bad idea!

> Category clash := Kind of type error in FOL, e.g., "the colour of that building is very heavy".
