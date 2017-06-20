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

# Relationele Calculus

# Programma's verbinden met een Database
Niet te kennen voor examen

# Functionele Afhankelijkheden

# Normalisatie

\part{Het Fysiek Model}

# Geheugen- en Bestandsorganisatie

# Indexeren

# Querryverwerking en Optimalisatie

# Transacties

# Concurrentiecontrole

# Herstel
