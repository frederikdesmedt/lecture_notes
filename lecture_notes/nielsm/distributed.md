# Distributed systems

* Concurrency
* No common clock
* partial failure

Marshalling: serialization --> encoding data for messaging
 <->  unmarshalling

ACID transactions:

* Atomicity
* Consistency
* Isolation
* Durability

## Locking

Two-phase locking

* growing
* shrinking

## Java RMI

## Java EE

> Indirect communication (slide) == examenvraag

PubSub != time-coupled
Queues maken we persistent -> beter bestand tegen crashes

24/10 Distributed file systems
Belangrijkste in dist file systems is performance
--> Cache
Problems:

* Concurrency
* Node failure
* No global clock

Opening file -> returns handle = reference
-->Nog altijd even traag als het netwerk
We kunnen een lokale kopie maken van de file om zo de file niet meer te moeten lezen.
Welk patroon is er bij de gebruikers van dit systeem: veel files openen, veel naar dezelfde file lezen en schrijven,�
We maken de middleware op zo een manier dat we ervoor kunnen kiezen welke manier we de files gaan openen of hoe we ze cachen.
Hoe zit het bij files die we sharen, concurrency problemen etc.
Als we meer lezen dan schrijven is het beter dat we een lokale kopie bij houden. Want er is minder kans op concurrency problemen. (NTFS & AFS)

**NetworkFileSystem(diskless workstations):** -> stateless
Block based based cashing
->Veel intensiever op het netwerk, iedere read & write ops moeten over het netwerk
COST = read & write over het netwerk
	 = polling (checken of de file is veranderd)

**AndrewFileSystem(disk on every machine):** -> stateful
Whole file cashing
->Veel minder intensief op het netwerk, enkel de openfile moet over het netwerk

Om alle files up to date houden:

- Timestamp
- Observer --> server pusht dat er een verandering is van de file (statefull server)
- client vraagt om de zoveel tijd of de file aangepast is.

File service architecture:
Requirements:

- addressed by most file systems:
	- falure transparency
	- performance transparency

Flat file service:

- file
- data
- operations (read, write, create,....) -> bij read en write geven we een positie mee waar we gaan lezen of schrijven, dus we kunnen de operatie meermaals uitvoeren zonder problemen!
- UFID

Serverside cache: read ahead en delayed write

### Case Study: Network file System: (NFS)
Mount service on server --> het remote mounten van een disk op een filesystem

- Hard mount
- Soft moun

Redenen veel netwerk verkeer:

- blockbased caching
- Stateless
- config. flex path name translation

### Case Study: AFS

Vice: file server
Venus: user/client software

load on server: NFS vs AFS: 100% NFS <-> 40% AFS
--> Andrew werkt voor een specifiek patroon!!!

Comparison NFS vs AFS

###############################################################################
31/10 Middleware layers
Common services

- Persistence
- Transactions
- Security

## Persistence

Fault-tolerant services

Replication:

- performance
- availability
- fault tolerance

### Fault tolerant services:

* Passive (primary-backup) Replication (--> majority not doing the work [Not fast!!])
	  --> we hebben 1 primary node waar we de FE op inpluggen om aan de data te raken. Deze primary node gaat er ook voor zorgen dat de andere nodes up-to-date zijn
	  Communicatie tussen de primary en secondary nodes: view synchronous group communication
	  ==> main goal: hide faults

* Active Replication
		--> Elke node doet het werk. Iedere node moet de request behandelen.
		De FE krijgt hier meerdere antwoorden terug (van de verschillende nodes). Dus heeft de FE een grote vrijheid hier. (We kunnen gewoon het eerste antwoord terug sturen[snelheid], wachten op alle antwoorden[zekerheid],...)
		Default methode is om te wachten op alle nodes om te antwoorden. (tolerantie op de Byzantijn fouten --> fouten door hardware)
		Iedere node moet in dezelfde volgorde de requests krijgen om SCS te bekomen!
		--> Geen lineairizeability! -> geen synchronized clock

Byzantine | failStop				|	Communicatiemethode:
----------|-------------------------|---------------------------------------
n/2-1	  |	n-1	 ==> active		    |	Total Order of Multicast (TOM)
----------|-------------------------|---------------------------------------
	X	  |	n-1	 ==> passive	    |	View Synchronous Group


### Highly available services:

Open file: just send to any of the replication Servers
Close file: send to all of the replication servers --> So change is on all of the servers

	==> read one - write all
	Path to consistency -->  R + W > N -> #reads + #writes > #nodes
		--> wanneer deze formule niet opgaat weten we dat er een fout is in de consistentie van het systeem.

		VolumesStorageGroup (VSG)
			--> group gaat over een aantal servers && volume over files
			Wanneer er een node uit de groep is (door server failure,...) sturen we toch de data naar de andere servers bij een write-all

		AvailableVolumesStorageGroups (AVSG)
			--> deelgroep van VSG van de nodes die beschikbaar zijn.


		Coda Version Vectors(CVV):
			Iedere node weet welke servers welke versie hebben. Zo kunnen we het hele netwerk up-to-date houden. Bij iedere write wordt het versiegetal verhoogd.


		--> Veel kleinere overhead dan de fault tolerance systemen, de communicatie is veel lichter.
		--> Dit kan omdat we niet echt zo hard bezig zijn met de fault-tolerance, er mag een kleine inconsistentie voorkomen. Deze gaat toch worden opgevangen over time.


## Intro to cloud computing

Case studies:

* Microsoft Azure
* Google App Engine --> gebruikt Java 1.7

### IaaS

### PaaS

### SaaS

### GAE: Task queues

Replication
    -> file that is always up-to-date
    -> we asume consistency
    <-> cashing -> file probably not up-to-date --> we hope for consistency

#### Single-copy-semantics

## Feedback sessions

RMI: Welke concepten zijn belangrijk?

* verschil tussen remote, serializable & local!
* Concurrency --> synchronization enkel wanneer nodig!
* Gebruik van naming service

Java EE:

## IOT

Consumer IOT vs Industrial IOT

Moorse law is niet van toepassing op:

* batteries
* antenna

### power constraint

--> altijd het touwtrekken tussen batterij en performance

LoRa --> long range 
Downstream: enkel net na een send van data!

TDSM --> mesh networking

### TCP for ioT?

neen:

* 3-way handshake
* TCP = statefull connection

Examen:

* core techical stuff

## Cloud computing

OPEX: operational expenses
CAPEX: up-front expenses

## EXAM

5 types van vragen
(Zorg de uiteg van top tot bottom uitgelegd word)

- overview
- detailed explanation
- small exercise
- explain your code (RMI / JEE)(5-7 punten op het examen)
- explain how to apply the learned technologies

--> IOT kan ook op het examen komen

Keuze van GAE kan nog op het examen!! (de vraag op het examen gaat over cloud computing, niet noodzakelijk over GAE!)

Gesloten boek examen!

