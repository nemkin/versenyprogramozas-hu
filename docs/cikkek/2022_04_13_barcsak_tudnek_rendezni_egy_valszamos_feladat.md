# Bárcsak tudnék rendezni (egy valszámos feladat)

Ebben a cikkben szeretnék egy kicsit bővebben a [Codeforces 1753C: Wish I Knew How to Sort](https://codeforces.com/problemset/problem/1753/C) feladat megoldásairól beszélni.

A feladatban kapunk egy bináris, $n$ elemű $a$ tömböt, amit rendezni szeretnénk, de sajnos az algel tanárunk elfelejtette megtanítani nekünk a rendezési algoritmusokat. :) Jobb ötlet híján egy randomizált módszert választunk, melyben egy lépés a következőképpen néz ki:

- Véletlenszerűen (egyenletes eloszlással, egymástól és a korábbi lépéseinktől függetlenül) rámutatunk két pozícióra a tömbben. Legyenek ezek $i$ és $j$ indexszel jelölve, ahol $i < j$.
- Ha rossz sorrendben vannak ($a_i > a_j$), megcseréljük őket, egyébként nem csinálunk semmit.

Ezt a lépést addig ismételjük, amíg a tömb rendezett nem lesz.

**Kérdés**: Mi a várható értéke a végrehajtott lépéseknek?

A feladatban technikai részlet, hogy a megoldást irreducibilis (tovább már nem egyszerűsíthető alakú) törtként kell kiírni, ennek a módszerére a végén térek ki.  

A feladatnak legalább kettő, egymástól nagyon különböző megoldása van, mindkettőt nagyon hasznos ismerni valszámos feladatok kezeléséhez.

## Első megoldási módszer: Várható érték linearitása



## Második megoldási módszer: Markov lánc dinamikus programozással


## Megoldás kiírása irreducibilis tört alakban
