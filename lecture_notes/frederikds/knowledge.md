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