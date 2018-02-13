Advanced Programming Languages in AI
====================================

Materials in slide are sufficient.

Exam slots are not fixed, they will be all be made available later in the semester (so don't look at the exam roster on KULoket).

Linear programming: values can be reals, integer programming: values can only be integers (more difficult than linear programming).

If multiple but similar solutions are found, use symmetry breaking to only show one (for example: solutions=[{jan, piet, ann}, {ann, piet, jan}, ...])

To get access to the website of Simonis: use web.archive.org to get an archived version.

The professor really likes the "Constraint Logic Programming using ECLiPSe" book.

Occur check can be explicitly turned on in most Prolog-systems.

Passive constraints are constraints that will not limit the possible assignments of the variable, instead, you wait until all variables are assigned and only then
the constraint is executed, an example: `3*X < Y + 2`.

Active constraint *can* restrict the values of the variables, an example of an active constraint is `=/2` (unification).

> ? Active constraints are more useful because they actively reduce the search space during propagation.