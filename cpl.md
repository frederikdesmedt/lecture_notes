Comparative Programming Languages
=============

Type inference
-----

The type inference algorithms we will see are also the basis for more advanced languages like Haskell and Scala.

We reduce type inference to model generation of a theory, in which the theory represents the restrictions on the types of variables/functions/...
Yet, we use a more specialized (and efficient) approach to finding a model, i.e.: substitutions and unification.

The following list of equations:
> $t_1 = int$  
> $t_2 = t_3 \rightarrow int$
Has multiple models, since we can choose any $t_3$.

If we cannot use an equation in a substitution (because it contains a variable that's already in the substitution), just replace that variable with the substitution. If we do this for all those kind of variables in the formula we can use it as a substition. We do not need to do this recursively because the substitutions already are substitutions.

The substitution algorithm we see in the lecture is proven to terminate, the proof however is not given in the lecture nor the book.

In the substitution step we also substitute the new substitution in all existing substitutions (otherwise one of the previously calculated substitutions might no longer be a valid substitution given the other substitutions).

**Exam:** Very often a question about the type inference algorithm. *An intuition is not enough, he will give an example where intuition will not be enough.*

**Challenge:** Can you write an *expression* that has a polymorphic type, i.e., the type can be anything?  
**Solution:** The only way we can construct such an expression is by creating one that will never end (non-terminating): `letrec ? f(x:?) = (f x) in (f 0)`. In here the constraint on `f` is `int -> ?`.

Module system
-----

Enforce abstraction boundaries between different modules: make sure that another module can only access the procedures/... we want them to have access to (encapsulation).

Examples of module systems:

- namespaces/name scopes (in C#)
- packages (in Java)
- classes + interfaces
- functions
- JARs in Java, and other kinds of compilations in other systems.

The JAR example is the scope we should look to.

ML-based languages have a module system inside the language (contrary to, say, JARs, which are not part of the (Java) language).
This is also the kind we will be focussing on.

Note how interfaces (like in Java) is not a module system by itself, since it does not abide to the four criteria listed in the introduction of the presentation.

In the context of Java, the **implementation** of the module would be the JAR, the **interface** of the module would be the documentation (JavaDoc). Again, note how these are not part of the language, we do want this though, because then we can perform type checking during compilation (not possible in Java!).

We would love to have a module system that has a module interface that provides type information + additional functional information (what is informally provided in JavaDoc).

The lecture will contain ridiculous examples, because they are all toy examples, so it might be hard to understand why we would want the module system, yet it makes sense in larger systems/programs.

With a module system we can compile (and type check) only based on the module interface of the imported/required modules, we should not require the module implementation! This allows us to change the module implementation on the fly (considering they maintain the same functionality).

> `from m1 take a` := take value `a` from module `m1`

Dependencies of modules we use can be modelled in a dependency graph, in our language this dependency graph must be a DAG.

*In our language, a module is seen as a "named environment".*

> Qualified variable := a variable that we retrieve from another module?

Tricky part of module system implementation: when evaluating the expressions in a module, which environment do we do it in?
**(Partial) answer:** We evaluate the first module in the empty environment, the second module in the resulting environment after having evaluated module 1, ...

One level of abstraction in our language: module split up in an interface and an implementation.

Module interface can provide additional information, such as a kind of formal JavaDoc (no one really does this, because it is so complex) or in our language: just provide type information.

> Type of a module := the interface of the module. This allows us to integrate the module system in the type system we introduced in the previous chapter.

What we do not yet have in our module system: the interface only says something about the type, i.e., a procedure `sort` doesn't actually have to sort.

> module m1
>   interface
>     [ u : bool ]
>   body
>     [ u = 33 ]
> from m1 take u

The code above *runs*, but doesn't *type check*, remember how we talked about this in the chapter about type systems (where a type checker rejects working applications).

**Opaque types** provide us with a lot more abstraction in the module system. **Transparent types** aren't really very powerful, they're just aliases/abbreviations for (expressible) types.

If we would like to support **opaque types** we need to consider the opaque type as a primitive type (like int, bool, ...) since we cannot know the exact type, i.e., we have a primitive type `from m take t`.

Opaque types are only an abstraction at compile-time, at runtime it is *always* "replaced" by a concrete type.

We're not looking at the third extension of module systems, it is covered in the book however.

Chapter 9: Objects
----

Dynamic dispatch is what differentiates a method from a function.

> **Dynamic dispatch** := Run the implementation of the most concrete type/class the object is an instance of, not the implementation of a superclass.

If we want dynamic dispatch in plain old functions, we would have to check the type of the argument and go to a specific implementation based on the type. This is not very nice, but the main disadvantage: it's not extensible! So it seems we cannot implement it extensibly in regular functions, making methods and functions fundamentally different (methods being more powerful).

With methods we automatically have recursion for free, thanks to dynamic dispatch (in a function `f` we can always do `self.f(...)` or something similar).

**Question:** "99% of the OO-languages still use inheritance", which means 1% doesn't, i.e., inheritance is not a requirement for the language to be object-oriented. Then what are the minimum requirements in order to be considered object-oriented?

> **Field shadowing** := When a subclass has a field with the same name as a field of one of it's superclasses, the field of the subclass is the only one in scope for the subclass.

Note how in Java there is no **"method shadowing"**, rather there is overriding, where both the superclass and the subclass go to the method implementation of the subclass.

