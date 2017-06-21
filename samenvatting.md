---
title: Samenvatting Databases
author: 
    - Robin Vanhove
date: Juni 2017
toc: true
numbersections: true
geometry: margin=2cm
header-includes:
    \usepackage[dutch]{babel}
    \usepackage{graphicx}
    \usepackage{subcaption}
    \usepackage[
	type={CC},
	modifier={by-nc-sa},
	version={4.0},
    ]{doclicense}
---


# {-}

Beknopte samenvatting voor het OPO Gegevensbanken.

\vfill
\hfill Versie \input{VERSION.txt}

\hfill Gecompileerd op \today
\doclicenseThis
\clearpage

\part{Conceptueel en Relationeel Model \& Query's}

# ER & EER 
Niet te kennen voor examen

# Relationele algebra
## Operatoren

\begin{center}
\begin{minipage}{0.4\textwidth}
\includegraphics[width=\textwidth]{graphics/operatoren.png}
\end{minipage}
\begin{minipage}[c]{0.4\textwidth}
\begin{center}
\begin{tabular}{ l r }
  Naam & Teken \\
  \hline \\
  Selectie & $\sigma$ \\
  Projectie & $\pi$ \\
  Hernoeming & $\rho, \leftarrow$\\
  Unie & $\cup$ \\
  Doorsnede & $\cap$ \\
  Verschil & $-$ \\
  Caresisch product & $\bowtie$ \\
  Join Operator & $*$ \\
  Deling & $\div$ \\
\end{tabular}
\end{center}
\end{minipage}
\end{center}

Fundamentele operatoren {$\sigma, \pi, \cup, -, \times$}, zijn de enige nodige operatoren. De andere kunnen er op gebaseerd worden.

### Selectie

$$\sigma_{\text{selectiecriterium}}(R)$$

Selecteerd een aantal tupels ui een rij $R$ meht het criterium. Het resultaat is een nieuwe relatie (tabel) met hetzelfde schema.

bv. 

- $\sigma_{ID=1}(USERS)$
- $\sigma_{color='red' \vee color='green' }(BOATS)$
- $\sigma_{AGE<50}(USERS)$

Selectie is cummulatief, dus $\sigma_a(\sigma_b(T)) = \sigma_{a\wedge b}(T)$

### Projectie

$$\pi_{\text{attributenlijst}}(R)$$

Een aantal kolommen uit een tabel halen.

bv.

- $\pi_{fist\_name, last\_name}(USERS)$
- $\pi_{color}(\sigma_{ID=1}(BOATS))$

### Hernoeming

$$RESULT \leftarrow \sigma_{Dno=1}(EMPLOYEE)$$
$$\rho_{RESULT}(\sigma_{Dno=1}(EMPLOYEE))$$

### Unie doorsnede en verschil

\begin{center}
\begin{tabular}{ l r }
  Unie & $\cup$ \\
  Doorsnede & $\cap$ \\
  Verschil & $-$ \\
\end{tabular}
\end{center}

Enkel op vergelijkbare relaties.

### Cartesisch product

$$Q = R \times S$$

Geeft als resultaat een nieuwe relatie die elke mogelijke combinatie van de twee tupels bevat.

### Join Operator

$$R \bowtie_F S$$

Is hetzelfde als een cartesisch product gevolgd door een selectie.

Er zijn meerdere soorten joins

- __Theta join__ een join waarbij de voorwaarde in de vorm is van $A \theta B$
    - Met $\theta = \{=, <, >, \leq, \geq, \not=\}$
- __Equi-join__, $R \bowtie_{a=b} S$
- __Natuurlijke join__, $R * S$. Een join waarbij de sleutel gebruikt wodt ip een conditie.

Een uitwendige join (links, rechts of volledig) is een speciale join die sowieso ieder element uit de (linker, rechter of beide) relatie, met en null als er geen paar gevonden is. Het teken hiervoor is een strikje met twee lijntjes in de richting van de join.

### Deling

$$T = R \div S$$

Tegengestelde van het cartesisch product. 

Bijvoorbeeld, voor welke zeilers bestaan reserveringen voor alle boten in een verzameling


## Aggregaatfuncties

$$_{groepering}\Im_{functies}(R)$$

Functies die op een verzameling waarden uitgevoerd worden. SUM, AVERAGE, MAX, MIN, COUNT. 

- Groepering is de verzameling van attributen waarop de groepering gebeurt
- Functies is de lijst van koppels (functie, attributt)

bv. $_{Dno}\Im_{AVERAGE Salary}(EMPLOYEE)$

# SQL

Structured Query Language

## Operatoren

Formularium van SQL beschikbaar op examen.

## Aggregaatfuncties

AVG, SUM, MIN, MAX, COUNT

Eerst groeperen met GROUG BY

### Aanpassingen

INSERT, UPDATE, DELETE

## Views

Een __view__ is een afgeleide relatie, dit wil zeggen dat de tupels niet expliciet worden opgeslagen. Implementatie op twee manieren

- __Query modification__, de query wordt aangepast voor hij wordt uitgevoerd op de onderliggende tabellen.
- __View materialization__, de afgeleide tabel (view) wordt aangemaakt en daarna wordt de query er op uitgevoerd.

Aanpssingen zijn mogelijk als de view maar uit 1 tabel (basisrelatie) bestaat en een pk bevat.

## Geneste Queries

Query in en Query.

IN, ALL, ANY, EXISTS

```
SELECT xxxx
FROM xxxx
WHERE xxxx IN (
    SELECT yyyy
    FROM yyyy
    WHERE yyyy
);
```

## Transactie

Een atomaire eenheid. Standaard is iedere query een transactie. Een transactie wordt als permanente aanpssing gezien.

START ... COMMIT

ROLLBACK kan gebruikt worden om een huidige transactie ongedaan te maken.

```
START;
UPDATE xxx SET xxxx WHERE xxxx;
UPDATE xxx SET yyyy WHERE yyyy;
COMMIT;
```


## Permissies

Verschillende database gebruikers kunnen ander rechten hebben.

```
GRANT right ON table TO user;
REVOKE right ON table TO user;
```

## Restricties

Een atribuut kan een primaire sleutel zijn (id). Gewoon uniek. Of een sleutel die naar een andere tabel wijst.
```
PRIMARY KEY <attr>
UNIQUE <attr>

FOREIGN KEY <attr> REFERENCES <table><attr>
```

Een atribuut kan een standaard waarde hebben. Zou niet 'null' mogen zijn. Of kan aan andere voorwaarde onderheven worden.

```
NOT NULL <attr>
DEFAULT <value>
CHECK <condition>
```

Algemene beperking opleggen met ASSERTION

```
CREATE ASSERTION <name> CHECK <cond>
```

## Triggers

Event - voorwaarde - actie

```
CREATE TIGGER <name>
{BEFORE | AFTER} <event> ON <table>
FOR EACH ROW
WHEN <cond>
    <action>
```

# Relationele Calculus

Relationele Algebra: __Hoe__
Relationele Calculus: __WAT__

- Tupelcalculus
- Domeincalculus

Gebruik van predikatenlogica.

## Queries

Een query is in de vorm van {t | formule(t)} of {t.A, t.B, ... T.z | formule(t)}

```
{ t.Bdate, t.Address | EMPLOYEE(t) and t.Fname = ‘John’ and t.Lname = ‘Smith’ }
{t | BOATS(t) and (t.color=‘red’ or t.color=‘green’)}
```

De kwantoren $\exists$ en $\forall$ zijn ook mogelijk.

# Programma's verbinden met een Database
Niet te kennen voor examen

# Ontwerp van een database

1. Hoogniveau moddellering (top-down)
    - (E)ER schema
2. Meteen een relationeel gegevnesschema (bottom-up)

Informatie bewaren, minimale redundantie ($\rightarrow$ maximale peformantie)

$\rightarrow$ __normaliseren__ 

## Informele richtlijnen

1. De betekenis van een relatie mot gemakkelijk verklaard kunnen worden.
    - Betekenis van een relatie moet duidleiljk zijn.
    - Betekenis van een attribuut moet duidleiljk zijn.
2. Redunantie en anomalieën vermijden.
3. Vermijd de waarde 'null'.
    - Niet van toepassing, ongekend.
4. Vermijd dat na equi-joins onechte tupels kunnen ontstaan. Vermijd attributen met dezelfde naam in verschillende relaties (als ze geen foreing keys zijn).

## Functionele Afhankelijkheden

Een __functionele afhankelijkheid__ ($X \rightarrow Y$) tussen twee (verzamelingen van) attributen X en Y is een beperking op de mogelijke tupels die gevormd kunnen worden. Voor twee tupels $t_1$ en $t_2$ is het namelijk zo dat als $t_1[x] = t_2[x]$ geldt dan ook $t_1[y] = t_2[y]$ geldt.

In andere woorden de waarden van de Y component van de tupel wordt bepaalt door de waarde van X. Of de waarde van de X component bepalen uniek (functioneel) de waarde van de Y component.

bv.

- Ssn $\rightarrow$ Ename
- Pnumber $\rightarrow$ {Pname, Plocation}
- {Ssn, Pnumber} $\rightarrow$ Hours


### Afleidingsregels

1. Reflexiviteitsregel $Y \subseteq X \Rightarrow X \rightarrow Y$
2. Uitbreidingsregel $\{X \rightarrow Y \} \models XZ \rightarrow YZ$
3. Transitiviteitsregel $\{X \rightarrow Y, Y \rightarrow Z \} \models X \rightarrow Z$
4. Decompositieregel $\{X \rightarrow YZ \} \models X \rightarrow Y$
5. Verenigingsregel $\{X \rightarrow Y, Y \rightarrow Z \}\models \{X \rightarrow YZ\}$
6. Pseudo-transitviteitsregel $\{X \rightarrow Y, WY \rightarrow Z \}\models \{WX \rightarrow Z\}$

$F^+$ is de verzameling die alle afleidingen die uit F volgen bevat.

bv. $F = \{ SSN \rightarrow ENAME, PNUMBER \rightarrow \{ PNAME, PLOCATION \}, \{ SSN, PNUMBER \} \rightarrow HOURS\}$

- { SSN $\}^+$ = { SSN, ENAME }
- { PNUMBER $\}^+$ = { PNUMBER, PNAME, PLOCATION }
- { SSN, PNUMBER $\}^+$ = { SSN, PNUMBER, ENAME, PNAME, PLOCATION, HOURS}

## Normalisatie
Een __normaalvorm__ legt bepaalde eisen op aan een relatie. Normalisatie is een relatie in een bepaalde normaalvorm brengen.

Elke volgende normaalvorm is een speciaal geval van de vorige.

Een __super sleutel__ of super key is een aantal attributen van een tupel die uniek zijn voor de tupel. 

Een __sleutel__ is een supersleutel waarvan geen attribuut verwijderd kan worden. Dus een sleutel is minimaal. Alle mogelijke sleutels zijn __kanidaat sleutels__ maar er is maar 1 __primaire sleutel__.

### Eerste Normaalvorm

Een relatieschema is in de eerste normaalvorm als het domein van elk atribuut is enkelvoudig (atomair).

Zo kan een attribuut geen lijst zijn. Maar moet er voor ieder element van de lijst een nieuwe tupel zijn.

\includegraphics[width=\textwidth]{graphics/NF1.png}

### Tweede Normaalvorm


### Derde Normaalvorm
### Boyce-Codd Normaalvorm
### Vierde Normaalvorm
### Vijfde Normaalvorm


\part{Het Fysiek Model}

# Geheugen- en Bestandsorganisatie

# Indexeren

# Querryverwerking en Optimalisatie

# Transacties

# Concurrentiecontrole

# Herstel
