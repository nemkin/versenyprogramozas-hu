# Bárcsak tudnék rendezni (egy valszámos feladat)

Ebben a cikkben szeretnék egy kicsit bővebben a [Codeforces 1753C: Wish I Knew How to Sort](https://codeforces.com/problemset/problem/1753/C) feladat megoldásairól beszélni.

A feladatban kapunk egy bináris, $n$ elemű $a$ tömböt, amit rendezni szeretnénk, de sajnos az algel tanárunk elfelejtette megtanítani nekünk a rendezési algoritmusokat. :) Jobb ötlet híján egy randomizált módszert választunk, melyben egy lépés a következőképpen néz ki:

- Véletlenszerűen (egyenletes eloszlással, egymástól és a korábbi lépéseinktől függetlenül) rámutatunk két pozícióra a tömbben. Legyenek ezek $i$ és $j$ indexszel jelölve, ahol $i < j$.
- Ha rossz sorrendben vannak ($a_i > a_j$), megcseréljük őket, egyébként nem csinálunk semmit.

Ezt a lépést addig ismételjük, amíg a tömb rendezett nem lesz.

Kérdés: mi a várható értéke a végrehajtott lépéseknek?

