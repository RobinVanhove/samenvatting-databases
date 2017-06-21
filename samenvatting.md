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

Beknopte samenvatting voor het OPO Gegevensbanken. Voornaamlijk gebaseerd op de slides, maar ook het boek.

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

Een relatieschema is in de __eerste normaalvorm__ als het domein van elk atribuut is enkelvoudig (atomair).

Zo kan een attribuut geen lijst zijn. Maar moet er voor ieder element van de lijst een nieuwe tupel zijn.

\includegraphics[height=6cm]{graphics/NF1.png}

### Tweede Normaalvorm

We spreken over een __partiele functionele afhankelijkheid__ of een __partial dependency__ als voor een afhankelijkheid $X \rightarrow Y$ geldt dat we een $X$ door $Z$ kunnen vervangen waarbij $Z \subset X$ en de afhankelijkheid $Z \rightarrow Y$ nog altijd geldig is.
Dus als we een attribuut kunnen weglaten is het partieel functioneel afhankelijk.

Anders spreken we over een __volledige functionele afhankelijkheid__.

Een relatieschema is in de __tweede normaalvorm__ asa ieder niet-sleutel attribuut volledig functioneel afhankelijk is.

In andere woorden voor elk niet-sleutel-attribuut moet de hele primaire sleutel nodig zijn om het te determineren.

\includegraphics[height=6cm]{graphics/NF2.png}

### Derde Normaalvorm

$Y$ is __trivaal functioneel afhankelijk__ van $X$ asa $Y\subseteq X$.

Een functionele afhenkelijkehid $X \rightarrow Y$ is een __transitieve functionele afhankelijkheid__ asa er geen $Z$ bestaat waarbij


\begin{minipage}{0.7\textwidth}

1. Z is volledig en niet-triviaal functioneel afhankelijk van X.

2. Z is geen (echte of onechte) deelverzameling van een kandidaatsleutel.

3. Y is niet-triviaal functioneel afhankeklijk van Z.

\end{minipage}
\begin{minipage}{0.2\textwidth}
\hfill
\includegraphics[width=\textwidth]{graphics/transFuncAf.png}
\end{minipage}

Een 2NF-reltieschema is in de __derde normaalvorm__ asa voor geen enkel niet-sleutelattribuut A geldt dat A _transitief_ functioneel afhankelijk is van een kandidaatsleutel. Dus alles is _direct_ afhankelijk van een sleutel.

\includegraphics[height=5cm]{graphics/NF3.png}

### Boyce-Codd Normaalvorm

De __boyce-Codd Normaalvorm__ (BCNF) geldt als er voor iedere niet-triviale functionele afhankelijkheid $X \rightarrow Y$ geldt dat $X$ een supersleutel is.

De BCNF haalt alle ongewenste functionele afhankelijkheden weg. Maar dit is niet steeds bereikbaar zonder andere problemen te creëren.

### Vierde Normaalvorm

\begin{minipage}{0.5\textwidth}
Een \textbf{meerwaardige afhankelijkheid} genoteerd met $X \twoheadrightarrow Y$ als er vier tupels zijn zodat 

\begin{itemize}
\item $t_1[X] = t_2[X] = t_3[X] = t_4[X]$
\item $t_3[Y] = t_1[Y]$ en $t_4[Y] = t_2[Y]$
\item $t_3[Z] = t_2[Z]$ en $t_4[Z] = t_1[Z]$
\end{itemize}
\end{minipage}
\begin{minipage}{0.35\textwidth}
\hfill
\begin{tabular}[t]{ c c c c }
    Tupel & X & Y & X \\
    \hline
    $t_1$ & a & $b_1$ &  $c_1$ \\
    $t_2$ & a & $b_1$ &  $c_2$ \\
    $t_3$ & a & $b_2$ &  $c_1$ \\
    $t_4$ & a & $b_2$ &  $c_2$ \\
\end{tabular}
\end{minipage}

bv. T-shirts hebben een _model_, _maat_ en _kleur_. Model $\twoheadrightarrow$ maat betkent dat:

- Voor elk _model_ wordt in welbepaalde _maten_ geleverd, onafhankelijk van de _kleur_.
- Dus elke combinatie van kleu en maat is mogelijk.

Een __meerwaardige afhankelijkheid__ $X \twoheadrightarrow Y$ is __triviaal__ asa $Y \subseteq X$.

Een relatieschema is in de __vierde normaalvorm__ asa voer iedere niet-triviale meerwaardige afhankelijkehid van de vorm $X \twoheadrightarrow Y$ van $F^+$ geldt dat $X$ een supersleutel is.

### Vijfde Normaalvorm


\part{Het Fysiek Model}

# Geheugen- en Bestandsorganisatie

# Indexeren

# Querryverwerking en Optimalisatie

# Transacties

# Concurrentiecontrole

# Herstel
