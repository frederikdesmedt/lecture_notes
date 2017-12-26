# Vereistenanalyse van complexe systemen

## Business context

> Ideeën van de klant omzetten in plannen voor de developers

We willen dat de software een added value is voor de klant. Dus stellen we altijd de vraag, waarom maken we dit, wat brengt dit bij tot de business van de klant.

**Use cases:**

* Normal
* Alternative: Andere weg maar toch naar hetzelfde einddoel als normal
* Exception: Kan het einddoel niet bereiken. Maar we proberen toch altijd nog een weg naar het einddoel te vinden door bijvoorbeeld opnieuw een invoer van de gebruiker te vragen.

**Use case diagram:**

* Extend: uitbreiding van een bepaalde usecase die mogelijk is onder bepaalde condities. Bv: met creditcard betalen voor een item i.p.v. cash.
* Include: Is deel van de usecase maar het heeft voordelen om dit deel eruit te halen, voor herbruikbaarheid,....


## EVALUATIE OPDRACHT BUSINESS CONTEXT

* Een use case is een actie die waarde toevoegd aan de store.
* Zet alles in de diagrammen (usecase diagram), splits eventueel op in kleinere deel diagrammen
* Shopkeeper etc zijn ook actors op business niveau
* Interested stakeholders zijn alle actoren die binnen de business ergens gebruik kunnen maken van deze actie
        --> register user: accountant is ook een interested stakeholder (waar moet ik een invoice naar toe sturen?)
* Het betalen is een aparte usecase (include) veel exceptions,... die kunnen gegooid worden. Zo halen we een groot deel van de complexiteit uit de andere use cases.
* Het kiezen van een item is een aparte use case. Belangrijk deel voor de business!!
* Woordkeuze is belangrijk! Gebruikt de klant altijd item gebruik item, gebruikt hij product noem het dan product.

## System context

### Functional requirements

> Het uitwerken van de kleine onderdelen van de business use cases
> Een interactie tussen systeem en actoren.

### Non-functional requirements

--> regels die opgelegd worden aan de developers, volgen uit de business rules & vragen van de klant
--> duur om allemaal te implementeren (100% up time,...)
--> verschilende types van non-functionals
Hou bij wat de vraag is maar ook zeker waarom dat de vraag er is, op deze manier kunnen we alternatieven aanbieden
Zorg voor meetbaarheid, anders komt het aan op gevoel en is moeilijk om aan te tonen dat het zo is
Use case: --> schrijven voor de klant! (en daarna ook de developers -> Maak het dus duidelijk!!)

