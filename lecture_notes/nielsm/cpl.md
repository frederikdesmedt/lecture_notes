# Vergelijkende studie van imperatieve programmeertalen

Expressions:

* literals: 1,2,3,...,#t,'xy
* identifier: null, +, -, number?, list
* (E1 E2 ...) (+ 1 2)
* (keyword ...) if, define, cond, and, ...

S-Exp := Symbol
      | S-List
S-List := (S-Exp*)

Quasi quoting --> extended version of quoting (backquote)

## Chapter 3

static <-> dynamic scoping
    --> Welke waarde gebruiken we voor bepaalde vars?

translator geeft niet altijd dezelfde waarde

## Chapter 4

EXPLICIT_REFS language -> mutable variables/mutable state

In deze taal kunnen zelfde expressies in eenzelfde enviroment een andere waarde hebben --> zie example p9

Het is essentieel dat we in een bepaalde volgorde alle expressies uitvoeren.

>Herschrijf taal om store door te geven als param ipv globale store te gebruiken.

## Chapter 5: Continuations

control-flow context --> writing program with a hole '[]'

> vof (2-(2-1))
> -> vof 2
> -> vof (2-1)
> --> vof 2
> --> vof 1

>vof/k (2 - (2 - 1)), []
>vof/k 2, [] - (2 - 1)
>ac    2, [] - (2 - 1)
>vof/k (2-1), 2 - []
>vof/k 2, 2-([] - 1)
>ac    2, 2-([] - 1)
>vof/k 1, 2-(2 - [])
>ac    1, 2 - (2 - [])
>ac    1, 2 - []
>1

### ECMAscript: case study

ES is function scoped
--> vars worden tot bovenaan in de functie gehezen

block scoping is mogelijk via "let"

OBJECTS:
is objectgericht(prototype-based) via properties

de parameters van objecten kunnen van ieder type zijn.

Makkelijk om objecten aan te maken via prototypes of een constructor

This is een speciale var -> check ppt

Toevoegen van functies gaat door ze toe te voegen aan de prototype van het type

EXTENDED OBJECTS:

ES6: new features:

* Classes
* Proxies --> custom behavior voor fundamentele operaties

##################################

Les 27/10

Recap last class:
Apply Continuation(AC) gebruiken we, wanneer we net een nieuwe waar de hebben gevonden, de expressie te bekijken wat de volgende stap is om te berekenen.
Het programma stopt als we bij een end-cont komen.

#### Trampolining and registerizing

trampolining: zorgen dat we geen stack overflow krijgen bij recursieve oproepen.

Exceptions (new language):

* Lists, with null?, car(head) and cdr(tail)
* try-catch and raise

Threads (new language --> bij implicit-refs):

* spawn(p) --> start new thread
* yield() --> geef ruimte aan een andere thread om te runnen
* mutexes: wait() & signal()

 --> programma's stoppen pas als alle threads gedaan zijn. Niet alleen als de main thread klaar is.

 De output van het programma is de output van de main thread, ook al zijn er nog threads die later aan het uitvoeren zijn dan de main thread

 The interpreter:
    in het boek cheaten ze een beetje door een thread voor te stellen als een functie (thunked values '()').

    States of the scheduler:
      - the-ready-queue
      - the-final-answer  --> value of the main thread, if done
      - the-max-time-slice  --> number of steps a thread may run
      - the-time-remaining  --> number of steps remaining for the current thread

    Einde van programma: als "the-ready-queue" leeg is. Dan geven we "the-final-answer" terug.

Synchronization:
  mutex is een mutable pair, waar we een boolean hebben en een expression.
  De boolean geeft weer of de thread open of closed is.

  Het is mogelijk om te eindigen zonder dat alle threads zijn afgelopen: door mutex halen we threads uit de ready-queue

#########################################################################################
09/11

## Type-systems

Type error: We maken een klasse om errors in op te kunnen vangen. Zoals bijvoorbeeld een operatie toepassen op een verkeerd type kunnen we voor de runtime al opvangen. bv: 3-"s"

Hiervoor hebben we ook een static checker nodig: Deze checkt of een programma correct is of niet. --> rejects programs that can lead to type errors

