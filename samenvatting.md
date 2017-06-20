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
- __Natuurlijke join__, $R * S$

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