* Naam
* Short description
* Trigger
* Primary actors
* Secondary actors
* Precondities
* Normal flow (één tijdseenheid): gebruik duidelijke, actieve werkwoorden.
* Alternative flow: een alternatieve manier om tot het einde te komen
* Exception: Een error waardoor we niet meer verder kunnen in het verhaal, maar we proberen wel altijd een alternatief te vinden om terug in de normalflow te komen
* Post condition = startpunt van een andere use case!
* Additional information --> extra info bij bepaalde cases die nuttig zijn voor developers/... maar niet nuttig zijn om duidelijk te maken in deflow
* Remarks (eventuele TODO's)
* Version, Author, date --> om te zien welke veranderingen dat er allemaal gebeurd zijn!(niet echt nodig voor deze opdrachten)

Hoe schrijven:
> Werken in iteraties, zo kunnen we sneller beginnen met developpen en hebben we sneller feedback (= goedkoper om bij te sturen)

-> schrijf in actie - reactie
-> makkelijke taal, makkelijker voor klant en voor developers
-> Beschrijf wat er moet gebeuren, niet hoe!

-> begin met een blackbox omschrijving te maken, denk nog niet teveel aan het systeem en hoe het juist moet werken.
-> Naar het einde willen we toch een whitebox omschrijving
-> Schrijf in 3-rd person

----------------

elementary business process: een actie op een bepaalde plaats in een bepaalde tijd

De use cases veel meer uitsplitsen. --> iedere stap is op een moment en waarschijnlijk door 1 actor

---------------

vandaag: Elementen uit de backlog. --> user stories

User stories:
Elke story moet een waarde bijbrengen bij het programma. En zijn zeer kleine stukjes van het programma.

We splitsen alles op in user stories (nog kleinere delen) omdat we op deze manier veel makkelijker kunnen prioriteren.
Omdat we kleinere delen hebben kunnen we beter inschatten hoe lang dat we gaan nodig hebben voor de ontwikkeling

Spike: een deel van een functie waar we meer tijd voor gaan nodig hebben. bv: het opzetten van een server,...
Bug: Een niet afgewerkte user story

User Story:

* titel: Als [actor] kan ik [Actie]
* beschrijving (rationale): Zodat ik [Reden] --> waarom heb ik deze story?
* details voor de implementatie: Meestal hebben we hier enkel een korte normal flow, omdat we zo kleine delen aan het beschrijven zijn --> zelf te kiezen op welke manier we dit schrijven
* tests: zorgen dat de validatie critera duidelijk zijn op gelijst --> manier om te bepalen of een story done is of niet

Schrijf user stories volgens het INVEST principe:

* Independent --> user stories hebben niets met elkaar te maken
* Negotiable --> moeten besproken kunnen worden met de klant
* Valuable --> Heeft de story nut voor de klant?
* Estimable --> moet kunnen ingeschat worden door de developers
* Size --> We werken met kleine delen
* Testable --> Alle stories moeten getest kunnen worden.

ATTENTION:

* Scope creep: het blijven uitbreiden van de scope met nieuwe features, door klant of de developers
* Functional vs. Technical split: Alle stories zijn end-to-end, er moet een duidelijke eind value zijn van de story
* Amount of detail: Hoe minder contact met de klant --> hoe duidelijker alles uitgeschreven moet worden.
* Readability: Alles moet duidelijk leesbaar en begrijpbaar zijn.

------------

### Domain modeling

Een domeinmodel weergeeft alle business entiteiten en de relatie tussen deze entiteiten.
  --> uitgescheven in UML

-------------------

21/12

### Object constraint language

> taal over UML om extra constraints toe te voegen op UML diagrammen

gebaseerd op de wiskunde (verzamelingenleer etc.) maar minder formeel

qualified associatie: een associatie op een eigenschap van een klasse (bv. accountnumber voor een bank --> extra papier)

2 manieren om OCL te schrijven:

- "plakbriefjes"/notes
- context diagram met de invarianten/precondities/...

>**invalid** == nullpointer
>**null && invalid** komen enkel voor bij individuele objecten, _niet op collecties of bags_!!

Multivalued property kan een lege lijst teruggeven ipv null --> bv friend.employer (geeft de werkgevers van persoon terug, als persoon geen werkgevers heeft krijgen we de lege lijst)

associatie klassen kunnen worden aangesproken door de naam van de klasse te gebruiken, bv. company.job

Oefeningen OCL:

> OCL omzetten naar SQL queries
> SELECT f1 FROM T1 JOIN T2 ON (T1.fi = T2.fj) WHERE .....
>
> SElECT salary FROM job JOIN company ON (job.cid = company.cid) WHERE company.name = "MyComp"
>
> SELECT date FROM marriage JOIN job ON (marriage.hid = job.pid) or (marriage.wid = job.pid) JOIN company ON (job.cid = company.cid)

### Sets and bags

#### Collection types

voor operaties op collecties gebruiken we "->" bv. myCompany.employee->size()

Mogelijke operaties op OclAny: 

- =, <>
- oclIsNew
- oclIsUndefined
- oclIsInvalid
- oclType
- oclIsTypeOf
- oclIsKindOf --> gaat ook kijken of het element een subtype is of niet

4 soorten collections:

- sets --> zonder duplicaten
- bag
- orderedSets --> geordende set
- sequence

> **nonunique** --> een object kan meerdere keren geassocieerd worden met eenzelfde object (defaut is unique)

> Is there at least one married couple working in my company,
> not myCompany.employee.spouse->excludesAll(myCompany.employee)
> not myCompany.employee.spouse->intersect(myCompany.employee)->isEmpty()

##### iterators

- select --> alles waarvoor de expressie TRUE is
- reject --> alles waarvoor de expressie FALSE is
- any
- forAll
- exists
- one
- collect
- closure

> myCompany.job -> any(title="secretary") and (person.gender=Gender.MALE)

> myCompany.employee.spouse->forAll( partner: Person | partner = partner.spouse.spouse)

> r(A) = {B} U r(B)  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = {B} U {C} U r(C)  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = {B,C} U {A} U r(A)  
> &nbsp;&nbsp;==> oplossing: {A,B,C} --> is altijd de kleinste oplossing

### Pre- en postcondities

Use cases moeten ook formeel worden gedefinieerd --> extra stukje "uses" waar we namen definieren

### Oefening: File system --> modeloplossing komt op toledo

**Invariant: niet meer files op schijf dan capaciteit**
> Context HardDisk  
> inv CapacityNotexceeded:  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sum(self.directory.diskItem.size) <= self.capaciy (met size is de properSize-> dus ook de som van de size van de elementen in de directory)

**Invaiant: properSize**
>Context Directory  
> &nbsp;&nbsp;&nbsp;&nbsp;inv properSize  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.size = sum(self.diskItem.size) + _delta_ met _delta_ plaats voor deze waarde op te slaan

**Invariant: Dir mag zichzelf niet rechtstreeks of onrechtstreeks bevatten**
>Context Directory  
>&nbsp;&nbsp;&nbsp;&nbsp;inv NoSelfContainment  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;not self.diskItem->closure(diskItem)->includes(self)