Een checker is SOUND als er geen programma is dat toch een type error kan gooien.
Een checker moet wel USEFULL zijn, er moeten genoeg programma's bestaan die de checker als goed ziet. --> vaak herkent de checker wel niet alle goede programma's

voorbeelden die de checker moet opvangen:

* index out of bounds error
* field not found error
* method not found error
* subclass of exception is caught
* breaking abstractions
* concurrency errors

Type errors for LETREC:

* evaluating a variable that is not bound
* applying "-" or "zero?" to values that are not numbers
* applying an if-test to a value that is not a boolean
* evaluating a procedure application where the operand is not a proc-val

Voorstel voor types voor letrec:
  Type::= int
  Type::= bool
  Type::= (Type -> Type)

  --> Maar hier kunnen we niet alle programma's mee controleren. bv: returntype is of bool of int kunnen we niet voortellen hier.

###################################################
Les: 24/11

Type inferences --> vaak op examen!!

Solving the equations:
Substitution: Alle tot hier toe gekende variabelen moeten vervangen worden door hun juiste waarde.

Wanneer we een inconsistentie hebben krijgen we een melding dat we het niet kunnen evalueren.

Voorbeelden:
  proc(x:?) x                                             ==> x:%   (% is een unknown type)

  let id = proc(x:?) x in (id 1)                          ==> x:int

  let id = proc(x:?) x in let y = (id 1) in (id zero? 0)  ==> error (int & bool can't be unified)

  proc(f:?) proc(x:?) (f(f x))                            ==> f:(%1 -> %1); x:%1

  letrec ? f(x:?) = (f x) in (f 0)                        ==> uitvoer moet % zijn --> kan enkel maar voorkomen in een programma dat niet eindig is

## Modules

Voorbeelden van modules:

* named scopes, namespaces
* packages
* classes & interfaces
* functions
* JAR-files etc. --> Vooral de focus op deze --> te vinden in de familie van ML-talen

Interface for JAR-file: javadoc --> interface is een weergave van wat de client van de module moet weten en op welke manier hij deze moet gebruiken

Modules mogen enkel andere modules gebruiken die al voorheen gedefinieerd zijn

In welke enviroment moeten alle expressies geevalueerd worden?
  -> We gaan na iedere module de env expanden met deze module

#########
**30/11**

Modules: bestaan uit 2 delen:

* Interface -->  het type van de module
* Implementation

Bij modules gebruiken we niet de environments van de modules om de volgende environment te berekenen maar gebruiken we de type-environment

type-environment = alle types van een module

### Extention simple modules: Modules that declare types

> Now modules can declare types in his interface.

2 sorts of types:

* Transparent types
* Opaque types

**Transparent types:**

**Opaque types:** We define what we can do with this type, but not specifing what kind of type it is.

> interface
> [opaque t
> zero : t
> succ : (t -> t)
> pred : (t -> t)
> is-zero: (t -> bool)]

### Another extention (niet te kennen): module procedures

## Objects

Classes have private **fields** and public **methods**. All methods and fields are untyped.

Constructor wordt hier voorgesteld als intitalize() --> Deze constructor moet alle fields in de klasse instantiëren.

Assigments voor de fields doen we door set _field_ = _value_.

> new: creates object
> send: het aanroepen van een functie
> self: het object zelf

**Encapsulation:**
**Dynamic dispatch:** Het uitvoeren van een methode hangt af van het type van het object.
**Recursion:** Omdat we dynamic dispatch hebben moeten we geen speciale dingen implementeren om recursieve oproepen te kunnen doen. En werkt het gewoon out-of-the-box
**Inheritance (extends):** Op deze manier kunnen we code hergebruiken. --> zoals java,...

> == IMPLICIT-Refs + lists, multiple args, multi-declaration let,...

ExpVAL kan een type ListOf hebben, dit is anders als in java waar lijsten objecten zijn. 

An object is made up from:

- classname
- list of refs to ExpVals

> slide 1é
> als we een instantie maken van c3 moeten we 5 variabelen aanmaken, x&y, y and x&z
> in de lijst staan de variabelen van c1 eerst in de lijst van de variabelen

------------------------------------------------------

## Case study: GO

* statically-typed language
* Type inference 
* Fast compilation

> **go get** --> dependency management


